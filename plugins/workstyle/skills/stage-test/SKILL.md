---
name: stage-test
description: Test what was merged into a release on staging — collect the merged PRs across contract, backend and frontend, build a test plan from the issues, run it in Chrome, mark each case, and say whether main is safe to merge. Triggers — "/stage-test", "test it on staging", "is it safe to merge to prod", "we merged, check it", "reproduce the bugs from this file".
---

How to run staging and where its logs are comes from `~/.claude/workstyle-context.md`. A value missing there → ask once and add it.

## 1. What landed

The release branch (`release/<slug>`) and the merged PRs in every project repo since the last check, or the set the user named. Also the open PRs the tickets still wait for.

Is staging running that code? Frontend: the staging command from the context, on the release branch. Backend: the commit deployed on staging (deploy logs). Staging is not running it yet → say so and stop.

## 2. Test plan

From the issues' acceptance criteria, the spec scenarios in Linear documents, and the Figma frames. Per case: role, steps, expected result. Every role that sees the feature differently gets its own case. Include edge cases the tickets name, and the read-only states.

A file of bugs from the user → that file is the plan.

## 3. Run

Claude in Chrome, a new tab. Known testing quirks of the app under MCP are in memory. Staging data is fake: no cleanup after.

Per case: pass / fail. On fail: a screenshot, the request and response, and whose side it is. The response differs from the contract → backend. The UI renders a correct response wrong → frontend. The contract itself is missing something → contract.

## 4. Answer

```
| # | case | role | result | side |
```

Then: "prod: safe / not safe — <blockers>". Fail screenshots → `workstyle:desk`. A bug file from the user → mark each line in that file in place.
