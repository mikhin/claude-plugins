---
name: next
description: Name the user's one next step on the current project — what blocks others first (reviews, unanswered mentions, stuck release issues), then the user's own work. Triggers — "/next", "what's next", "next step", "what should I do now".
allowed-tools: Bash(gh:*), Bash(cat:*), mcp__slack__conversations_search_messages, mcp__slack__conversations_replies, mcp__linear__list_issues, mcp__linear__list_projects, mcp__linear__get_issue
---

The org, channels and the user's Slack id come from `~/.claude/workstyle-context.md`. A value missing there → ask once and add it.

Run the six sources in parallel, then pick the first non-empty one, in this order. Work that blocks someone else comes before the user's own.

1. **PRs waiting for the user's review**, oldest first:
   ```
   gh search prs --review-requested=@me --owner=<org> --state=open --draft=false --sort=created --order=asc --json repository,number,title,createdAt,url
   ```
2. **Slack threads waiting on the user**, last 7 days, the project's channels, bot messages dropped. Two searches with `conversations_search_messages` and `filter_date_after` a week back:
   - mentions: `search_query: "<@<user's Slack id>>"`;
   - threads the user wrote in: `filter_users_from: "<user's Slack id>"` — a reply to the user's message rarely mentions them.

   Merge by thread (`ThreadTs`, or the message's own ts when it has none). Read each thread once with `conversations_replies`: it counts when the last human message is someone else's and still asks something of the user (a question, a request, a decision to make), even if the user replied earlier in the thread. A link, a thanks or an emoji-only reply does not count.
3. **Release issues waiting on the user**: `list_projects` for the started release projects, then `list_issues` by each project, any assignee. Check every issue's status against its PRs and threads. It counts only when it waits on the user: a PR of it needs the user's review or answer, it is blocked by the user's issue, it has no assignee, or its status contradicts the facts (In Progress with the PR merged, Done with the PR open, In Progress with no PR and no push for 3+ days). Others' issues that move on their own are not shown.
4. **The user's PRs with feedback**: changes requested, or unresolved review threads:
   ```
   gh api graphql -f query='{ search(query:"org:<org> is:pr is:open draft:false author:@me", type:ISSUE, first:30){ nodes{ ... on PullRequest { repository{name} number title url reviewDecision reviewThreads(first:100){ nodes{ isResolved } } } } } }'
   ```
5. **The user's Linear issues In Progress**: `list_issues` with `assignee: "me"`, `state: "In Progress"`.
6. **The user's Linear issues in Todo**: same, `state: "Todo"`, highest priority first. Then the user's Backlog issues from source 3's release projects — an issue planned into a release is work to take, whatever its status.

A PR and its Linear issue are one item — show it once, at the higher rung.

**Every PR in the list is checked on GitHub, whichever source brought it** — a Slack "ready now" is a claim, not a fact:
```
gh pr view <url> --json baseRefName,headRefName,isDraft,mergeable,commits,statusCheckRollup,reviewRequests,reviews
```
Check the base against the issue's release branch, commits that are not the author's own (a stale stack), CI, and whether the user's last review is older than the last push. A PR that is not ready becomes "wait for <what is missing>", with the fact that says so, not "review".

Read the user's reviews from `reviews`, not `latestReviews` — the latter keeps one review per author, so a COMMENTED after an APPROVED hides the approve. An approve with no later CHANGES_REQUESTED stands: that PR is not "review", at most "close the answered threads".

Output:

```
next: <verb + what, in words> — <link>
      <kind: "PR in <repo>" / "thread in #<channel>" / "Linear issue">, <issue key if any>
why: <one clause: who waits, or since when>

then:
- <verb + what, in words> — <link>
- …
```

Lead with the thing in words ("review the icon and color contract"), never with a bare number; a PR number and an issue key never sit side by side unlabeled.

A raw PR number or issue key says nothing — "review #331 and #333 on ABC-709", "close the threads in PR #150" is a miss. Every item names its subject in words; the number only rides along in the link. This holds when the answer is shortened too: drop items, not words.

Up to five items under `then`, in the same order. Nothing anywhere → say so in one line. A source that failed (auth, rate limit) is named in one line under the list, not silently skipped.
