---
name: report
description: Draft the user's project update for the client channel covering every working day since the last one, from GitHub, Slack and Linear activity. Draft only, never posts. Triggers — "/report", "write my update", "daily report", "report for the last days".
allowed-tools: Bash(gh:*), Bash(date:*), Bash(cat:*), Skill, mcp__slack__conversations_search_messages, mcp__slack__conversations_replies, mcp__linear__list_issues
---

The org, the user's GitHub login, the report channel, the user's Slack id, the time zone and what to leave out come from `~/.claude/workstyle-context.md`. A value missing there → ask once and add it.

Load the `workstyle:comms` skill first. Its rules hold for the whole draft.

## 1. Find the gap

Search the user's messages in the report channel with no `search_query`, `filter_users_from: "<user's Slack id>"`. A report is a top-level message (`ThreadTs` empty or equal to its own ts) that sums up a day, whatever its opening words.

- The latest report closes the day it was posted, in the context's time zone.
- The gap is every Mon–Fri after that day, up to and including today.
- No gap → say so in one line and stop.

Keep the last two reports. They set the tone, the header and the level of detail.

## 2. Collect activity for the gap

`SINCE` is the first day of the gap. Run the sources in parallel.

GitHub:
```
gh search prs --owner=<org> --author=@me --merged-at=">=SINCE" --json repository,number,title,url,closedAt
gh search prs --owner=<org> --author=@me --created=">=SINCE" --json repository,number,title,url,state,createdAt
gh search prs --owner=<org> --reviewed-by=@me --updated=">=SINCE" --json repository,number,title,url,author,updatedAt --jq '[.[]|select(.author.login!="<user's login>")]'
```

Linear, `list_issues`:
- `assignee: "me"`, `updatedAt: SINCE`, fields `title, url, status, completedAt, updatedAt`;
- `creator: "me"`, `createdAt: SINCE`, same fields.

Slack: the user's messages since `SINCE` in the project's channels, `filter_users_from: "<user's Slack id>"`. These show the calls, decisions, reviews and onboarding that leave no PR or ticket. Read a thread with `conversations_replies` only when the message alone does not say what was decided.

## 3. Draft

- Put each item on the day of its timestamp, in the context's time zone.
- One item per outcome. A PR and its Linear issue are one item, linked by the issue.
- Say what changed for the reader, not what was done to the code.
- Leave out team-internal matters: staffing, billing, the user's employer's process.
- `next:` comes from the user's Linear In Progress and Todo issues. Both empty → leave the line for the user to fill in.

One gap is one message, the shape of the last reports: one day is "update for today:", several days get a section per day, a full week is "update for this week:".

Show the draft ready to paste. Never post it.
