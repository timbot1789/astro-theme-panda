---
title: PR Review Standards 
pubDate: 2024-04-04
categories: ['Tech']
tags: ['project management', 'coding']
description: ''
---

This is a document I wrote for an open source project I am managing. I hope other people may find it useful.

# Do Not Harm the Code
New code must improve the codebase.
- It is the responsibility of the PR creator to **_demonstrate_** how the changes improve the codebase.
- It is the responsibility of the PR reviewer to **_validate_** that the changes improve the codebase.

## Development and Code Submission Process
##### What to do if you're creating a Pull Request
1. Pull the most recent version of Development.
2. Create a new feature or bug branch off of Develop to do your work in.
3. Push your code up to the repo.
4. Create a PR and wait for the automated tests to run[^1].
5. If the automated tests fail, fix what's causing the failure and push up the changes.
6. Once all automated tests pass, request reviewers. **If the automated tests are not passing, your PR will not be reviewed.**
7. Respond to any feedback you receive from reviewers in a timely manner.
8. Once you have one approval and no change requests, you may squash and merge your code into Development. Congratulations! You have meaningfully improved the project.

If you have any questions, like you don't understand why a certain test is failing, or you don't know who to request reviews from, feel free to reach out on Discord in the PASS Developers thread.

### Tips
- **Focus your PR on one feature or bug**. Your goal is to demonstrate that your code works. That's easiest to do if all your changes are related to one thing
- **Keep PRs small (<1000 additional lines of real code)**. Your goal is to demonstrate that all your changes work and don't harm the system. The fewer changes you have, the easier that is to do. If you find yourself changing thousands of lines of code, consider breaking up your change into multiple PRs.
- **Add testing steps and screenshots to your PR description.** You want to make it clear to your reviewer what your code is supposed to do. A good way to walk them through that is include clear testing steps. 

### Testing
Part of demonstrating that your code works is writing unit tests. Each PR must include a unit test for every file you've added. And you must include a [happy path test](https://en.wikipedia.org/wiki/Happy_path) for every major new behavior you have added. These do not need to be overly complex. For example, if you add a new React component, write a test that demonstrates that the component renders[^2].

You may find that implementing a new feature requires breaking existing behavior, and changing a unit test. If that is the case, add a comment to the PR explaining the situation, and discuss it with your code reviewer.

Additionally, we run test coverage checks on the system. We do not require 100% test coverage. However, we do require that new code does not _decrease_ the overall project test coverage.

## Code Review Process
##### What to do if you're reviewing a code change

You do not need to review PRs that do not pass the automated tests.[^3]

1. Learn _what_ the PR does. Make behavior understandable.
    1. **Understand the problem this PR is trying to solve.** Why is the PR necessary? Is it fixing a bug? Adding a desired feature? Refactoring code? Responding to an Issue? If there's an Issue, read it and any comments on it. Is the problem a meaningful issue worth solving? If you don't understand the problem, ask the creator to describe it more.
    2. **Try briefly to solve the problem yourself.** Take about 15 minutes and think about how you'd solve the problem. Maybe try writing part of the solution code. Coming up with your own approach with help you better critique the PR creator's approach.
    3. **Test the PR.** Does the PR actually do what it claims to do? If you don't know how to test it, or you don't understand the behavior you see, ask the creator. Smoke test existing behavior to make sure nothing is unintentionally broken.
    4. **Suggest behavior improvements.** If your testing found confusing behavior or bugs, request fixes for them. Once behavior is clear, move onto code review.
2. Learn _how_ the changes do what they do. Enforce good design.
    1. **Outline the source code solution.** Look over what major components are affected by the change. Is this about what you expected from your quick solution? If not, what's different, and why?
    2. **Look over each file.** Check for duplicate code. If the creator added a function that already exists in the repo, suggest they use that function instead. Check for hard-to-find bugs. Network errors, edge cases, etc.

3. Approve, Request Changes, or Leave a comment.
    1. If you're confident that the code solves the problem it claims to solve, and the code improves the code base, **approve the PR.** If there is still follow-up work, talk with the creator and document that work. E.G. if the PR is connected to an issue, and only partially solves it, note on the issue that work is still left and is still open to development. If the PR affects multiple open issues, update all issues appropriately.
    2. If you find that the PR does not improve the codebase, **request changes on the PR**. Does it fail to solve the problem? Does it introduce new bugs to other parts of the system? Is the code impossible for other maintainers to understand or work with?
    3. If you can't say one way or the other, or you just want to note something, leave a comment.

### Tips
- **Don't take long on PR Reviews.** If work is sitting in code review for weeks or has a lot of back and forth in comments, have a meeting with the creator and other reviewers. Maybe the PR can be broken up into several pieces. Part of the PR can be let in, while the other parts wait for later. Or maybe it's best to close the PR and start again with a clean slate. Nothing wrong with that. Failing fast is part of the development process.
- **Don't leave unnecessary comments.** A lot of comments on a PR can be overwhelming for the creator. If a PR already has 2 reviewers on it, don't review it unless you're asked to, or you notice a critical flaw that the other reviewers have missed.
- **Don't leave duplicate comments.** Before leaving a comment, look over the description provided by the creator and comments left by earlier reviewers. Don't duplicate comments that have already been addressed.
- **Don't use PR review to discuss broad design goals.** Use Issues for that. If you notice a complex PR with no attached issue, ask the creator to make an issue too, and describe things there. This provides you a more flexible forum to talk about design without getting bogged down in code. Simple PRs where the problem and solution is obvious don't need issues.

### Closing PRs
PASS is a public project that receives submissions of varying qualities. It is possible to receive a PR that cannot be improved through normal review, and it is best to close the PR to start again. This should only happen if a PR is fundamentally flawed in some unfixable way, or if the creator seems to have abandoned the PR without fixing its issues. Some examples of when this could happen:
1. The PR is clearly spam from a robot.
2. The PR contains no changes.
5. The PR has fixable bugs, but the PR creator has not responded to comments on github or discord for several weeks. The work on that branch may be reopened under a new owner.
3. The PR is based off a very old version of PASS, and the changes aren't compatible with the current version.
4. The PR is a duplicate. It copies work or features already implemented by another PR.
6. The PR changes are fundamentally incompatible with PASS's goals and mission. e.g. a PR titled "Remove SOLID, replace with a PostGRES server hosted on Google Cloud".

If a PR meets these requirements, close with a comment explaining why it must be closed, and how the PR creator can improve their next submission. **If the creator is a project member, try contacting them before closing the PR yourself.**

## Further Reading
Recommended reading on PR review processes, for both contributors and reviewers:

- [Github's guide to code reviews](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/reviewing-proposed-changes-in-a-pull-request)
- [A thoughtful blog post on successful code reviews](https://rewind.com/blog/best-practices-for-reviewing-pull-requests-in-github/)
- [A discussion forum with insights from several experienced developers](https://softwareengineering.stackexchange.com/questions/381343/how-to-make-better-code-reviews-when-pull-requests-are-large)


[^1]: If this is your first time contributing to the project, you will need a maintainer to start the automated tests for you. You can run all automated tests locally. It is highly recommended that you do so _before_ pushing your PR.
[^2]: More complex React behaviors, or new behaviors added to existing components, may require more tests.
[^3]: An exception is for a PR by a new contributor. Their PRs are not allowed to trigger actions by default. If you see a new contributor's PR, glance over it to check that it's broadly OK. Then approve the test run. After the tests pass, continue your review.
