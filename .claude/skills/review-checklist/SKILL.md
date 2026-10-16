---
name: review-checklist
description: Reviews a product brief against the PM's four-point checklist (owner named, success measure, scope matches start to end, problem before proposal). Use when the student says "review this brief", "run the review-checklist", or points at a brief file or folder of briefs.
---

# review-checklist

Run the same four checks on every brief, every time. The user should never have to explain the checklist again.

## Input
A brief file, several files, or a folder (for example `06-sidekicks/briefs/`). If none is given, ask which brief. A brief may also be a wiki page; read it with `read_page`.

## The four checks
Read the whole brief before judging. For each check, answer yes or NO, and quote or point to the exact line that decided it.

1. **Owner named**: the brief says who owns it, by person, role or team. Passing the work to "whoever picks it up", "next quarter" or "nobody's picked it up yet" is NOT an owner.
2. **Success measure**: the brief says how we will know it worked. It must say how someone would tell, not only what gets built or logged. A measure with no baseline still counts, but note the missing baseline.
3. **Scope matches start to end**: compare what the brief says it will do at the start (proposal, "that's the whole ask") with the scope stated or implied at the end. NO if items get added mid-document or the closing scope is wider than the opening one. NO if there is no scope statement and the proposal itself grows.
4. **Problem before proposal**: the brief explains the problem before it proposes a fix. NO if it opens with the solution and the problem shows up later, or is never stated.

## Rules
- Judge only what the brief says. Do not fill gaps from other sources or guess an owner.
- Do not rewrite the brief or fix it unless asked. Flag, don't repair.
- Real-world names in the brief are fictional course data; do not try to work out who anyone is.

## Output
For each brief, in this format:

```
<brief title or filename>
  owner named ................. yes / NO — flagged
  success measure ............. yes / NO — flagged
  scope matches start to end .. yes / NO — flagged
  problem before proposal ..... yes / NO — flagged
  -> N flag(s): one or two sentences per flag, quoting the line behind it.
```

End with one summary line: `X briefs checked, Y flagged.` When there are several briefs, add which check failed most often.
