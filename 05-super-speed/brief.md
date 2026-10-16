# Product brief: Quiet Responders

**Dispatch PM, for Helen Achebe · Draft · Nothing built yet**

## Executive summary
Since 4.2 (12 Aug), four responders have gone quiet and can't get back. A missed ping costs the same as a turn-down, scores never recover, and a low-ranked responder is rarely pinged, so they can't earn points back. Nobody sees it happening: handlers get no ping history, and responders get no explanation. We propose that when a responder starts going quiet, both they and their handler see it within days, can test the phone in one tap, and have a fair way back in. This is more than moving 60s to 90s, which wouldn't lift anyone off the floor.

## Who it's for
- **Primary: the responder** (e.g. Vesper). Phone only, often away from it. Sees no ranking and no reason, just silence.
- **Secondary: the handler** (e.g. Aunt Dot, Kip). Sees a dead card next to a busy one and can only say "hang in there."

Both are known by handle and tags only (Policy 4.1). *We have only heard from handlers; we need to talk to a quiet responder.*

## The problem
1. **Silence is scored as refusal.** A miss and a turn-down both cost −0.12; scores never decay.
2. **There is no way back.** Break-even is about 60% taken, and a low-ranked responder is rarely pinged. Vesper: 5 taken, 9 missed in 17 pings, at the floor by 1 Sep, needing about 7 yeses in a row to recover.
3. **We can't tell "didn't reach" from "didn't answer."** The four had 58.9% missed vs 13.9% for everyone else, though they weren't decliners.
4. **It's not a fair race.** First pings missed 2.7% before 4.2, 21.4% after.
5. **Nobody can see it,** so four people hid inside an average.

## Proposed solution
- **Responders:** half-time reminder, late-claim window (first tap wins), delivery receipts, plain "you've been quiet" and "we're bringing you back" notices.
- **Handlers:** a Quiet flag on the card ("3 of last 6 pings had no answer"), with one-tap **Check connection**, **Fresh start** (audit-logged) and **Message responder**.
- **Routing (Wen, Marcus; numbers illustrative):** a miss costs less than a decline (−0.04 vs −0.12) and an undelivered ping costs nothing; idle scores drift back toward 0.5, never above; a guaranteed offer after 7 quiet days on non-urgent callouts; one-time reset for the four.

## What changes once it ships
| | Today | After |
|---|---|---|
| **Responder** | Silence, no reason. Pings vanish before they reach the phone. | Told when they've gone quiet. Fair window to answer. Misses don't sink them. Coming back is automatic. |
| **Handler** | Can't see ping history. Can only say "hang in there." | Sees the flag within days, with a reason. Tests the phone and restarts the responder in a tap. |
| **Vesper** | Flag would have fired 16 Aug. Score would end near 0.56, not 0. | Aunt Dot would have known within 4 days, not weeks. |

## How we'll know it worked
| Metric | Now | Target |
|---|---|---|
| Responders quiet >2 weeks | 4 of 15 | 0 |
| Flag to handler action | no flag | under 2 days |
| Days quiet to recovered | never | under 14 |
| First-ping miss rate | 21.4% | toward 5% |
| Callouts with no taken ping *(guardrail)* | 11.1% | about 6% |
| Time-to-accept *(guardrail)* | Ravi to supply | no worse |

## What this will not solve
- **Why the four missed 4x more on 12 to 14 Aug.** Still unknown; receipts will tell us next time.
- **Unreachable or departed responders.** We surface it; we can't fix a phone or a plan.
- **Low volume.** Fair offers, not more callouts.
- **Proximity weighting (0.60).** Far-away responders can still be pinged less.
- **Coverage gaps, mutual aid, handler phone app, dark mode.** Separate work.
- **Whether 60s is the right ping wait.** We test around it, not prove it.

**Open before build:** do scores persist across restarts (Wen)? Can the phone report received/opened, and is late-claim safe (Engineering)? Where's the urgency line (Helen, Marcus)?
