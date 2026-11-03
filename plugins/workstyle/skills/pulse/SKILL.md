---
name: pulse
description: One teammate's activity day by day across GitHub, Linear and Slack, compared with their own usual pace and their release scope — evidence, not judgement — and a non-toxic way to ask. Triggers — "/pulse <name>", "what did <name> do", "is <name> slow", "<name>'s activity", "how do I ask without being toxic".
---

Read-only. The org, repos and channels come from `~/.claude/workstyle-context.md`.

## 1. Who and when

The person: the GitHub login (`gh api orgs/<org>/members`), the Slack id (`users_search`), the Linear user. The period: the one the user named, else the last 10 working days. Mind the person's time zone when splitting days.

## 2. Per day

- GitHub, all project repos, all branches: commits (`git log --all --author` in the local clones after `git fetch`, `gh search commits --author`), PRs opened and merged, reviews and review comments.
- Linear: status changes (`stateHistory`), comments, issues created.
- Slack: messages in the project's channels (`filter_users_from`), threads they answered.

## 3. Compare

- With their own pace in the previous weeks, not with someone else's.
- With their scope: the open release issues assigned to them, and the dates the release needs.
- Gaps: name the days without a trace. Few commits does not mean no work: calls, research and local work leave no trace. Say what the data cannot see.

## 4. Answer

```
| day | commits | PRs | reviews | linear | slack | what |
```

Then one line of conclusion with the evidence, and how sure it is. No labels like "lazy" or "slow" without numbers.

When the user asks how to ask them: a draft in the user's voice (`workstyle:comms`). Curiosity, not control. Ask about the work, not the person: "how is <issue> going, where can I help?". One question.
