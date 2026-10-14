# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

Here is a question from the engineering manager: Quick one, did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know?

The pings table on rook-database has last session's numbers. Read that alongside this code. Does it support the hypotheses we have defined, and does it help to answer why the pings have totally fallen off for the four? I feel like we are dancing around the same themes but there is no clear answer of the root cause and an obvious solution. And does that answer the engineering manager's question above?

### 2.

I feel like we may have already covered this, but succintly: Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.

### 3.

can you put this in a visual diagram, using Vesper as an example with real data after Aug 12. Next to that, show a diagram that walks through step by step exactly what would happen for Vesper to start getting work again.

### 4.

Help me complete this sentence in plain english, using Vesper as the example. Assume no code changes at this time - I think we would need to reset/lift  him off the floor otherwise he will now never get any more pings.

According to my findings in the code, for someone who's gone quiet, they would need to...
