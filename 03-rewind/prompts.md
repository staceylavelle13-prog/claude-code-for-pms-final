# 03 · Rewind — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly.

Last session you read four conversations and every support ticket
since 4.2 — and found the two piles did not agree.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

show me a visual of missed pings, taken, turned down by responder

### 2.

can we correlate the missed pings to time of day or geographic location/proximity to the incident?

### 3.

Is there any other data related to handler performance relative to their designated responders? Are the handlers dedicated over history to the same responder, or are there changes in handler to responder relationships and/or behavior that might explain the misses for Undertow, Farlight, Meteor Mite and Vesper? Show me data with percentages of change

### 4.

Of all the analysis we have done - or something I may have missed - what is the most compelling and meaningful number or percentage in all the data that explains the massive drop in pings for those 4 people

### 5.

Compare the support_tickets table on rook-database against the pings table. Do they agree with each other? When did people start writing in, and when do the numbers actually move? Look at other fields in the database and pick out patterns or outliers that may help us understand what is happening?

### 6.

Is there any data about the phones the responders are using, or other hypotheses about the underlying technology the responders have access to (wifi, cell signal, phone)?

### 7.

what do you think is the root cause of the sharp increase in missed pings for the four?

### 8.

can you show me ravi's time to accept by responder data

### 9.

how do we map the people reporting the tickets to the people experiencing the biggest issues from the data?

### 10.

Pick one responder from the file who went quiet AND had no tickets (Meteor Mite or Vesper). Show me every week for them, how many times we pinged them, how many they took. Then tell me what happened to that person, week by week, in plain English. I want to hone in on every possible detail about this one handler and responder to understand what is going on with the four.

### 11.

What would need to happen for a person who has gone quiet to start getting pinged again? The declines don't seem to be the issue, as you said the declines are being logged well within the 60 second time allowance. So it maybe to do with missed and the second person who is pinged if the first person ignores/misses?

### 12.

Tell me what is the correlation and percentages of taken/missed/declined when the responder is the first choice or the second (or beyond) choice. Is the wait time 60 seconds for second pings as well?
