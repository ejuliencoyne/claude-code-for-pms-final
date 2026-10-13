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
