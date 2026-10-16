---
name: review-checklist
description: Reviews a product brief against my standard six-point checklist before it goes any further. Use when I say "review this brief", "run the review checklist", "check this brief", or point at a brief file and ask whether it is ready to move on.
---

# Review checklist

Run the same check every time on the brief I point you at. If I haven't named one, ask which file. Read the whole brief before judging anything.

## The six checks

1. **Owner.** The brief names a person who owns it (not just a team).
2. **Users and problem.** It clearly defines the user persona(s) and the problem we are trying to solve for them.
3. **Problem before fix.** It explains the problem before it proposes a solution. A solution appearing first, or a problem that is only implied by the solution, is a fail.
4. **Success measure.** It says how we'll know it worked: a metric, signal or observable outcome, ideally with a baseline or target. "Improve the experience" is a fail.
5. **Scope matches.** The scope stated at the end matches the scope stated at the start. Compare them item by item and list anything added, dropped or reworded so it means something different.
6. **Readable by anyone.** A person with no context on the product could pick it up and understand it. Flag unexplained jargon, internal names, acronyms and implementation detail that the reader needs to already know.

## How to judge

- Mark each check **Pass**, **Partial** or **Fail**.
- Support every mark with a short quote or section reference from the brief. Don't mark Pass on a guess; if the brief is silent, that is a Fail.
- For Partial and Fail, say in one sentence what is missing and what a fix would look like. Don't rewrite the brief.
- Review only what is on the page. Don't fill gaps from other files or from what you know about the project.

## Output format

```
Brief: <title / file>
Verdict: Ready / Ready with fixes / Not ready

1. Owner: Pass | Partial | Fail
   Evidence: "<quote>" or "not stated"
   Fix: <only if not Pass>
... (repeat for all six)

Top things to fix first: <up to 3, most important first>
```

Verdict rule: **Ready** if all six pass; **Ready with fixes** if there are only Partials; **Not ready** if any check fails.

Keep the tone plain and direct. End with the verdict and the fixes, not a summary of the brief.
