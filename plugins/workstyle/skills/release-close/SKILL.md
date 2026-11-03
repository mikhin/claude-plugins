---
name: release-close
description: Check a release by facts, not statuses — every issue of the Linear project against its PRs in every project repo, everyone's issues included — and list the tails with an action for each. Read-only until the user acks. Triggers — "/release-close", "is the release clean", "what is left in the release", "is everything in sync", "check the release by facts", "are the labels right", "close the release".
---

Read-only. Linear status changes, comments and git cleanup happen only after the user's ack.

The org, repos, Linear team and release conventions come from `~/.claude/workstyle-context.md`. A value missing there → ask once and add it.

## 1. The release

The Linear project by its id, since projects get renamed. Its branch: `release/<slug>`, check with `git ls-remote --heads origin 'refs/heads/release/*'` in each repo that has release branches. Has `release/*` → `main` merged yet? (`gh pr list --base main --state merged --search "head:release/<slug>"`.)

## 2. Every issue, any assignee

`list_issues` by project. For each issue find its PRs in all project repos: Linear attachments, the branch name (`gitBranchName`), `gh search prs --owner=<org> "<issue key>"`.

Facts per PR: state, base branch, merged into what, CI, unresolved review threads (with pending reviews, through GraphQL).

Status vs fact — each mismatch is a tail:
- Done, but a PR is open, closed unmerged, or merged into the wrong base.
- In Progress or In Review, but every PR is merged.
- In Progress with no PR and no push for 3+ days.
- Merged into `release/*`, but the commit is not in `main` after the release shipped (`git branch -r --contains <sha>`).
- Labels (frontend / backend / contract) that do not match the repos the PRs are in.
- A Done or Canceled issue in a backlog project → move it to the release it shipped with.

## 3. Tails outside the issues

- Open PRs into `release/<slug>` that no issue links to.
- A contract PR merged without its frontend or backend counterpart.
- Release branches left after the merge, branches of merged PRs.

## 4. Answer

```
| issue | status | fact | action |
```

Only rows with a mismatch. Then the actions grouped by who does them: the user / others (what to ask, and whom). The person's name, never "someone".

After the user's ack: Linear changes in one pass (`project` and `state` in the same save), and git cleanup through `workstyle:prune`.
