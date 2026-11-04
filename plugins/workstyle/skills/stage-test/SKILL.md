---
name: stage-test
description: Test a release on staging with two separate subagents — one verifies release readiness across Slack, Linear, GitHub and deployments and saves a TXT test plan; the other executes it and reports release blockers. Triggers — "/stage-test", "test it on staging", "is it safe to merge to prod", "we merged, check it", "reproduce the bugs from this file".
---

## Orchestration

Run two sequential stages in separate subagents. The parent coordinates, checks the handoff and reports the result; it does not silently replace the agents with its own browser walkthrough. A normal request to test staging starts this workflow without asking the user to describe it again. If delegation is unavailable, say so and agree on a fallback before proceeding.

Read project instructions, project memory and `~/.claude/workstyle-context.md` for repositories, release process, staging URL or command, logs and output folder. Verify mutable facts. Look in connected sources before asking for missing context. Never put credentials or private project configuration into this skill.

Testing does not authorize merging, deploying, fixing code, closing tickets or sending messages. Use the browser the user selected and its current skill. Check the connection and sign-in before promising a run; do not substitute a different browser or local build without agreement.

## Stage 1 — preparation agent

Give the agent the requested release or bug file and the project context paths. Its job is read-only reconnaissance plus the handoff file, not running the test cases.

1. Establish scope from the release project, the last recorded check and the user's request. Read Linear issues, comments, linked specs and Figma; Slack decisions and unanswered release questions; GitHub PRs, reviews and checks across frontend, contract, backend and other affected repos. Prefer the latest decision over superseded ticket wording. List unavailable sources as gaps, not as checked.
2. Reconcile each in-scope requirement with its implementation and dependencies. Find open or unmerged PRs, unresolved reviews, failed checks, unfinished tickets, missing decisions and cross-repo dependencies. A Done ticket or green CI alone is not evidence of readiness. Separate unrelated backlog from release blockers; do not silently drop disputed work from scope.
3. Verify what actually runs on staging: URL, frontend build/commit, backend deployed commit, deployment success and timestamps, and the applicable contract revision. Compare these with the intended release heads, including extra or missing commits. An open PR may already be deployed; report that distinction. Shared staging may have been overwritten by another release. A local frontend against a staging API is not the deployed frontend; record which is being tested and check local branch and dirty files if that setup is intended.
4. Check browser availability, authentication, test accounts/roles, suitable test data and required integrations. State which cases need user action or unavailable access. Do not infer that all staging data is disposable; use the project's explicit testing policy.
5. Build cases from acceptance criteria, current specs, designs and known regressions. Each case has a stable ID, source link, role, prerequisites/data, steps and observable expected result. Cover roles that differ, edge cases and read-only states. A user-supplied bug file defines the requested cases; preserve its identifiers.

Save a plain TXT plan using `workstyle:desk` conventions: reuse the release's output subfolder, otherwise create one under the configured output folder (`~/Desktop` by default). Read an existing file before updating it. Use `staging test plan.txt`; do not overwrite a different release's plan.

The file must let a fresh agent work without the preparation conversation:

- Release scope, check time, source links and local context paths.
- Intended and deployed commits per repo, staging URL/setup and deployment evidence.
- Readiness: READY or BLOCKED, with every blocker, remaining question and unchecked source. For each blocker, say what evidence would clear it.
- Numbered test cases, role/access prerequisites and evidence to capture. No passwords, tokens or full credentials.

Return the absolute plan path and readiness verdict to the parent. Save the plan even when blocked.

## Handoff gate

The parent reads the plan and checks that readiness is supported by evidence and the cases cover the agreed scope. Missing preparation goes back to the preparation agent. When BLOCKED, give the user the plan and concrete blockers; do not start the full test run. If the user explicitly requests a partial run, pass its limitations to the testing agent and keep blocked cases visible. No release-safe verdict from a partial run.

## Stage 2 — testing agent

Start a different subagent with the absolute plan path, this skill, applicable project/browser instructions and the user's constraints. The saved plan is the handoff; do not require the tester to reconstruct the preparation chat.

Before the first case, recheck staging identity and access against the plan. If the deployed code or prerequisites changed, return the mismatch for preparation to update. Do not test a different build under the old plan. Record a deployment change during the run and reassess affected results.

Execute every case in the selected browser, using a new tab unless the user asked for an existing one. Apply the app's known testing quirks from memory. For changed UI, check real renders at the project's viewports and against the referenced design. Never count mocked/local tests as evidence of deployed behavior.

Record pass / fail / blocked / not run per case. A failure includes reproducible steps, expected and actual behavior, a screenshot and relevant request/response or logs when available. Redact credentials and unrelated private data. Attribute backend vs frontend vs contract only when evidence supports it; otherwise mark the side unknown. Missing access is blocked, not pass.

Save `staging test results.txt` and evidence beside the plan, with tested commits and run time. A user bug file is also marked in place as requested. Report actual coverage and remaining blockers; do not fix or publish anything as part of the run.

## Final result

The parent checks the results against every planned case and links both TXT files and failure evidence. Summarize:

```
| # | case | role | result | side |
```

Then: "prod: safe / not safe / readiness unverified — <reason>". Safe requires completed relevant cases, verified deployment identity and no unresolved release blockers. A successful browser connection or a few passing smoke tests is not a completed release check.
