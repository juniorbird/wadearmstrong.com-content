---
title: Sample Code Review Checklist
description: A good code review starts with a good pull request. Before you find yourself a reviewer for your code, do this.
layout: layouts/base.njk
date: 2019-10-02
tags:
  - posts
  - things
---
# Sample Code Review Checklist

## For the Developer

### Developing Good Code

* Your code should comply with the Code Standards.

### Before You PR

A good code review starts with a good pull request. Before you find yourself a reviewer for your code, make sure:

* Things that need docblocks have docblocks, and existing docblocks are up-to-date
* Existing tests pass
* All new code has unit tests
* You've removed `console.log()` and `var_dump()`
* There's no commented-out code
* Your code builds successfully
* Your code does what it says on the label (if you have any concerns whether or not your code works, get someone from QE to educate you on the use cases you're developing for)
* You pulled from master, and resolved any merge conflicts
* You cleaned up your git timeline (commit, and even push, often; but only keep meaningful commits, that are also safe to roll back to, in the log)

If you haven't done all of the above, then you won't get a code review -- your work will just get kicked back to you. Spend your time, and your reviewer's time, well, and do all of these first. Ask for help if you need it.


### After You Open Your PR, but Before You Get a Reviewer

* Have you marked up your PR in order that people who haven't worked with the problem can easily understand your code?
  * All non-obvious pieces of code have a note on what they do and why
  * If particular pieces of code match to particular requirements, explain how
  * Any kludges are explained, especially why this kludge is actually the best option
* If any TODOs are left, make sure you've opened tickets for them, and comment on them in the PR (on specific lines that make the need for these new tickets clear, if possible)


### Getting a Reviewer

* Pick one randomly
* Don't review your own ticket
* If you think an SME should see your ticket, tag them
* Make sure your reviewer(s) know they're on the hook

## For the Reviewer

### Before You Review

* If you're the named reviewer, it's your job to make the review work:
  * All PRs take 2 approvers; if there's only 1 assigned, get another
  * If you know of an SME for the codebase, see if they'll do a review
  * Once you and another person have signed off, move the ticket to Pre-UAT
* Block out at least 1 hour
* Read the ticket and make sure you understand the requirements
* You should be able to trust that the code is fit for purpose, so spend time reviewing the written code rather than exercising the code in execution. But, if in doubt, pull and run the code.

### Early Returns are Good, Even In Code Review

If any of the below are true, mark the code as Needs Work and bounce it right back:

* Existing tests don't pass
* No tests for new code
* Tests are unreadable
* Merge conflicts, no matter how trivial
* Git log is a mess
* Lots of `console.log()` or `var_dump()`
* Lots of commented-out code

### Giving Good Code Review

* You should be able to understand how the code works, and why, by the end of your review
* Ask any questions you have within Stash; the other reviewers may have the same thought
* No comment or question is too small, but
* Read the code before asking how something works, even if it's code from another module
* Do the math/logic to make sure that all permutations of code flow are being tested
* Attend to the Code Standards as you look at the code; you may want to keep a checklist

## Further Reading

* [Fog Creek's 'Stop More Bugs with our Code Review Checklist](https://blog.fogcreek.com/increase-defect-detection-with-our-code-review-checklist-example/)
* [Jeff Atwood's 'Code Smells'](https://blog.codinghorror.com/code-smells/)
* ['Creating Your Code Review Checklist' from SmartBear](https://blog.smartbear.com/development/creating-your-code-review-checklist/)
* [10 Principles of a Good Code Review](https://dev.to/codemouse92/10-principles-of-a-good-code-review-2eg)
* [Giving and Receiving Great Code Reviews](https://dev.to/samjarman/giving-and-receiving-great-code-reviews)
* [Top 10 Pull Request Review Mistakes](https://blog.scottnonnenberg.com/top-ten-pull-request-review-mistakes/)
