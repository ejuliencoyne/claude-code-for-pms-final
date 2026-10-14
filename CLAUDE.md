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
product briefs, Dispatch page, 4.2 comments) and Priya's handover doc
(`00-rook/company/notes/handoff-from-priya.docx`, 21 Aug 2026). The user is the
new PM for Rook Dispatch (started Mon 24 Aug 2026). Fictional company.

### Products
- **Rook Dispatch** (ours, the flagship): responder coordination. Incident comes in
  -> rank available responders (routing priority) -> ping the top one -> taken, or
  turned down / missed and it moves to the next. Handlers use a web console;
  responders use a native phone app. Monthly release train (4.x); routing config
  ships with the release, not a runtime setting.
- **Rook Supply**: gear provisioning (requisitions, maintenance, field failure
  reports) for handlers and quartermasters. Separate team; Dispatch's Responder
  Availability Record is read by Supply for maintenance scheduling.
- Customers are responders (independent, not employees) and their handlers.
  Subscription priced per active responder.
- **Confidentiality:** Rook never stores cover-to-legal identity mappings. Read
  Security Policy 4.1 before designing anything touching responder records. Never
  try to work out who anyone is.

### People (Dispatch)
Wiki comments use roles, not names. Mapping:
| Role in comments | Person | Notes |
|---|---|---|
| Engineering manager | Marcus Oyelaran (Site Aleph) | Runs Dispatch eng; Priya's "start here" |
| Staff engineer | Wen Li (Berlin) | Built the ping-ranking logic; no doc exists, talk to her |
| Product designer | Sofia Marino (Site Aleph) | Console + phone app; ran September interviews |
| Support lead | Nadia Hoffmann (Berlin) | Owns tickets; suggested a standing 15 min |
| Director of Product | Helen Achebe (Site Aleph) | The user's manager; owns roadmap and commitments |
| Data analyst | Ravi Menon (Singapore) | Owns the weekly responder ping-acceptance numbers |
| Priya | Priya Raghunathan | Previous Dispatch PM, left 21 Aug 2026 |
Named handlers in the Research interviews: Aunt Dot (for Vesper), Mr. Ambrose
(Captain Vantage), Halloran (Sgt. Bulwark; also the Supply quartermaster voice in
briefs), Kip (Meteor Mite, The Gale).

### Vocabulary
- **Callout**: request for a responder to attend an incident; Dispatch's unit of work.
- **Ping**: a callout offered to one responder's phone. **Taken** = yes. **Turned
  down** = no. **Missed** = ping wait ran out. Turned down and missed both move
  on, but are recorded separately.
- **Ping wait**: how long a ping stays before counting as missed (same for
  everyone, set per release).
- **Routing priority**: score ranking responders: proximity (travel-time
  estimate), availability, capability match, recent acceptance history. Turning
  down or missing pings lowers the recent-acceptance part.
- **Acceptance rate** (headline metric: taken / all pings), **time-to-accept**
  (median seconds ping -> taken), **coverage gap** (no available responder had the
  needed capability tags; different from low acceptance).
- **Capability tag**, **Responder Availability Record**, **mutual aid** (not
  supported; Q4 exploration), **requisition**, **field failure report**, **service
  interval**, **cover identity**.

### Where things stand (as of the wiki, last activity ~2 Sept 2026)
**4.2 shipped 12 Aug, and is the live problem.** It did two things at once:
(1) proximity weighted up vs. recent acceptance history in routing; (2) ping wait
cut 90 -> 60 s. Plus filter persistence and 3 fixes.
- Support: tickets ~3x normal from 12 Aug, said to be ~2/3 "phone never goes off",
  ~1/3 "gone before I could answer" (Nadia's split; not checked in the DB).
- **Data (rook-database, pings 29 Jun-6 Sep; wiki says Ravi owns weekly numbers):**
  taken rate was flat 75-78% for 6 weeks, then 54% (wk of 10 Aug), 66, 67, 73%
  (wk of 31 Aug). Missed went ~2% -> 12-21%; turned down didn't rise. Tickets went
  from 5-8/wk to 20-32/wk. This is a step on the release date, not a gradual
  slide, which cuts against Priya's "seasonal, recovers in Sept" view (prior
  Augusts not checked).
