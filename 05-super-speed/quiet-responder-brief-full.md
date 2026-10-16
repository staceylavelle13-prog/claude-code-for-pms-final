# Product brief: Quiet Responders in Rook Dispatch

**Author:** Dispatch PM · **For:** Helen Achebe · **Status:** Draft for discussion, nothing built yet
**Sources:** Helen's request (05-super-speed/director-request.txt), rook-wiki (Rook Dispatch, Glossary, 4.2 release, September handler interviews, Handler Phone App brief, roadmap), `dispatch-routing` code, and our own analysis of rook-database.

---

## 1. Summary

Since 4.2 (12 Aug), a handful of responders have gone quiet. Their phones stop buzzing, they can't tell why, and the system gives them no way back. Engineering can change a number this afternoon. That would shorten the hole, not remove it. This brief proposes what a quiet responder should actually experience, what their handler should see and be able to do, and how we'd know it's working.

**The proposal in one line:** a responder who goes quiet should be told, should not be punished for pings that never had a fair chance to be answered, and should have a guaranteed route back to work. Their handler should be able to see it and help.

---

## 2. User personas

### Primary: the quiet responder ("Vesper", "Meteor Mite")

| | |
|---|---|
| **Who** | An independent masked responder who receives callouts on their phone and decides whether to take them. Identified to us only by working handle, capability tags, availability windows and callout history. Usually one responder per area. Often not at their phone: upstairs, getting ready, out of signal. |
| **Job to be done** | Get offered the callouts I'm available and capable for, and have a fair chance to answer before they go. |
| **What they experience today** | Silence. Their ping volume falls from about 12 a week to 1 to 6. When a ping does arrive it may be gone before they reach the phone. They never see a ranking, a score or a reason. |
| **Pain** | Nothing explains the quiet: is the phone broken, was I forgotten, is it me? Meteor Mite has been texting Kip all week asking if something is broken. Vesper has missed his last two pings and can't know he is at the floor. |
| **What they can do today** | Nothing. There is no way to see why, check their phone is reachable, or ask to be put back in rotation. |
| **What good looks like** | I know when I'm quiet and why. I get a fair window to answer. If I've been out of the loop, the system brings me back in without me having to prove myself first. |

**Evidence gap:** every quote we have is from handlers, not responders. The September interviews were handlers only. Before build, someone (Sofia) should speak to two or three quiet responders directly. Handle names only, no identity questions.

### Secondary: the handler ("Kip", "Aunt Dot")

| | |
|---|---|
| **Who** | A handler who manages one to a few responders from the web console, usually at night, with the console competing with other screens. Kip handles Meteor Mite and The Gale. Aunt Dot handles Vesper. |
| **Job to be done** | Keep my responder working and make sure callouts reach them. I'm their only line of sight into what Dispatch is doing, and the first person they come to. |
| **What they see today** | A coverage card per responder with status and capability tags. No ping history, no trend, no reason. |
| **Pain, in their words** | *"Two cards on the same screen that might as well be two different products."* *"I just want it to look less like a coincidence when I'm staring at it at midnight."* *"Mite thinks Mite's been forgotten."* (Kip) |
| **What they can do today** | Almost nothing. Routing config ships with the release. They can only say "hang in there". |
| **What good looks like** | I notice a quiet week within a day, I can see why, and I can do something about it without filing a ticket. |

**Constraint on both:** nothing in this brief needs, or may assume, a link from a cover to a legal identity (Security Policy 4.1; read it before build).

---

## 3. Problems we are solving

Evidence is from our database analysis (pings and tickets, 29 Jun to 7 Sep) and the September interviews. Items marked *unknown* are not yet established.

**P1. The responder is punished for silence as if it were refusal, and never forgiven.**
`history.py` charges the same −0.12 for a missed ping and a turned-down ping, and +0.08 for a take. Nothing ever decays (the 2019 TODO was never built). Wen's own note asks whether this is right.

**P2. There is no way back, which is the math problem.**
- The score only moves when someone is pinged. A low-ranked responder is pinged only when everyone above them fails, so they get few chances to earn points.
- Break-even is about 60% taken. Below that, the score sinks. At the floor (0), it takes about 7 takes in a row to reach 0.56.
- Vesper, from a start near 1.0 on 12 Aug: 17 pings to 1 Sep, 5 taken, 3 turned down, 9 missed. Floor by 1 Sep. At 1 to 2 pings a week, seven perfect yeses is a month or more.
- At his observed mix (29% taken, 18% declined, 53% missed), the average ping costs him about 0.06. The hole deepens even while he is still being asked.

**P3. A "miss" is not one thing, and we can't tell which kind it is.**
Four responders (Vesper, The Undertow, Farlight, Meteor Mite) went from about 12 pings a week to 1 to 6. Their post-4.2 pings were 58.9% missed against 13.9% for everyone else, though they were not decliners (turned down 21.6% before vs 20.9% for others). Declines are answered in 6 to 40 seconds, so these misses look like no answer at all: perhaps late or no delivery, perhaps no signal (ticket 3014). *Unknown:* what the trigger was on 12 to 14 Aug.

