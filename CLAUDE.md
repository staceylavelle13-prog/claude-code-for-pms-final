# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

Sources: rook-wiki (About Rook, Glossary, Team directory, Releases, Q3 roadmap,
Product briefs, 4.2 release page and comments) and Priya's handover doc
(00-rook/company/notes/handoff-from-priya.docx, 21 Aug 2026). Wiki pages are
dated; check dates before treating anything as current. Live data is in rook-database.

### Me and the job
I'm the new PM for Rook Dispatch, replacing Priya Raghunathan (left 21 Aug 2026,
no overlap). She was the only Dispatch PM for 14 months and says she made calls
faster than she checked them. I report to Helen Achebe.

### Company and products
Rook Industries sells coordination and provisioning software to independent masked
responders and their handlers/quartermasters (publicly: "logistics and workforce
coordination for emergency services"). 241 employees, HQ Site Aleph; offices in
Berlin, Singapore, Cornish lighthouse. Subscription, priced per active responder.
Monthly release train; point releases are 4.x.
- **Rook Dispatch** (mine): availability, proximity, who gets pinged about a callout,
  whether they take it. Users: handlers (console, desktop) and responders (phone).
- **Rook Supply**: requisitions, maintenance, field failure reports. Users: handlers,
  quartermasters. Not mine, but shares the Responder Availability Record
  (written by Dispatch, read by Supply).
- **Confidentiality, hard rule:** cover identities are never stored; we hold capability
  tags, availability windows, callout history only. Never design anything that assumes
  a cover-to-legal-identity mapping, and never try to work out who anyone is. Read
  Security Policy 4.1 before touching responder records.

### People (wiki roles in comments map to these names)
| Role in comments | Person | Notes |
| --- | --- | --- |
| Engineering manager | Marcus Oyelaran (Site Aleph) | Runs Dispatch eng. Priya: "start here"; straight talker |
| Staff engineer | Wen Li (Berlin) | Built the routing/ping logic; the only real source on how ranking works |
| Product designer | Sofia Marino (Site Aleph) | Owns console and phone app; ran the September interviews |
| Support lead | Nadia Hoffmann (Berlin) | Owns tickets; hears handler complaints first; worth a standing 15 min |
| Data analyst | Ravi Menon (Singapore) | Owns the weekly numbers on how often responders take pings |
| Director of Product | Helen Achebe (Site Aleph) | My director; owns roadmap and commitments |
| "Priya" | Priya Raghunathan | Predecessor; reachable only via Marcus, "if something catches fire" |

Handlers named in wiki rows: Aunt Dot (for Vesper), Mr. Ambrose (Captain Vantage),
Halloran (Sgt. Bulwark; also a Supply voice on approvals and the gear-cage
spreadsheet), Kip (Meteor Mite and The Gale).

### Vocabulary (Glossary, 4 Aug 2026)
- **Callout**: request for a responder to attend an incident; Dispatch's unit of work.
- **Ping**: one callout offered to one responder's phone. Outcomes: **taken**,
  **turned down** (moves to next responder), **missed** (ping wait expired; also moves
  on, but recorded separately).
- **Ping wait**: how long a ping sits before counting as missed; one value for everyone,
  set in the release.
- **Acceptance rate**: taken / all pings; headline metric. **Time-to-accept**: median
  seconds from sent to taken.
- **Coverage gap**: no available responder had the required capability tags. Not the
  same as low acceptance (nobody *could* go vs nobody *would*).
- **Routing priority**: score ranking available responders. Inputs: proximity
  (travel-time estimate), availability, capability match, recent acceptance history.
  Turned-down or missed pings lower the recent-acceptance part, so lower later rank.
- **Capability tags**: flight, structural-entry, hazmat-tolerant, cold-weather,
  aquatic, crowd-management, de-escalation.
- **Mutual aid**: responders covering each other's callouts across the city. Unsupported;
  Q4 exploration.
- Supply: **requisition** (handler asks, quartermaster approves), **field failure
  report**, **service interval**.

### Releases (Dispatch)
- **4.0** (7 Apr): new console nav, responder profile redesign, routing-override audit log.
- **4.1** (16 Jun): proximity uses travel-time estimate, bulk callout, push delivery reliability.
- **4.2** (12 Aug): proximity weighted up vs recent acceptance; ping wait cut 90s to 60s;
  console filters persist; three defect fixes. Released clean (no rollback, no pages).

### Where things stand: 4.2 is the live problem
Since 4.2: callout tickets ~3x normal (from ~12 Aug week), still elevated at 26 Aug,
flat. Split about two-thirds "phone never goes off", one-third "gone before I could
answer". A handler emailed Nadia directly, which never happens. Nadia can supply the
ticket breakdown.
- **Two changes shipped together** (routing weighting and 60s ping wait), so cause is
  not separable from the release notes alone. The 60s wait plausibly explains the
  "gone before I could answer" third (Nadia's read). Nothing yet explains "never
  goes off".
- **Open question from Marcus, unanswered (14 Aug):** was the weighting meant to apply
  to responders who've been turning jobs down, or was that an accident? Config doesn't
  distinguish. Wen Li said she'd look on return (24 Aug); no answer on the page.
- **Priya's read:** mostly seasonal (August is soft), come back in September; don't
  revert, the change was asked for by wide-area responders. That is a hypothesis with
  no numbers behind it. It was set aside, not tested. Marcus held off giving me a
  conclusion until I'd looked myself.
- **No real data yet:** Marcus offered only a rough pull. Ravi's weekly acceptance
  numbers are the thing to get; compare before/after 12 Aug and against prior Augusts.
- **Undocumented:** nobody has written down how ping ranking works. Priya asked me to.

### Roadmap (Q3, last reviewed 30 Jun, before 4.2; stale, all owned by Priya)
| Item | Target | Status | Reality |
| --- | --- | --- | --- |
| Change to who gets pinged | 4.2 | Committed | Shipped |
| Ping timeout tuning | 4.2 | Committed | 90 to 60s shipped; confirm that was the whole item |
| Availability Confidence | 4.2 | Committed | Not in 4.2 release notes; slipped, still shown Committed |
| Requisition approval chains | 4.3 | Committed | Supply item; brief is bloated (one ask plus four add-ons) |
| Handler phone app | Q4 | Exploring | Brief 8 Sep, Sofia exploring, unstaffed |
| Shared cover between responders | Q4 | Exploring | Cover identity rules apply; check Security Policy 4.1 first |
Priya said a couple of items were squeezed out of 4.2 and that she never had the
conversation with Helen about which are still Q3 commitments. That conversation is mine
to have. Committed items on the roadmap are "locked"; changes go through Product.

### Product briefs
Bulk Callout (Jan 2026) and Routing Override Audit Log (Mar 2026) are older than the
4.0/4.1 releases that list those features; their "nobody's picked it up" notes are
out of date. Handler Phone App (Sep) and Requisition Approval Chains (Jul, Supply)
are current.

### Working notes
- Don't reopen 4.2 as "revert or not" before the data. Don't hand stakeholders
  conclusions the evidence doesn't support.
- Priya says console filter persistence tickets are noise; ignore for now.
- Mobile has been stable since 4.1; console is stable; routing is the risk area.
- Not yet read: Rook Dispatch / Rook Supply pages, Security Policy 4.1.
- rook-database has callouts, pings, responders (name, handler, area only), handlers
  and support_tickets (29 Jun to 7 Sep). No tags, terrain, population or signal data;
  15 areas, one responder each except Eastgate (two).
- Data finding: missed pings went from 2.3% to 18% after 12 Aug (taken 76.6% to 64%);
  turned-down barely moved; it hit all 15 areas. Weekly callouts fell only 10-20%, so
  "seasonal" doesn't explain it. Tickets went from 5-8 a week to 20-32.
- Four responders (Vesper, The Undertow, Farlight, Meteor Mite) went from about 12 pings
  a week to 1-6; The Gale and Nightwell kept about 70. Handlers Desmond Okafor and
  Linda Pruitt have open tickets on this since 17 Aug; Aunt Dot and Kip say the same.
- My working hypotheses, not confirmed: (1) the 60s wait causes the misses; (2) the
  ranking change starves responders with weaker acceptance history; (3) the two feed
  each other (misses lower rank). Next: ask Wen Li about 2, check answer-time history
  for 1, ask Ravi when his weekly numbers first showed the drop.
- Ticket 3014 (14 Jul, before 4.2): The Undertow missed a ping out of signal, so phone
  signal is a real factor the fixed ping wait ignores.
- Code is in 00-rook/code/dispatch-routing. In history.py a missed ping and a turned-down
  ping both cost -0.12 (taken +0.08), the score never decays (2019 TODO), and intent isn't
  recorded. Break-even is about 60% taken; a near-perfect score hits 0 after about 8
  non-yes pings. Ranking only sets order; nobody is removed from the list.
- Replay of the ping history: the four starved responders had strong history before 12 Aug
  (replayed scores 0.84-1.0, about 80% taken, turned-down share normal). Their misses came
  first (burst 12-14 Aug, while scores were still high), then pings fell week by week
  (about 49, 16, 6, 3). Hypothesis 2 mostly contradicted; 3 partly supported; 1 untested
  (pings has no answer-time column). Giving turned-down pings no penalty still leaves
  them at 0.16-0.32, so declines aren't the driver; misses are.
- Still unexplained: why those four missed about 4x more than everyone else (53-64% vs
  10-18%). Signal or device is a guess. Next: check what they share in the 12-14 Aug misses.
- Interviews (4, Sept) vs tickets (147): interviews are loud on "gone before I could answer",
  dark mode, bigger text; tickets show "quiet / never goes off" is about 39 tickets, 38 after
  12 Aug, and filter persistence is 16 tickets (13 after 4.2), so not noise. Dot and Kip
  described quiet weeks but filed no tickets. Draft note to Priya (via Marcus) not yet sent.