- Two effects: most responders (12 of 16) saw missed rise to ~11-16% (fits the 60 s
  cut) and recovered to ~70% taken. **Four collapsed: Farlight, Meteor Mite, The
  Undertow, Vesper**: ~49 pings/wk down to 3, mostly missed, none taken since 19
  Aug. They hide in the aggregate (headline rate recovers partly because they drop
  out). Why they miss most of what they do get is unexplained.
- Likely mechanism (hypothesis, from `dispatch-routing`): a miss counts as a decline
  (-0.12; accept is only +0.08), there's no decay (2019 TODO), so misses push
  responders down the order and starve them. Meteor Mite and The Gale share
  Eastgate: after 12 Aug The Gale got 69 pings, Mite 14.
- Marcus's 14 Aug question (does weighting apply equally to responders who
  decline?): answered by code, it does; config has no distinction. Wen never
  followed up.
- Interviews (Sept, Sofia): Vesper's handler Aunt Dot and Kip (Mite/Gale)
  independently describe quiet-vs-fast weeks; Ambrose lost a callout mid-dressing.
- Marcus and Nadia agreed (28 Aug) to regroup on 4.2 after the PM's first week.

**Roadmap is stale.** Q3 roadmap last reviewed 30 June; everything owned by Priya.
- 4.2 Committed: Change to who gets pinged (shipped); Ping timeout tuning (a cut
  shipped; unclear whether "tuning" is done); **Availability Confidence (not in
  the 4.2 notes, still shows Committed)**. Priya says some items were squeezed out
  of 4.2 and the Director conversation on which are still Q3 commitments hasn't
  happened. **Open action: talk to Helen.**
- 4.3 Committed: Requisition approval chains (Supply).
- Q4 Exploring: Handler phone app (Sofia); Shared cover between responders.

**Briefs (wiki Product briefs)**
- Bulk Callout (Jan): brief says unowned, but it shipped in 4.1; brief is stale.
  Routing Override Audit Log (Mar): in 4.0.
- Requisition Approval Chains (Jul, Supply): core ask is a second approval over
  $2,000, plus several extras; phasing undecided. Halloran's interview complaint is
  speed (11-day wait, priority field ignored), which a second approver may worsen.
- Handler Phone App (8 Sept): notifications, status view, one-tap contact;
  Sofia exploring, unstaffed, no baseline metric.

**Other to-dos from Priya's handover:** write down how routing decides who gets
pinged (no doc exists; Wen is the source); expect low-value tickets from filter
persistence; use the arrival-without-attachment window in month one; she was sole
PM for 14 months and made calls faster than she checked them.

### Working notes
- Routing code: `00-rook/code/dispatch-routing/` (`config.py` weights 0.60 prox /
  0.25 acceptance / 0.15 capability, was 0.45/0.40/0.15; timeout 60 s). Stubs:
  availability and device push. `00-rook/feedback/` is empty.
- rook-database tables: callouts, pings, responders, handlers, support_tickets.
- Wiki search matches titles only; read_page shows comments (much of the 4.2 story).
- Filter persistence lives in the console codebase, not in `dispatch-routing`; how
  it works is unknown. Ask Marcus or Wen for where it lives.
- Data stops 6-7 Sep; the session ran 13 Oct. Ask Ravi for newer weeks. Weeks are
  Mon-start; 4.2 cutoff used was `sent_at >= '2026-08-12'`.
- Open next: confirm the miss-spiral with Wen; find why the four miss pings;
  talk to Helen about 4.2/4.3 commitments; check Nadia's ticket split; write up
  the routing logic Priya never did.
- Known inconsistencies: glossary says missed is recorded separately from turned
  down, but scoring treats them the same; "monthly" releases are really ~2-monthly
  and no 4.3 is listed; Ashgrove's "phone silent 3 days" ticket (#3070) does match
  his pings (17 Aug evening to 20 Aug evening), so that is not an inconsistency.
