---
name: org-research
description: Research one topic across the whole project — every repo of the GitHub org (code on every branch, issues, PRs, review threads), all of Linear (issues, comments, documents, projects) and the project's Slack channels and DMs — and write a dated, linked txt report to the Desktop. Read-only. Triggers — "/org-research <topic>", "research <topic> across the project", "dig up everything about", "what did we have on", "where was this decided".
allowed-tools: Agent, Bash(gh:*), Bash(git:*), Bash(curl:*), Bash(cat:*), Write, mcp__slack__conversations_search_messages, mcp__slack__conversations_replies, mcp__linear__list_issues, mcp__linear__get_issue, mcp__linear__list_comments, mcp__linear__list_documents, mcp__linear__get_document, mcp__linear__list_projects, mcp__linear__get_project, mcp__linear__get_status_updates
---

Read-only everywhere: no comments, reactions, status changes, pushes. `git fetch` is the only write, and it touches local refs only.

The org, repos, local clones and Slack channel ids come from `~/.claude/workstyle-context.md`. A value missing there → ask once and add it.

## 0. Terms

Turn the topic into search terms: words in every language the team writes in, synonyms, misspellings people actually use, code identifiers, wire names that differ from the UI word. Show the list in one line and go on.

## 1. Round one — three agents in parallel

One `general-purpose` agent per source, in one message. Each gets the terms and returns facts only: date, author, one-line claim, link. Plus a list of new terms it met — issue keys, PR numbers, identifiers, names of things. No conclusions.

**GitHub** — the org, all repos including archived (`gh repo list <org> --json name`).
- Issues, PRs and their comments: `gh search issues --owner=<org> --include-prs "<term>" --limit 100 --json repository,number,title,url,state,createdAt`.
- For each hit, the whole conversation and review threads with pending reviews:
  `gh api graphql -f query='{repository(owner:"<org>",name:"<repo>"){pullRequest(number:N){body comments(first:100){nodes{author{login} body createdAt url}} reviewThreads(first:100){nodes{isResolved comments(first:50){nodes{author{login} path body createdAt url}}}} reviews(first:50){nodes{state body author{login} submittedAt}}}}}'`
- Code on every branch, from the local clones (`gh search code` sees only the default branch, and work lives on `release/*`):
  `git -C <clone> fetch -q --all && git -C <clone> grep -il "<term>" $(git -C <clone> for-each-ref --format='%(refname)' refs/remotes) | sort -u`
- When something appeared or vanished: `git -C <clone> log --all -S "<term>" --format='%h %ad %an %s' --date=short`, and commit messages: `git -C <clone> log --all -i --grep="<term>" --format='%h %ad %an %s' --date=short`.
- Repos with no clone: `gh search code --owner=<org> "<term>"`.

**Linear**
- Titles and descriptions: `list_issues` with `query`, `includeArchived: true`, `limit: 250`.
- Comments — the MCP does not search them. GraphQL does:
  `curl -s https://api.linear.app/graphql -H "Authorization: ${LINEAR_API_KEY:-$(eval "$LINEAR_KEY_COMMAND")}" -H 'Content-Type: application/json' -d '{"query":"{ searchIssues(term:\"<term>\", includeComments:true, first:100){ nodes{ identifier title url } } }"}'`
- Every hit: `get_issue` with `relations`, `attachments`, `documents`, `stateHistory`, then `list_comments` with `issueId`.
- Documents: `list_documents` with `query`, `includeArchived: true`, then `get_document`.
- Projects: `list_projects`, then `get_project` and `get_status_updates` for the ones that touch the topic; `list_comments` with `projectId`.

**Slack** — the project's channels and DMs.
- `conversations_search_messages` with `search_query: "<term>"`, `limit: 100`, page with `cursor` until empty. Keep rows whose Channel is one of the project's channels or a DM/MPIM (`D…`, `mpdm-…`); drop the rest of the workspace.
- Every hit with a thread: `conversations_replies` with its `ThreadTs` and `limit: "1000"`. The default `limit` is `1d` and silently cuts older replies.
- A reply rarely repeats the topic word, so a thread counts as a whole, never by its matching line.

## 2. Further rounds

Search the new terms from all three agents in the other sources (an issue key from Slack goes to GitHub and Linear, a PR number from Linear goes to Slack). Run the agents again with only the new terms. Stop when a round brings no new fact.

## 3. Report

Write `~/Desktop/research-<topic-slug>.txt`, plain text, no markdown:

```
<topic> — <date>
terms: …

timeline
YYYY-MM-DD  <who>: <what was decided or changed>  <link>
…

now
<the state today, each claim with its link>

contradictions
<a source that says A against a source that says B, both links>

open questions
<asked and not answered, with the link to where it was asked>

searched, nothing found
<source: terms>
```

Every fact carries a link and a date. A fact found in two places is one line with both links. A source that failed (auth, rate limit) goes under "searched, nothing found" with the error.

In chat: the file path and the three most important lines from "now".
