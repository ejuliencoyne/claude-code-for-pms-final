# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.
Please look at the code changes that were included in 4.2 and give a summary of what the changes are. Quote every section of the code that changed in this release

### 2.
what does poll_device do?

### 3.
how about available_for?

### 4.
how does the change in weight_proximity relate to the data we saw in the pings table?

### 5.
sure, do both, compare ping order within a callout for responders in the same area, and look at who was pinged first in callouts where the nearest responder had a recent miss

### 6.
yes please run the simulation to simulate the ranking weights

### 7.
were there any UI changes in 4.2? (indicated in either code or ticket increases)

### 8.
yes please look at all tickets

### 9.
can you remind me of the engineering manager's question about the 4.2 release on the release page?

### 10.
it seems like from everything we've looked at, the only thing that concretely seems like it would likely help with the starved responders is increasing the timeout time back to 90s. does that align with the data we have looked at so far?

### 11.
it seems like the increased misses from the four could be related to some kind of ui change or something about the filter persistence changes, which I still don't understand that well. Can you get any other information about what filter persistence changes in 4.2 means?

### 12.
just a note, this whole team is fictional, so there are no people that can be asked

### 13.
thanks - is there anything we haven't discussed yet that could separate the 4 starved responders from the rest?

### 14.
remind me of the handlers for the four starved responders?

### 15.
can I read aunt dot and kip's full interviews?

### 16.
and can you give me a summary of linda and desmonds tickets again with specific quotes that might indicate an issue with their responders' devices?

### 17.
so all of this doesn't give any indication of what could have caused the sudden increase in misses for any of the four, correct

### 18.
are there any other periods where any responders had a spike in misses before or after 4.2?

### 19.
ok great. so it sounds like maybe those 4 just had some kind of less fast behavior that caused them to have a spike in misses, and that small spike put them far enough down in rankings that they continued to miss. Probably setting back to 90 seconds, resetting those 4's scores, and speaking with their handlers would be the best approach? does it seem like anything needs to be done in regards to any of the other responders?

### 20.
right, but there wasn't any change in removing a decay in 4.2, was there? the status quo before the change didn't seem like it was causing any spirals even without decaying penalties

### 21.
I suppose also if we want to avoid manual intervention to future proof the solution, it might be better to not do a manual reset and instead implement the decaying penalties so that it would effectively reset the four as their older penalties would reduce in weight, and would also make it so this was less likely to happen in the future

### 22.
that would be great, but please retroactively apply the decay to old misses (so misses from 4 weeks ago count has having already decayed by those 4 weeks today)
