---
name: answer
description: A teammate or the client asked the user something — find the question, say what is actually asked, check earlier decisions and the code, contract and tickets, and draft the reply. Never sends. Triggers — "/answer", "<name> asked me", "reply to <name>", "what do I answer", "I don't get what they ask", "go through the comments".
---

The org, repos, channels and ids come from `~/.claude/workstyle-context.md`. A value missing there → ask once and add it.

## 1. The question, verbatim

Find it: the Slack thread (`conversations_search_messages`, then `conversations_replies`), a Linear comment, or a PR comment. Read the whole thread and what came before it. Who asks, and in what role.

## 2. What is actually asked

One line: "Question: …". Also what is not asked. Two readings → name both and say which one the thread supports. When the user did not understand the question, this line is the whole answer, with a concrete example.

## 3. Was it already decided?

Slack, Linear comments and documents, PR threads, git log, memory. A found decision → quote + link + date, and the answer follows from it. Do not invent a second answer next to an existing one.

## 4. Facts

Code on the branch where the work lives (`release/*`, not `main`), the API contract, Linear issues. Repos owned by others are read-only. Describe backend behaviour from the contract and its responses, never from its source.

## 5. Draft

In the user's voice (`workstyle:comms`): the language of the thread, short, no quotes around it, links instead of retelling. A decision that belongs to another team → what the contract requires, then stop.

## Answer

```
Question: <one line>
Draft:
<text ready to paste>
Sources: <links>
```

Post only after the user's ack, in the same thread.
