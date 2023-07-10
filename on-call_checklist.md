---
title: On-Call Checklist
layout: layouts/base.njk
description: Did you get called for an issue? This checklist will help you make it through!
date: 2019-08-24
tags:
  - posts
---
Did you get called for an issue? This checklist will help you make it through. *Do it in order*.

1. If you were woken up, they can wait 2 minutes while you start some coffee. Don't make them wait much longer, but get that coffee started!
2. Join chat. Announce your presence, and what team you're here for
3. _Slow down_. Take a few deep breaths.
4. If this is an incident, make sure the Incident Manager (IM) explains to you *exactly the current understanding* of what's happening.
5. If this is not an incident, then *feel free to ask NOC to clarify* what they see as going on
6. Remember that you may be able to fix the problem, but _your biggest job is to route the problem to other people who can fix it_.
7. Ask questions to confirm that they're actually dealing with your technology, and not some other part of the stack.
  * If you *don't know what the thing is that is erroring*, call the secondary or tertiary. _A big part of the on-call job is routing info_.
  * If it's some other part of the stack, recommend the team or person whom you'd contact outside of an incident; NOC will handle finding the person on-call from that team.
  * If it's not your tech, share information to help prove it's not, but *stay on for at least a little while* while others confirm. _The longer you stay, the more you learn_.
8. Once you know it's your tech, ask questions to make sure you understand the details of the issue:
  * On what pages?
  * On what part of the app?
  * What actions have to be taken to cause the issue?
  * What, specifically, is the degradation of user experience?
9. If, at this time, *you don't understand what's going on* or *don't understand the technology,* call your backup (or the tertiary)
10. Make sure that you agree this is a real emergency. If you don't think the degradation is that serious, speak up and explain why. You may be right, you're the expert!
11. Take some more deep breaths. Look away from the screen. Make sure you have water/caffeinated beverage/etc. Take a sip. _Slow down_.
12. Remember that you may be able to fix the problem, but _your biggest job is to route the problem to other people who can fix it_.
13. If it is an emergency, find specifically what component is the problem.
14. As you're working, if you're unsure about anything, ask the IM, or anyone else whom you recognize/trust, offline. _Everyone wants to help_!
15. When looking for causes, note that, *during an emergency, only 2 things can be done*, so focus on solutions involving these:
  * Roll back a release, so look for changes in the most recent release
  * Override a config, so look for ways to turn off the problematic feature
16. If you have a fix you're confident in, _speak up_.
  * It's the IM's job to decide to do the fix or not, so give them the information!
  * Remember you're the most-qualified person, in this particular area
  * Propose the thing, then do it/make sure it gets done
17. If you don't have a fix you're comfortable in, _it's ok to ask for help_ - It's ok to act as a router, and not to directly fix
  * If asked to implement a solution, or for the information to implement the solution, and you can do it, do it! You don't need anyone's approval. Don't call someone on just because you're nervous. This is why you get paid the big bucks!
  * If you don't know the solution, make sure you at least know the problem and solution space, so that you can call in the right help
    * If during daytime, call on whomever you'd normally consider to be the SME for that area
    * After-hours, have NOC call the secondary or tertiary, you have backup!
  * Remember that you may be able to fix the problem, but _your biggest job is to route the problem to other people who can fix it_.
  * If you call in another team member to help, *make sure to stick around for the remainder of the incident*, and learn. Your goal should be to:
    * Learn enough to do it yourself next time _if this is one of the technologies we're responsible for_
    * Learn whom to escalate to, if not
18. Write up what you did in the on-call diary