**P4. The responder never gets a fair race.**
First pings were missed 2.7% of the time before 4.2 and 21.4% after. The four responders' first pings after 12 Aug were 70% missed and 7.5% taken. Handlers describe it from the side: *"it didn't used to feel like a fair race, phone to stairs"* (Dot), *"one boot on"* (Ambrose). The ping wait went from 90s to 60s in the same release as the weighting change, so we can't separate the two from release notes.

**P5. Handlers are blind to it, so no one can help.**
- Nothing on the console shows ping volume, ranking or why a responder went quiet. About 39 "quiet / never goes off" tickets, 38 after 12 Aug. Yet Vesper and Meteor Mite have filed none, so tickets under-report it.
- Handlers learn a callout is live late or by accident (Dot hears a buzz through the ceiling; Ambrose heard from the responder, not the console).

**P6. Nobody is watching the shape of the thing.**
Acceptance rate is reported weekly in aggregate. That is how four people hid inside an average. Callouts with no taken ping rose from 5.5% to 11.1%. No metric, owner or alert exists for "someone has stopped getting work".

---

## 4. Why not just move 60 s to 90 s

- The ping wait is a real contributor to "gone before I could answer", and worth testing. But it doesn't touch the hole in P2.
- Restoring 90 s wouldn't reset the four. Their scores are already at or near the floor, and with no decay and no forgiveness they stay there.
- The four's misses aren't slow answers (declines are fast and unchanged). More time may not help responders whose phones aren't being reached.
- It treats every responder as identical, though ticket 3014 shows signal matters.

Treat the timeout as one lever inside the design below, not the design.

---

## 5. Proposed solution

Four parts. A and B are what the responder lives with. C is what the handler gets. D is how we keep it honest.

### A. A fair race, and a heads-up (responder, phone)

- **Reminder at half time.** A second, distinct notification when about half the ping wait has passed ("Callout still open: 30s"). Hypothesis to test, not a certainty.
- **Late-claim window.** If a ping times out and the next responder hasn't answered yet, the first can still take it for a short grace period. First tap wins, and both phones show what happened. Replaces "gone before I could answer" with something that feels fair. Needs engineering to confirm the assignment race is safe.
- **Delivery receipts.** The phone confirms "received" and "opened". This is the data we lack (P3), and it is what separates the two kinds of miss.
- **A "You've been quiet" notice.** When a responder falls below their normal volume, they get a plain message: "We haven't sent you much lately. Check your phone is reachable. You can ask your handler to run a test." No scores, no blame.
- **A re-entry notice.** When they come back into rotation: "We're sending you a few callouts to get you back in. If your phone isn't getting them, tell your handler."

### B. A guaranteed way back (responder, routing)

Everything here is a suggestion for Wen Li and Marcus to size. The numbers are illustrative and need replay testing against the ping history before anyone ships them.

| Rule | What changes | Why |
|---|---|---|
| **Split the outcomes** | A miss costs less than a decline (e.g. −0.04 vs −0.12). A ping not delivered or never opened costs nothing. | Silence isn't refusal. Break-even for a responder who only misses falls from about 60% taken to about 33%. |
| **Decay toward neutral** | After N days without a ping, the score drifts toward 0.5 and stops there. It never rises above neutral on its own. | Resolves Wen's 2019 TODO both ways: a bad month is forgiven, but nobody is promoted for doing nothing. |
| **Minimum offers** | A responder with no pings in 7 days, eligible by tags, availability and distance, is guaranteed a ping on a qualifying callout. Never on a callout flagged urgent, never ahead of anyone with a clear proximity lead. | Gives the score a chance to move. Without offers, no rule above can work. |
| **One-time reset for the four** | Reset Vesper, The Undertow, Farlight and Meteor Mite to 0.5 at launch. | Recovers them now instead of waiting for decay. Engineering to confirm whether scores persist or live only in memory. |

Ranking still only orders the list; nobody is ever removed from it.

### C. The handler sees quiet, and can act on it (console)

**Responder card gains a "Pings & reach" strip:**

```
Meteor Mite    Available    flight · cold-weather
Pings this week: 2   (usual 12)    Last taken: 11 days ago
Last 10 pings: 2 taken · 1 turned down · 7 no answer
● Quiet: ranked low after repeated no-answers.   [What's this?]
```

- **A "Quiet" state** appears when a responder's pings drop below about a quarter of their own normal for two weeks. It is defined against their own baseline, so it never compares a responder to a busier colleague.
- **Plain-language reason**, not a score: "7 of last 10 pings got no answer", "no pings in 9 days", "last ping not delivered".
- **Three handler actions**, each one tap:
  1. **Check connection:** sends the responder a test ping and shows whether it was delivered and opened. Answers "is the phone working?" in a minute rather than a ticket.
  2. **Request a fresh start:** puts the responder into re-entry (part B). Logged in the existing routing-override audit log (4.0), with the handler's name and the date.
  3. **Message the responder:** pre-filled "You've been quiet. Is your phone OK?"
