---
title: Solid calendar App Analysis
pubDate: 2023-04-04
categories: ['Tech']
tags: ['solid','github']
description: ''
---

The [Solid Calendar app](https://github.com/timbot1789/solid-calendar) requires the ability to find all calendar events in a pod that occur over an arbitrary, end user-defined time interval. It also requires the ability to share individual events with other solid web IDs without sharing an entire calendar.

The Solid calendar app manages a list of "events" in a user's pod that are based on the [schema.org event schema](https://schema.org/Event). All events happen over an interval of time, described by their `startDate` and `endDate` properties (these properties can contain both date and time information, and may span any amount of time, even years). The Solid calendar app then displays all events that occur over a certain time interval, like a given month, a given week, or a given day. The user may choose to change this time on demand, and the calendar will display a new set of events. The user may create new events on their calendar, or edit existing events. The user may also share events with other people, which grants the recipient read privileges and limited edit privileges to the event.

The set of events displayed can be represented by the following SQL query:

```SQL
SELECT * from EVENT WHERE (EVENT.startDate >= @IntervalStart AND EVENT.startDate <= @IntervalEnd) OR (EVENT.endDate >= @IntervalStart AND EVENT.endDate <= @IntervalEnd) OR (EVENT.startDate < @IntervalStart AND EVENT.endDate > @IntervalEnd);
```

Solid's current system only provides limited ways to organize and retrieve such events. Most of the ways I know about result in fetching either too many events and ignoring many of them (overfetching), or fetching too few and not displaying all relevant events that occur in a timeframe (underfetching). Some also require a large number of network fetches, which has performance implications. The ways I know about are:

1. _Store all events in one single "calendar.ttl" file_. This is an efficient fetch, but it means that individual events cannot be shared. The user can only share their entire calendar. It also results in overfetching, as a person's calendar may span several years, but they're usually only interested in seeing a week or a month.
2. _Create a "calendar" container and then create an "event-{uuid}.ttl" file for every event_. This allows individual events to be shared with other users, but as there is no way to determine when events occur without fetching them, it requires the app to fetch all events in the container, which is an overfetch. It also is rather slow. This is the approach the calendar app currently implements, and there are noticable load times for calendars with more than 100 events when the app is on the same machine as the pod server. 
3. _Create a "calendar" container, and create a series of containers underneath it for each month, which then contain the event.ttl files that occur in that month, based on their start time. Then fetch all events in the container for the month that the user is viewing_. This results in an underfetch if an event spans multiple months. If an event starts in january, and ends in march, then the event should be displayed when fetching january, february, or march calendars. However, it would only be retrieved when fetching january. In order to guarantee a proper calendar display, the app must fetch all events, which then means this case devolves into 2. Copies of the event could be stored in the february and march calendars, but that makes data management more complicated. It also results in an overfetch if the user wants to view a certain week or day, instead of an entire month.
4. _Create a "calendar" container and then create an "event-{startDate}-{endDate}-{uuid}.ttl" for every event_. Same as 2, but include the start and end time in the file name. This allows the app to only fetch the calendar container, and then decide based off the file names which files it needs to fetch. This avoids both underfetching and overfetching. However, it makes editing the start or end time of an event more complicated as it now functionally requires deleting an old event and replacing it with a new event. It also makes data privacy weaker, as a person with access to the calendar container, but not the individual event, can still see the event's start and end times.
