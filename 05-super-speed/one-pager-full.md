# Dispatch: no responder should stop receiving pings without anyone noticing

*For Helen Achebe · from the Dispatch PM · draft, October 2026*

## Executive summary

**What happened.** Release 4.2 (12 Aug) cut the time a responder has to answer a ping (the ping wait) from 90 s to 60 s, and gave proximity more weight in deciding who is pinged first. Since then, support tickets have gone from 5-8 a week to 20-32, and two problems are behind them:

- **Callouts disappearing before responders can answer.** Missed pings (the wait ran out) rose from about 2% to 12-21% of all pings. This affects most responders (12 of 16). It has eased since, and the share of pings taken is back to about 73%, against 75-78% before 4.2. 15 tickets from 11 responders describe it.
- **Four responders whose pings have not come back since the 4.2 changes.** Farlight, Meteor Mite, The Undertow and Vesper went from about 49 pings a week between them to 3, and none has taken a callout since 19 Aug. Their handlers' phones "never go off", and ~30 tickets are open and unanswered. These four also vanish into the overall numbers, which look as if they are recovering partly because the four have dropped out.

**Why the four's pings have not come back.** Routing ranks responders partly on recent acceptance. A missed ping counts against a responder as much as a turn-down (-0.12, against +0.08 for a take), and scores never decay (a 2019 TODO). So after a run of misses a responder is ranked lower, gets fewer pings, and has no way to climb back. Why these four missed so many pings in the first place (59%, against 14% for the other 12) is still unexplained. Nobody sees this happening, and handlers find out when their responder's phone stops going off.

**What we propose.** You asked us not to quietly change a number. This proposal **keeps the 60 s wait and does no automatic reset of the four**. It fixes the loop that keeps their scores low and their pings down, and makes the problem visible to the people it happens to:

1. **Scores recover over time (decay).** An old miss counts for less each week.
2. **A missed ping costs less than a turn-down.** A turn-down is a decision; a miss may just be a phone not in hand.
3. **Handlers see a flag** when a responder gets far fewer pings than usual, and can **request help** from Rook Support. There is no automatic or immediate reset.
4. **A weekly ping report per responder** gives handlers 8 weeks of pings received, taken, missed and turned down, so they can see what is happening over time.
5. **Responders see the same flag on their phone**, and see what happened to a ping they didn't answer in time.

Items 1 and 2 stop new cases. Item 3 is how people who are already stuck, like the four, get help, because decay alone will not rescue them (see risks).

## What changes for responders

- **A "You are receiving fewer pings than usual" card in the phone app** when their pings fall below the threshold. It shows pings this week against their usual, and the date of their last ping and last callout taken. It does not blame them, and it shows nothing about other responders.
- **An "Ask for a review" button on the card.** It sends a help request to their handler and Support, so they know someone is looking.
- **A clear ending for a lost ping.** When a ping expires, the phone says "This callout was offered to the next responder after 60 seconds" and does not just make it vanish. A countdown on the ping shows how long is left. This covers the 15 "gone before I could answer" tickets.
- **One missed ping stops being a lasting mark.** After a rough week they get pinged again as their score recovers.

## What changes for handlers (Kip, Aunt Dot, Ambrose)

- **A "Receiving fewer pings" flag in the console** next to the responder, once their pings fall below the threshold. It shows pings this week against the responder's usual, and links to the weekly report.
- **A weekly ping report for each responder, always available** (not only when flagged). It covers the last 8 weeks and shows, per week, pings received and how many were taken, missed and turned down, as a table and a simple chart. The flag links to it. Handlers can see for themselves what is happening for a responder over time, without raising a ticket. Kip can compare Meteor Mite and The Gale side by side.
- **A "Request help" button on the flag.** It opens a Support case with the responder's recent history attached. Support looks at the case, answers the handler, and decides what to do. Whether that includes a score reset is Support's call, case by case.
- **A status on the request** (open, being looked at, done). This replaces the 28 "phone never goes off" tickets that are still unanswered.
- **The flag is per responder, not per console.** Kip has two responders (Meteor Mite and The Gale).
- **Load balances over time.** Kip's The Gale rose from 13 to 21 pings a week while Meteor Mite fell to 1. Recovery should rebalance this.

## What changes in routing

| Setting | Now | Proposed (to tune) |
|---|---|---|
| Ping wait | 60 s | 60 s, unchanged |
| Miss penalty | -0.12 (same as a turn-down) | smaller, e.g. -0.06 |
| Turn-down penalty, take reward | -0.12, +0.08 | unchanged |
| Decay | none | recent history counts fully, older history fades; half-life around 14 days |
| Weights (0.60 / 0.25 / 0.15) | as shipped in 4.2 | unchanged |

Values are illustrations, not tuned. Routing config ships with a release, so this goes in the next one.

**Flag threshold (illustrative):** trailing 7-day pings below 50% of the responder's own previous 4-week average. The four fell by about 94% and would all have been flagged. Ashgrove and Halfmoon dipped by about 25% and would not.

## How we'll know it's working

| Signal | Now | Target |
|---|---|---|
| Missed share of pings | 13-21% | falls, but the 60 s wait keeps it above the old 2-3% |
| Responders with a miss-driven drop in pings | the four | none new; flagged ones recover |
| Flagged responders with a handler request in 3 days | no flag exists | most of them |
| Support requests answered within 2 working days | 28 "phone never goes off" tickets, all open | 100% of new requests |
| Flagged responders back above threshold within 2 weeks of a Support case | unknown | set after the first cases |
| New "phone never goes off" and "gone before I could answer" tickets | 45 routing tickets since 12 Aug | falling week on week |
| Taken rate and never-taken callouts | 66-73% taken; 11.1% never taken (was 5.5%) | taken rate up toward 75%; never-taken down |

## Risks and open questions

- **Decay will not rescue the four on its own.** In the replay they kept missing into September and recent misses count in full, so their scores were still very low under every half-life tested (Meteor Mite especially). Without a reset they depend on the flag and Support. A lighter miss penalty may help, but we have not tested it.
- **Replay before shipping.** The penalty and decay values need replaying on the pings data first. Not a true counterfactual: responders would be pinged differently, and their answers are unknown.
- **Why the four miss 59% of pings (14% for the others) is still unexplained.** Nothing in the data covers device delivery or answer time. If the cause is outside routing, none of this fully fixes it.
- **Keeping 60 s means the misses stay higher for everyone.** The "gone before I could answer" experience is explained to responders but not removed. Why is 90 s off the table? Knowing would help us judge what else is possible.
- **Support has to do something.** What can Support do for a flagged responder without a reset available to them? We need Support and engineering to agree this, and the load is unknown.
- **Weights.** In the replay the old weights cut the four's pings just as much, so leaving them alone is supported. Was the 12 Aug weighting a decision or an accident? Marcus's 14 Aug question is still open.
- **Decay compresses differences.** A short half-life makes all responders look more alike, so proximity matters more. Check The Gale, Nightwell and Stormwrack for overload.
- **Report access and data.** Each handler sees only their own responders (each responder has one handler). Counts only, and no cover identities (Security Policy 4.1). The pings table already holds the outcomes, so the report needs no new data, but we have not checked how it would be built into the console.
- **Alert fatigue.** A threshold set too low flags normal low-ping weeks. Trial it on past data first.
- **Responder wording.** They are independent, not employees. The card must not read as a performance warning. Sofia should review the copy.
- **Confidentiality.** Nothing in the flag or a Support case should touch cover identities (Security Policy 4.1). Check this before building.
- **Production behaviour is unknown:** whether scores reset when the service restarts, and whether console filters affect availability.
