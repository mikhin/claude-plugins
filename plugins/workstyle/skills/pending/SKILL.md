---
name: pending
description: Turn the user's thought about a PR into comments in their pending (unsubmitted) GitHub review — English, friendly, anchored to lines — and never submit unless the user says publish. Triggers — "/pending", "add to pending", "leave a pending comment", "put it in the pending review", "publish the pending review".
---

## 1. Find the PR and the pending review

PR from the argument (number in the current repo or a URL), else `gh pr view`.

`gh api repos/.../pulls/N/comments` does not show pending comments. Read them through GraphQL:

```
gh api graphql -f query='{repository(owner:"<o>",name:"<r>"){pullRequest(number:N){id headRefOid reviews(states:PENDING,first:5){nodes{id author{login} body comments(first:100){nodes{id path line body}}}}}}}'
```

GitHub allows one pending review per author. The user's exists → add to it. None → create it with REST `POST /repos/<o>/<r>/pulls/N/reviews` with `commit_id` and `comments`, and **no `event` field**. An `event` submits the review, and a submitted review cannot go back to draft.

## 2. Write the comment

- English, friendly, as a question or a suggestion, never a verdict: "Could we …?", "What do you think about …?". Never toxic, even when the user's note was.
- One point per comment. What breaks, in one sentence. The fix, if there is one, in one line or a ```suggestion block.
- Anchor to `path` + `line` on the RIGHT side. The line must be inside a diff hunk. Otherwise put the point in the review body.
- About UI → a link to the Figma node.
- An earlier pending comment on the same point → rewrite it with `updatePullRequestReviewComment`, do not add a second one.

Add to an existing review:
```
gh api graphql -f query='mutation{addPullRequestReviewThread(input:{pullRequestReviewId:"<id>",path:"<path>",line:<n>,side:RIGHT,body:"<text>"}){thread{id}}}'
```

## 3. Answer

Every comment as `path:line — text`, and the PR link. Nothing is sent to the author.

## Publish

Only on an explicit "publish" / "submit": `submitPullRequestReview` with `event: COMMENT`. APPROVE or REQUEST_CHANGES only when the user names it. Before that, translate anything not in English.
