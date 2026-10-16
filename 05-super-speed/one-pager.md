# Dispatch: no responder should stop receiving pings without anyone noticing

*For Helen Achebe · from the Dispatch PM · draft, October 2026*

## Executive summary

Release 4.2 (12 Aug) cut the ping wait from 90 s to 60 s and weighted proximity more heavily. Support tickets rose from 5-8 a week to 20-32. Two problems sit behind them:

- **Callouts gone before responders can answer.** Missed pings rose from ~2% to 12-21% for most responders (12 of 16) and have since eased. 15 tickets.
- **Four responders (Farlight, Meteor Mite, The Undertow, Vesper) whose pings decreased sharply and have not come back.** They went from ~49 pings a week between them to 3, with no callout taken since 19 Aug. ~30 tickets are open. A miss counts against a responder as much as a turn-down and scores never decay, so a responder ranked low has no way back. Why these four missed 59% of pings (others 14%) is unexplained.

**Proposal.** Keep the 60 s wait and do no automatic reset. Instead:

**Routing logic changes**
1. **Decay** old scores, so misses fade.
2. **Lighter penalty for a miss** than a turn-down (e.g. -0.06 vs -0.12).

**UI changes**
1. **Handler flag** when a responder receives far fewer pings than usual. Handlers contact Support on their own.
2. **Weekly ping report** per responder.
3. **Responder "Recent activity" card** on the phone, and a clear message when a ping expires.

## Handlers (Kip, Aunt Dot, Ambrose)

- **"Receiving fewer pings" flag** next to a responder when trailing 7-day pings fall below 50% of their previous 4-week average (illustrative; flags all four, not Ashgrove or Halfmoon).
- **No in-app request.** If a flagged responder needs help, the handler contacts Rook Support directly, with a "copy summary" button to paste the responder's recent numbers into the message. Support decides, including any reset, case by case.
- **Report, always available:** 8 weeks of pings received, taken, missed and turned down per week, as a table and chart.

## Responders

- **"Recent activity" card** with three equal figures: pings this week, last ping, last callout accepted. "See recent pings" lists the last 10 pings with date, time and accepted, missed or declined. Neutral wording, nothing about other responders.
- **Expired pings explain themselves**: a countdown while the ping is live, then "A callout on [date, time] has been offered to the next responder". The message stays, stacked, on the next incoming ping until the responder clicks OK, so a missed ping is not silently lost.

## How we'll know it's working

| Signal | Now | Target |
|---|---|---|
| Flagged responders whose handler contacts Support within 3 days | no flag | most |
| Handler contacts answered in 2 working days | 28 open tickets | 100% |
| Routing tickets (45 since 12 Aug) | rising | falling weekly |
| Taken rate / never-taken callouts | 66-73% / 11.1% | toward 75% / toward 5.5% |

## Risks and open questions

- **Decaying miss and decline penalties would not have brought the four's ping count back up**: they kept missing into September, and replay left their scores very low. They depend on handlers noticing the flag and contacting Support. Penalty and decay values are untested; replay on pings data before shipping.
- **Cause of the four's misses is unexplained**, with no device or answer-time data. None of this may fully fix it.
- **60 s stays**, so misses stay above the old 2-3%.
- **Support needs a way to reset a responder's score.** None exists today, and without one a flagged responder (including the four) cannot be helped. Needs engineering to build and agree; Support load unknown.
- **Responder wording** must not read as a warning (Sofia to review). The card no longer says "fewer pings than usual", so responders see their own activity but not the flag; only handlers are told. Nothing may touch cover identities (Policy 4.1). Trial the threshold on past data to avoid false flags.
