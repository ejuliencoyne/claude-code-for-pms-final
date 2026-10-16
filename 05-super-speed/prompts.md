# 05 · Super Speed — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent three sessions finding out what went
wrong: the two piles of feedback that didn't agree, the numbers that
hid four people inside an average, and the code that settled it —
the shorter timeout is what did it.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

This will be for module 5

### 2.

let's just do the 1-pager for now. start with an executive summary, then how it will affect both responders and handlers, how we'll know it's working, and finish with additional risks and questions.

### 3.

it sounds like for whatever reason helen doesn't want to change back to 90 seconds (she is not a real person so I can't ask for further details). let's also propose a ui change for the responders, not just the handlers. it would also be helpful to not just reset the responders, but instead give the handlers a way to request a reset for their responder (would require intervention from support at rook perhaps). so there would be no manual reset right away, we would just show the handlers a flag that their responder was no longer recieving very many requests once they were below some threshold, and give them an option to request assistance with this

### 4.

no, I think we should keep score decay and a lighter penalty for a miss. just no change to the 90 second timeout and manual responder reset for the four affected.

### 5.

"Executive summary" doesn't give a good summary of what is happening - "most responders recovered" - recovered from what?

### 6.

change to "four responders who have not started recieving pings again since the 4.2 changes" not "gone silent" which sounds wishy washy

### 7.

let's change handler flag from "going quiet" to something more concrete about recieving fewer pings

### 8.

yes let's update everything to use the more concrete wording instead of "quiet"

### 9.

let's also change the word "starved" which sounds silly and wishy washy in an official readout

### 10.

let's also make a report available to handlers of each responder of pings recieved and accepted/missed/declined per week over the last two months so they can see what is happening for each responder over time

### 11.

Sounds good. now let's condense this whole report so it is less than a page

### 12.

remove the note about marcus in risks please

### 13.

update "Four responders (Farlight, Meteor Mite, The Undertow, Vesper) whose pings have not come back since 4.2." to something more like "four responders pings decreased sharply and have not come back"

### 14.

Let's make these formatting changes to the proposal: categorize the 3 UI changes and call out that category, and do the same for the first two routing logic changes

### 15.

Make the UI changes a list formatted like the routing logic changes with numbers 1-3

### 16.

let's actually remove the support request and let the handlers contact support on their own. but we should call out support needing a way to reset responders in the risks section

### 17.

Please update "Decay alone won't free the four" to something more concrete

### 18.

you can keep the old shorter text, and say "decaying miss and decline penalties would not have brought the four's ping count back up"

### 19.

Remove "why is 90s off the table?"

### 20.

Take the brief you just wrote and build me two working prototypes, one for the handler perspective and one for the responder perspective, actual screens I can click through, not a description of one. Save them as 05-super-speed/prototype-handler.html and 05-super-speed/prototype-responder.html, files I can just open in my browser. Show me where this would actually happen, and make at least one thing on it respond when I click it.

### 21.

let's change the responder prototype a bit. let's have the card just be "recent activity" and show pings this week, last ping, last accepted callout, and the "see my last 8 weeks". remove the "not a warning, talk to your handler" message

### 22.

the expired ping screen is good. let's update the UI of the recent activity box to have the three values be of similar weight in the display

### 23.

now instead of last 8 weeks, let's have it say "see recent pings" and have it show the last 10 pings for the responder, with date, time, and accepted/missed/declined (no color indicator for whether it was accepted missed or declined)

### 24.

great. now let's have the screen when a ping is recieved show any previously missed ones until they are cleared when a new one comes in

### 25.

sorry that's not what I meant. the home screen should not change. I would like to make changes to the screen that shows when a ping comes in

### 26.

ok, instead of the missed ping message, please continue to show any "This callout has been offered to the next responder" boxes until the responder clicks "ok". And instead of "this callout", say "a callout on (date, time) has been offered to the next responder"

### 27.

great, looks good. does the brief match what was built in these two html files?