- Module 1 status (13 Oct): everything above comes from files, the wiki and the
  database; no teammate has been consulted yet, so the mechanism is unconfirmed.
- Sources that mattered most: the 4.2 page comments, the pings table, `config.py`
  and `history.py`, and the Aunt Dot and Kip interviews.
- Priya's handover never mentions Ravi or Sofia; Ravi (weekly numbers) is the
  obvious first person to ask.
- Module 2 (13 Oct): interviews + tickets agree on two issues. (1) Callouts gone before
  the responder can answer: 15 tickets, 11 responders, from 12 Aug (60 s ping wait).
  (2) Quiet/starved responders: 30 tickets, Farlight and The Undertow collapsed (last
  taken 14 and 17 Aug); Meteor Mite and Vesper collapsed the same way but their
  handlers (Kip, Aunt Dot) filed zero tickets, so ticket counts understate severity.
  Ashgrove and Halfmoon only dipped (~10 to 7-8 a week) and recovered.
- Pings: no ping ID or ranking score, but (callout, responder) is unique and sent_at
  gives order. The Gale was pinged on all 16 Eastgate callouts in the week of 24 Aug,
  Mite on 1; The Gale rose 13 to 21 a week while Mite fell 11 to 1. Untaken callouts
  doubled (5.5% to 11.1%); 41181 and 41193 ended after one ping, cause unknown.
- Location data is coarse: only home `area` for responders and callouts. Travel-time
  source and what "available"/"region" mean are in a stub (`availability.py`), unknown.
  Proximity formula (1 - minutes/45) did not change in 4.2, only its weight (0.45 to 0.60).
- Interview vs ticket gaps: Ambrose's incident dated 12 Aug (ticket) vs "two weeks ago"
  (interview); Dot sees quiet/fast weeks as alternating, data shows fast first then quiet;
  Halloran says maintenance scheduling improved but tickets #3054, #3109 say otherwise.
- Open next: ask Wen for the travel-time source, availability/region rules, and why
  Mite is never pinged; ask Ravi for newer weeks and the untaken-callout rate.
