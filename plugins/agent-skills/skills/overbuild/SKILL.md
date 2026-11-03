---
name: overbuild
description: Review a diff for code that handles cases no user reaches or builds layers nobody needs — guards, queues, refs, counters, outputs nothing reads. Run on the branch diff as part of every review, and on every review round. Triggers — "overbuild", "/overbuild", "is this overbuilt", "what in this diff is extra".
---

# Overbuild

Read `git diff $(git merge-base HEAD origin/<target branch>)...HEAD`. For each hunk that adds a branch, a guard, a ref, a queue, a retry, a counter, a cache or an output, answer three questions. A hunk that cannot answer is a finding: cut it.

1. **What reaches this branch?** Cite the contract response (a field the spec marks optional, a status it documents) or the user action (a click, a keystroke, a route) that gets here. "Defensive", "just in case" and "the type allows it" are not answers — generated types follow the contract; a required field is present.
2. **What already covers it?** Find the project's existing mechanisms first: a submit lock (a write runs once), a pending marker (an element waits for its write), a flush (an action waits for the writes before it), an idempotent retry, the refetch after a write. The project's CLAUDE.md usually names them. A second mechanism next to one of these is a finding.
3. **Can the UI produce the test's interleaving?** A test that fires two edits in one `act`, remounts with the same token, or resolves promises out of order proves the code handles that order — not that a user can cause it. If the control is locked while its save is in flight, the interleaving cannot happen; drop the test and the code it guards.

## Findings that add code

A finding that proposes a guard names the normal path that reaches it — the user steps or the response, in order. No path, no guard.

## Rounds

Each round prints `git diff --shortstat` of the branch against the stat recorded in round one. A round that grows the diff to fix the previous round's guard is the pattern this skill exists to stop: step back and replace the mechanism with a simpler one (a lock while saving, a value derived in render) instead of guarding the guard.

## Output

One line per finding: `file:line — what to cut — what covers it instead`. Then the shortstat line. Nothing else.
