# 05 · Super Speed — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent three sessions finding out what went
wrong: the two piles of feedback that didn't agree, the numbers that
hid four people inside an average, and the code that settled it —
the shorter timeout is what did it.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

Using 05-super-speed/director-request.txt and rook-wiki as your primary sources/context help me write a product brief to solve the quiet responder problem. Clearly identify the user persona, problems we re trying to solve and a proposed solution. The solution is not simply to extend the response time from 60 to 90 seconds. Think about the handler's user experience, the math that currently prevents a quiet responder from ever digging out of the hole, and also how the handler can monitor the behavior to continuously improve the process.

### 2.

sorry i meant to say the responder is the primary user persona. but i like the ideas for the handler as well. please rewrite with responder as primary

### 3.

i haven't even read the second brief but the first one is too long. the final product should be a 1-pager.

### 4.

can we make the brief more digestible/applicable by using the responder I analyzed yesterday. Illustrate how the solution would have helped them in particular, and what other scenarios or edge cases the proposed solution will catch. also state what it will NOT solve for. the key is that both responder and handler have visibility and a simple way to test/troubleshoot and re-right the ship when they start going quiet

### 5.

as we iterate this doc it feels like we are drifting further away from a solid 1-page product brief. there should be (roughly) an executive summary, user personas, problems we re trying to solve, proposed solution and how we know it worked. i want to understand what will change for handlers and responders once it ships, and what this fix will not necessarily solve

### 6.

rename quiet-responder-brief.md to brief.md and save it in 05-super-speed

### 7.

great start. The handler should have the ability to message or somehow notify the handler if they see 'We've noticed you've been missing pings. no action is not helpful. there should also be a mechanism for the responder to contact the handler for another (general) reason

### 8.

is this allowing us to differentiate between missed and declined? also the responder app should have an option to account for seeing the ping but didn't get to it in time. any action taken by the responder should be logged on the handler console.

What is the meaning of "Nothing to do. Two cards...." for The Gale? This is not helpful

### 9.

after the responder has tapped a response, it should disappear from the phone. things are starting to look very busy. I don't think handler needs the Why: - too much detail

### 10.

i don't even see the unreda badge? what is the ranking score bar trying to achieve and what can the handler do about it?

### 11.

when the handler sends a message to responder, the responder needs a way to acknowledge/reply. Agree - remove the ranking score bar. What else is missing - or is extraneous - to enable clear comms and visibility to issues with the ability to action by both parties? tell me your suggestions before updating the prototype.

### 12.

what is the point of simulate 3 pings taken

### 13.

should there be a way to cler out the conversation and activity? perhaps save in an audit table but remove from the console after X time

### 14.

reset demo is not working