- Module 3 (14 Oct): Wen and Ravi are not real, so "ask Wen/Ravi" items above are dead ends; answers must come from files, the wiki and the database (data stops 6 Sep).
- Callouts: all 1,319 were pinged at least once, but volume fell from ~138 a week to 110 in the 4.2 week and 121-127 since (cause unknown). Never-taken callouts doubled (48 of 879 to 49 of 440, 5.5% to 11.1%), and all of the rise is callouts ending on a missed ping (0.3% to 5.2%); ending on a turn-down is flat at ~5%. Most never-taken callouts stop after one ping; why nobody else is tried is unknown. The rise sits in Southport, Riverside and Kingsbridge (6-9 callouts each, small counts).
- Vesper and Meteor Mite: both began missing pings in the days after 12 Aug, then pings dried up. After 4.2 neither took a callout where pinged first (their few takes were fallbacks); last taken 17 Aug (Vesper) and 19 Aug (Mite). Pings after a miss are now ~60 s apart (was ~90 s). Why they miss so much is still unexplained. Nightwell, The Gale, Vantage took over the work.
- Tickets: 28 "quiet / phone not gone off" tickets, all about Farlight, The Undertow, Ashgrove, Halfmoon, all still open and unanswered. Handlers' ping counts match the pings table, so "phone didn't ring" means no ping was sent (starvation), not a delivery fault; two "first in N days" tickets are missed pings. Only ~40% of post-4.2 tickets are routing-related; the rest are filters, passwords etc. Ashgrove and Halfmoon pings are still 7-8 a week (not back to 10).
- Each responder has one handler; Kip has two (Meteor Mite and The Gale, both Eastgate). Vesper's (Aunt Dot), Mite's (Kip), Bulwark's (Halloran) handlers filed zero tickets.
- Open next: read the ~60 non-routing tickets to confirm the split; list the never-taken callouts in Southport, Riverside and Kingsbridge one by one; find why callouts end after one ping.
- Module 4 (14 Oct), 4.2 code and UI: the only code change in `dispatch-routing` is `config.py` (timeout 90 -> 60 s; weights proximity 0.45 -> 0.60, acceptance 0.40 -> 0.25, capability 0.15 unchanged). Git has one initial commit, so changes come from the CHANGELOG and "was ... until 4.2" comments. `poll_device`, `available_for` and the other device/availability functions are stubs (`...`).
- Pings data vs weights: home-area responder pinged first fell 80% -> 61% after 12 Aug. In the three single-responder areas with a collapsed responder (Old Town/Vesper, Uptown/Farlight, Harborside/Undertow) the home responder was never pinged on 76 of 101 callouts (0 anywhere before; no other area affected). The 7-day recent-miss count did not predict being skipped.
- Score replay (history.py rules, actual pings, assumed home/away proximity, capability ignored): the four collapsed responders' acceptance score fell from ~0.85-0.94 to ~0.40-0.58 and hit 0.00; the other 12 stayed 0.77-0.93. Proximity weight alone does not explain starvation; old weights starve at least as much on the same history. Real home/away proximity gap must be small (~0.1-0.2) to match reality. Not a true counterfactual (misses were observed at 60 s).
- The four miss 59% of pings after 12 Aug vs 14% for the other 12 (both ~2-3% before). Their misses began 12-14 Aug, before pings dried up, so a 60 s timeout alone does not explain them; why they miss is still unexplained. Reverting to 90 s would likely help the 12 (and the "gone before I could answer" tickets) but probably not the four: scores sit at the 0.00 floor with no decay (2019 TODO). `history.py` keeps scores in an in-memory dict; whether production resets on restart is unknown (no one to ask; not answerable from the files).
- Tickets, all 147 read (40 before 12 Aug, 107 after): routing-related 45 of 107 (30 quiet/starved incl. 2 forwarded quotes, 13 gone before answer, 2 "first in N days then missed"), all open. 14 filter persistence, 4 maintenance, 22 other console UI/features, 10 account/admin, 12 Supply/gear. Non-routing, non-filter tickets also ~doubled (about 6 -> 11 a week), cause unknown. The three 4.2 defect fixes (duplicate push, capability tag ordering, coverage export time zone) look effective: 5 tickets before, none after.
- Filter persistence = handler-side console setting that keeps filter selections (responder, capability tags) between sessions. Only descriptions are the 4.2 release line, Priya's handover ("cosmetic ... noise"), Ambrose's interview, and tickets (not shared across computers, shared-computer mix-ups, silent resets, no clear-all, easy-to-miss badge). No spec in the wiki; console code is not in this repo. Unlikely cause of the four's misses: pings are answered on the phone; Kip handles both Meteor Mite (collapsed) and The Gale (did not) on one console and barely uses filters; handlers of the other 5 responders with filter tickets saw no collapse; Aunt Dot and Kip filed zero tickets.
- Marcus's 14 Aug question (comment on the 4.2 page): "was that meant to apply to responders who've been turning jobs down too, or only everyone else?" and "was it a decision or just fell out that way". Wen replied she'd look after the 24th; no answer posted. Code answer: applies equally; decision vs accident still unknown.
- Open next (fictional team, so answers must come from files, wiki and database, or be written up as open questions/assumptions): look for anything in the data that separates the four from the other 12 beyond pings (e.g. time of day, area, handler); write the 90 s revert plus score recovery (decay, softer miss penalty, or reset) up as a recommendation with its assumptions; list what cannot be answered from the sources (score persistence in production, whether persisted console filters affect availability, why the four miss).
- Module 4, what separates the four (Farlight, Meteor Mite, The Undertow, Vesper) from the other 12 (pings, post-12 Aug unless stated): NOT time of day (miss 64% day / 50% evening / 50% night vs 13/17/7%), NOT callout type (descriptions almost all unique), NOT area or handler (four areas, four handlers; Mite collapsed but The Gale, same area and handler, did not), NOT prior behaviour (pre-4.2 taken 76% vs 77%, turned down 22% vs 21%, missed 3% vs 2%; they had the highest acceptance scores). DOES differ: as first-choice ping the four miss 70% and take 8% (n=40) but as fallback miss 31% and take 63% (n=16); others first 17% missed/64% taken, fallback 7%/80%. Their miss rate rises with time since their last ping (29% under 3h, 50% 3-12h, 77% 12-24h, 89% 3d+; others flat 10-20%), direction unclear (idle -> misses, or misses -> fewer pings). Small samples. Not answerable from the data: device/app/push delivery (no such data; `handlers` table has only a name), answer time (pings table has outcome only), delivered vs not-answered. Treat "first-choice pings to the four go unanswered" as an open hypothesis, not a finding.
- Module 4, no earlier spikes: weekly miss rate was 1.2-3.4% every week 29 Jun-3 Aug (no responder had 2+ misses in any week); then 21.5% (week of 10 Aug), 17.7%, 14.8%, 12.7%. Data starts 29 Jun, so prior Augusts (Priya's "seasonal" idea) still can't be checked.
- Module 4, recommendation (draft, assumptions flagged): (1) Restore ping wait to 90 s: likely fixes the miss rise for the other 12 and the "gone before I could answer" tickets. (2) Reset the four's recent-acceptance scores to neutral: with no decay a responder at the 0.00 floor cannot climb back, which is why a reset is needed after the revert. Decay was NOT changed in 4.2 and pre-4.2 scores were healthy (~0.85-0.94), so missing decay is not the cause. (3) Leave the weights open: in the replay the old weights (acceptance 0.40) starved the four at least as much, so rolling weights back is not shown to help. (4) Optional hardening, separate from the rollback: decay toward neutral or a lighter penalty for a miss than a turn-down, only relevant if misses spike again for another reason. (5) Answer the ~30 open quiet/starved tickets (Linda Pruitt for Farlight, Desmond Okafor for The Undertow). Assumptions: the 60 s wait caused most of the miss rise; why the four miss ~4x more than others (59% vs 14%) is unexplained and no device/answer-time data exists; the team is fictional, so handler conversations are recommended actions only.
- Module 4, other responders: The Gale carries extra load (13 -> 21 pings a week; Kip says "exhausted"), a reset should rebalance; Nightwell and Stormwrack at 18-21 a week, watch for overload. Corporal Ashgrove and Halfmoon dipped (~10 -> 7-8 a week) and have not recovered, but their replayed scores stayed healthy (lows 0.64 and 0.56), so the cause is not the score; watch list. The other responders are recovering (taken ~68-70%); no individual action. Track untaken callouts after any change (doubled 5.5% -> 11.1%, all on a missed ping; most stop after one ping, reason unknown).
- Module 4, decay simulation (retroactive: each past ping's +0.08/-0.12 weighted by 0.5^(age/half-life), summed onto 0.5, capped 0-1; replay of actual pings, send time used as event time, not step-clamped like history.py): score on 7 Sep for Farlight/Meteor Mite/The Undertow/Vesper = 0.36/0.23/0.29/0.29 (7-day half-life), 0.33/0.13/0.31/0.40 (14-day), 0.53/0.17/0.62/0.95 (28-day); the other 12 were 0.60-1.00, 0.86-1.00 and 1.00. Decay alone does NOT reset the four on this history because they kept missing into late Aug and early Sep (Farlight 28 Aug, Vesper 1 Sep, Mite and Undertow 5 Sep) and recent misses count in full; Meteor Mite stays very low under every setting. Slower half-lives delay the collapse (Vesper/Undertow still ~1.0 on 24 Aug at 28 days) but recover less quickly; a 7-day half-life compresses the spread among the other responders. Not a true counterfactual (the four would have been pinged more under decay; their answers unknown).
- Module 4, revised view on the fix: prefer decay over a manual reset to avoid intervention, but treat "do the four stop missing after the 90 s revert?" as the test of whether decay is enough; keep a one-off reset as an optional bridge; if they keep missing, the unexplained cause of their misses still has to be found. Half-lives of 7/14/28 days are illustrative, not tuned.
- Module 4 status (14 Oct): the 4.2 code changes, ticket types and recommended fix (90 s revert; decay or reset for the four) are written up above; the cause of the four's extra misses is still unexplained and nothing in the files, wiki or database can answer it.
