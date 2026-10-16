# Quiet Responders: one-pager

**Dispatch PM, for Helen Achebe · Draft, nothing built yet**

**The idea:** when a responder starts going quiet, *both* they and their handler should see it within days, have a one-tap way to test the phone, and have a fair way back. Today neither sees anything, and the system punishes the silence.

## Who it's for
**Primary: the quiet responder.** Phone only, often not at it, and sees no ranking or reason. **Secondary: their handler** (Kip, Aunt Dot), who sees a dead card next to a busy one and can only say "hang in there." Both are known by handle and tags only (Policy 4.1). *Gap: our quotes are all from handlers; we haven't heard from a quiet responder yet.*

## Walkthrough: Vesper (handler: Aunt Dot)
Real data from our pings table.

| | What happened | What the proposal changes |
|---|---|---|
| **Before 4.2** | 13 to 15 pings a week, about 80% taken | Nothing; he's the baseline |
| **13 to 16 Aug** | 4 misses in his first 8 pings after 4.2 (13, 14, 16, 16 Aug), 1 turn-down. Dot hears the buzz through the ceiling and shouts up the stairs | **16 Aug: Quiet flag fires** ("3 of last 6 pings had no answer"). Dot sees it on his card; Vesper gets a "we've noticed you've been missing pings" notice. Half-time reminder and late-claim window start giving him a fair race |
| **17 to 19 Aug** | 1 taken, then a miss. Week total 5 pings, down from 12 | Dot taps **Check connection**: test ping shows delivered, opened or not. Either "phone's fine, it was upstairs" or "never arrived, check signal/app." Both are fixable in a day |
| **19 to 26 Aug** | Misses on 19, 23 (twice) and 26 Aug. Pings drop to 2 a week. Score heads to the floor | Undelivered pings cost nothing; other misses cost −0.04, not −0.12. If he's still low, Dot taps **Request a fresh start**: score to 0.5, plus a guaranteed offer within 7 days |
| **1 Sep** | Score at 0. One ping that week, missed. About 1 ping a week, needs about 7 yeses in a row to recover | Same 17 pings, lighter miss penalty alone: score ends at **about 0.56 instead of 0**. With a reset, higher |

*Replay assumes he started at 1.0 on 12 Aug and scores persist (to confirm with Wen). The ping sequence is his real one; what he'd have done with different offers is a guess.*

## What we'd build
- **Responder (phone):** half-time reminder; late-claim window (first tap wins); delivery receipts (received, opened); "you've been quiet" and "we're bringing you back" notices. No scores, no blame.
- **Handler (console):** card strip: *"Pings this week 2 (usual 13) · 3 of last 6 no answer · Quiet"* with a plain reason. One-tap: **Check connection**, **Fresh start** (audit-logged), **Message responder**.
- **Routing (Wen, Marcus; numbers illustrative):** miss costs less than a decline (−0.04 vs −0.12; undelivered costs nothing); score drifts toward 0.5 when idle, never above; guaranteed offer after 7 quiet days on non-urgent callouts; one-time reset for the four (Vesper, The Undertow, Farlight, Meteor Mite).
- **Quiet trigger:** 3 no-answers in the last 6 pings, or volume under half their own usual for a week. Not "a quarter for two weeks": that would have fired in September.

## What else it catches
| Scenario | What happens |
|---|---|
| **Out of signal** (Undertow, ticket 3014) | Receipt shows "not delivered"; no penalty; handler sees why |
| **Phone fine, responder just slow** (Vesper's stairs) | Reminder and late-claim help; the test ping proves it's not the phone |
| **Genuine decliner** | Declines still cost −0.12 and idle decay never goes above neutral, so saying no still lowers rank |
| **Many responders flag in the same week** (12 Aug hit all 15 areas) | Monitor alerts the PM and engineering as a likely release regression, not as 15 individual problems |
| **New responder, no baseline** | Quiet trigger uses recent misses, not history |
| **Responder on leave** | No flag outside their availability windows |
| **Handler with two responders** (Kip) | Each card explains itself, so quiet vs busy isn't a mystery |
| **Handler doesn't act** | Flag stays; shows in the weekly digest |
| **Urgent callout** | Guaranteed offers never apply |

## What it will NOT solve
- **Why the four missed 4x more on 12 to 14 Aug.** Still unknown. This gives us the receipts to find out next time.
- **Responders who are truly unreachable or have stepped back.** We surface it; we can't fix a person's phone or plans.
- **Fewer callouts.** It restores fair offers, not volume. A slow month stays slow.
- **Proximity favouring the responder next door.** Weighting is 0.60. A far-away responder can still be pinged less, just not starved.
- **Coverage gaps** (no one with the right tags), mutual aid, dark mode, handler phone app. All separate.
- **The 60s ping wait itself.** We test half-time and late-claim; we haven't proven 60s is wrong.

## How we'll know
| Metric | Now | Target |
|---|---|---|
| Responders Quiet >2 weeks | 4 of 15 | 0 |
| Flag to handler action | no flag | under 2 days |
| Days quiet to recovered | never | under 14 |
| Ping share vs own usual | four under 50% | 75%+ |
| First-ping miss rate | 21.4% | toward 5% |
| Callouts with no taken ping (guardrail) | 11.1% | about 6% |
| Time-to-accept (guardrail) | Ravi to supply | no worse |

Weekly review with Ravi for six weeks.

## Open before build
Do scores persist across restarts? Can the phone report received/opened, and is late-claim safe? Where's the urgency line? What do quiet responders say? (Wen, Engineering, Helen/Marcus, Sofia)
