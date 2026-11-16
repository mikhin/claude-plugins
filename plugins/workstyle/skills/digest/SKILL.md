---
name: digest
description: Catch up on the whole project for a period (default the last 7 days) — Slack channels, DMs and threads, Linear issues created or changed with their comments, GitHub PRs and reviews, past and coming meetings from the calendar, Zoom meeting summaries — grouped by topic into one dated, linked txt on the Desktop, with what waits for the user on top. Read-only. Triggers — "/digest", "/digest 14d", "catch me up", "what happened this week", "weekly digest", "what did I miss".
allowed-tools: Agent, Bash(gh:*), Bash(git:*), Bash(jq:*), Bash(date:*), Write, mcp__slack__conversations_history, mcp__slack__conversations_replies, mcp__slack__conversations_search_messages, mcp__linear__list_issues, mcp__linear__get_issue, mcp__linear__list_comments, mcp__linear__list_projects, mcp__linear__get_status_updates, mcp__linear__list_documents, mcp__linear__get_document, mcp__claude_ai_Google_Calendar__list_events, mcp__claude_ai_Zoom_for_Claude__search_meetings, mcp__claude_ai_Zoom_for_Claude__get_meeting_assets
---

Read-only everywhere: no comments, reactions, status changes, marks as read, pushes. `git fetch` is the only write, and it touches local refs only.

The org, repos, local clones, Slack channel ids, the user's ids, time zone and output folder come from `~/.claude/workstyle-context.md`. A value missing there → ask once and add it.

## 0. Period

The argument is the period: `7d` by default, `14d`, or a date `YYYY-MM-DD` to start from. Compute `<since>` as a date in the user's time zone and show it in one line. The calendar also looks ahead: next 7 days.

## 1. Collect — four agents in parallel

One `general-purpose` agent per source, in one message. Each gets `<since>`, the context values, and returns facts only: date, author, one-line claim, link, plus the keys it saw (issue keys, PR numbers). No conclusions. Each marks what waits for the user: a question or mention addressed to them with no answer from them, a review requested from them, an issue assigned to them that changed.

**Slack** — the project's channels and DMs.
- Each project channel: `conversations_history` with `limit` set to the period (`7d`), paging with `cursor`.
- DMs and group DMs: `conversations_search_messages` with `filter_date_after: <since - 1 day>` and `filter_users_with` set to each person who wrote in the project channels this period, `limit: 100`, paging. Keep rows whose Channel is a DM or MPIM; drop the rest of the workspace.
- Every message with a thread, and every thread a hit belongs to: `conversations_replies` with `limit: "1000"`. The default `limit` is `1d` and silently cuts older replies. A thread started before `<since>` with replies after it counts.

**Linear** — the project's team.
- `list_issues` with `team`, `updatedAt: <since>`, `includeArchived: true`, `limit: 250`, paging with `cursor`. Mark each as created or changed (`createdAt` against `<since>`).
- Every issue: `list_comments` with `issueId`; keep comments from the period. For a status change, `get_issue` with `stateHistory`.
- Projects: `list_projects` for the team, `get_status_updates` for the period.
- Documents: `list_documents` updated in the period, `get_document` for what changed.

**GitHub** — every repo of the org.
- `gh search prs --owner=<org> --updated=">=<since>" --limit 200 --json repository,number,title,url,state,author,createdAt,closedAt`, and the same with `gh search issues`.
- Each PR: conversation, review threads and reviews in the period:
  `gh api graphql -f query='{repository(owner:"<org>",name:"<repo>"){pullRequest(number:N){comments(first:100){nodes{author{login} body createdAt url}} reviewThreads(first:100){nodes{isResolved comments(first:50){nodes{author{login} path body createdAt url}}}} reviews(first:50){nodes{state body author{login} submittedAt}} reviewRequests(first:20){nodes{requestedReviewer{... on User{login}}}}}}}'`
- What landed: for each local clone, `git -C <clone> fetch -q --all` and `git -C <clone> log --since=<since> --merges --first-parent --format='%h %ad %an %s' --date=short` on `origin/main` and every `origin/release/*`.

**Meetings** — calendar and Zoom.
- Calendar: `list_events` from `<since>` to now plus 7 days. Keep project meetings: a project person among the attendees, or the project name in the title.
- Zoom: `search_meetings` from `<since>` to now, in UTC. For each project meeting with `has_summary_permission: true`, `get_meeting_assets` with its `meeting_uuid`. The answer is often too big and lands in a file; read only `jq -r '.meeting_summary.summary_markdown'` from it. With `has_summary` but no permission, record the meeting, its host, and "summary not shared" — the host's recap is often in the project channel, so look there before reporting it missing.

A source that fails (auth, rate limit, connector off) is reported, not skipped silently.

## 2. Merge

Group facts by topic, not by source: one Linear issue, one PR, one thread or one meeting decision is a topic, and the same key seen in two sources joins them (an issue key in a Slack thread, a PR linked from an issue, a meeting decision repeated in a channel). Topics with no key join by subject when they plainly are the same thing.

Order topics by what moved most. Drop bot noise (deploy notices, CI, link unfurls) unless it is the only signal of a release.

## 3. Report

Write `<output folder>/digest-<since>-<today>.txt`, plain text, no markdown, in the language the user writes in:

```
digest <since> — <today>

waits for you
<what is asked of the user and not answered: who, what, link>

decided
YYYY-MM-DD  <what was decided, by whom>  <links>

topics
<topic title> — <state now>
  YYYY-MM-DD  <who>: <what changed>  <link>
  …

meetings
YYYY-MM-DD  <title>, host <name>: <two lines of outcome or "summary not shared">  <link>

coming up
YYYY-MM-DD HH:MM  <meeting>

sources
<source: what was read, counts, failures>
```

Every line carries a date and a link. A fact found in two places is one line with both links.

In chat: the file path, the "waits for you" lines, and the three topics that moved most.