- **Side-by-side clarity for multi-responder handlers.** Kip's two cards each show the strip, so a quiet one and a busy one are explained, not just different. (Distinct alert sounds per responder are a cheap adjacent win Kip asked for; track separately.)
- **Callout-live visibility.** When a ping goes out to a handler's responder, the console shows it, with a countdown, in a way that is hard to miss. Responds to Dot and Ambrose, and overlaps the Q4 Handler Phone App brief. Console only here; the phone app stays its own project.

### D. Continuous improvement: a quiet-responder monitor

A weekly view for the PM, Wen Li, Ravi and support, and a trimmed version in the handler's console.

| Metric | Why | Starting point (data) | Proposed target |
|---|---|---|---|
| **Responders in Quiet state** | The headline | 4 of 15 | 0 for more than 2 weeks |
| **Ping share vs own baseline, per responder** | Stops averages hiding people | Four at under 50% | All at 75% or more of baseline |
| **Median days quiet to recovered** | Is the way back real? | Never recovered | Under 14 days |
| **Miss rate by cause** (delivered-unanswered vs not delivered) | Separates reach from refusal | Unknown | New metric; target set once measured |
| **First-ping miss rate** | The "fair race" signal | 21.4% (was 2.7%) | Back toward 5% |
| **Callouts with no taken ping** | Guardrail, the customer-facing harm | 11.1% (was 5.5%) | At or under 6% |
| **Time-to-accept (median)** | Guardrail: fairness mustn't slow response | Ravi to supply | No worse than today |
| **"Quiet" and "gone before" tickets** | Voice of handler | About 39 and rising | Falling week on week |

Review rhythm: weekly with Ravi's numbers for the first six weeks, then monthly. Any responder who stays Quiet for 4 weeks after re-entry is investigated by hand (device, signal, availability windows), because that is likely a real reach problem rather than a ranking one.

---

## 6. Success criteria

1. A quiet responder is told they are quiet, and back to at least 75% of their usual ping volume within two weeks of re-entry.
2. A responder who is pinged gets a fair window: first-ping miss rate returns toward pre-4.2 levels.
3. A handler like Kip can say, from the console alone, why Meteor Mite is quiet, and can do something about it in one tap.
4. No responder stays Quiet for more than two weeks without a visible reason and an owner.
5. Callouts with no taken ping return to about the pre-4.2 level (5.5% to 6%) without a rise in median time-to-accept.

---

## 7. Scope

**In:** responder notices, half-time reminder, late-claim window (if safe), delivery receipts, split miss/decline penalty, decay, minimum offers, one-time reset, console quiet-state strip with check-connection and fresh-start actions, weekly monitor.
**Out:** the handler phone app (own brief), per-responder alert sounds, dark mode and bigger text (noted, separate), mutual aid, anything touching Supply, any identity data.

---

## 8. Open questions and risks

| Question | Owner |
|---|---|
| What happened on 12 to 14 Aug to make the four miss about 4x more? Push-delivery logs for those days. What did 4.2's duplicate-push fix change? | Wen Li / Marcus |
| Do `_scores` persist across restarts or live only in memory? Would a restart reset everyone to 0.5? | Wen Li |
| Was the 4.2 weighting meant to target decliners, or was it an accident? (Marcus, 14 Aug, still unanswered.) | Wen Li |
| Does the phone app report "received/opened" today? Can the late-claim race be made safe? | Engineering |
| Time-to-accept by responder; when did the weekly numbers first show the drop? | Ravi |
| What do quiet responders themselves say? We have only handler accounts. | Sofia |
| Is a minimum-offer guarantee acceptable for safety on live callouts? Where is the urgency line? | Helen / Marcus |
| Is the Quiet threshold right? Will "pings vs own baseline" misfire for new responders with no baseline? | Ravi / Sofia |
| **Risk:** more offers to a responder whose phone is the real problem just means more misses. Check-connection and the not-delivered rule are meant to guard this. | PM |
| **Risk:** the roadmap's "Availability Confidence" (slipped from 4.2) overlaps with a reach signal. Decide whether to merge or keep separate. | PM / Helen |
| Check Security Policy 4.1 on any new per-responder reach or delivery data. | PM |

---

## 9. Suggested next steps

1. Wen Li and Marcus: answer the engineering questions above, and size parts A and B.
2. Replay the ping history through the proposed scoring rules and show Helen where the four would land (the proof the math works, before building).
3. Sofia: talk to two or three quiet responders, then sketch the phone notices and the card strip. Test with Kip and Aunt Dot.
4. Helen: decide the interim step (reset the four, or wait), and confirm this belongs on the Q3/Q4 roadmap. The conversation about what is still a Q3 commitment is still open.
