---
title: How to Start a Ticket
description: With just an hour or two of work up-front, it's possible to avoid many of the reasons stories become frustrating and unpleasant. Don't get your PRs kicked back; do your work up front.
layout: layouts/base.njk
date: 2023-05-12
tags:
  - posts
---
# How to Start a Ticket

With just an hour or two of work up-front, it's possible to avoid many of the reasons stories become frustrating and unpleasant. Don't get your PRs kicked back; do your work up front.

1. **Get All the Answers.** Product and UX's job is to help you resolve all the edge cases. Make sure you understand:
  * What to do if operations fail (bad user input, bad API response, bad login state, etc.)
  * What device types you need to make this work on
  * Confirm that any changed text, links, etc., is actually meant to change and not a typo
2. **Did someone else do it before?** If your goal is not to create something totally new in the codebase, then it's possible that others have done something similar before. Even if you're doing something new, it may be that other people have had to deal with the same interfaces in completing their tickets. Searching completed tickets for some of the keywords in your ticket can often reveal past pull requests that can act as signposts on your path, or even give you some of the code you need. It's not cheating, it's understanding the codebase and applying past best practices.
3. **Assess ways to do the ticket.** Read the code and understand how things work in the module (if you're developing something from scratch, find similar things and understand how they work). If you want to discuss your approach with others, one option is to outline multiple options in ticket comments and direct the discussion there. It may also help to draw pictures at this stage.
3. **Consult a Colleague.** Ask someone what they think of your approaches. Senior and Principal Engineers are specifically paid to do these things, so don't be shy about asking them. If you're not co-located, tag them in the ticket comments (make sure to communicate out-of-band so they expect it). Agree on a preferred approach.
4. **Go to a Domain Expert.** If another team often works with this kind of code , then reach out to validate your approach.
5. **Document the Chosen Approach.** Describe the approach in the ticket comments. This comment doesn't need to be all-encompassing, but, if someone finds the ticket in 8 months and needs to do the same thing, they should be able to get a good start from your comment.


## FAQ

### Do I need to do this every time?

No, it's certainly too much effort for 1-day tickets, but the effort's worth it for anything you'll work on for longer.

### Seriously, this seems like it would make tickets take longer

What really take a long time, both in terms of how much of a given sprint it takes up and, maybe more importantly, in terms of _how long it feels_, is when a ticket gets kicked back multiple times from code review. 

Think of this as a way to push all your mistakes to early in the process, when they can be redone cheaply:

* Changing your mind in a conversation is easier than changing code
* Drawing a new line or a new entity on a whiteboard is easier than changing code

Don't let yourself get bogged down, but, if you expect a ticket to take a few days, what's the downside of spending a couple hours up front?

### Shouldn't the organization trust me to plan my story on my own?

Sure, and they also trust you to execute the plan and write code. But most orgs also have code review, because we're all, together, smarter than any one of us, individually. Think of the above as "idea review".

### I don't want to ask dumb questions of Senior Engineers, that makes me feel dumb.

Answering those questions is literally in their job description, and they're literally evaluated on how well (and positively!) they answer them. You should never be made to feel dumb. If you are, let your manager know.

### So I can ask any dumb question, then?

Ask your best question. Do enough research that you're comfortable you have a specific question that, when answered, will lead you forward. Some advice on asking good questions: [1](https://stackoverflow.com/help/how-to-ask) [2](https://medium.com/@dan_abramov/asking-good-questions-421f08ee7e5c), [3](https://medium.com/@gordon_zhu/how-to-be-great-at-asking-questions-e37be04d0603), [4](http://www.catb.org/~esr/faqs/smart-questions.html), [5](https://codeblog.jonskeet.uk/2012/11/24/stack-overflow-question-checklist/).

Your best question probably expresses less understanding of a topic than a subject matter expert would have. If you knew all that SME knew, you wouldn't have a question (and you'd be the SME). Don't worry that you know less.

### What do I do if I get an answer?

Document it! In the ticket comments, if it's trivial; if you think it would be useful to other people, in the permanent documentation, such as a wiki or generated docs. If you're not sure, you probably have your own page on a wiki, so put it there. Documenting things you learn helps you not have to ask the same questions again and again, which makes you faster and happier. Soon, you'll be the expert!

### That's a lot of documentation!

Would you rather write a few comments or have people keep asking you what's going on? Thought so.