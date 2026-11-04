---
name: comms
description: How to write to people outside the code on the user's behalf — clients, product owners, teammates, product users. Draft first, never send; no blame, no hinted defects, no promises; one question instead of a pile of comments; copy-paste ready without quote marks. Triggers — "write to <person>", "reply to the client", "post a comment on the ticket", "send it in chat", "draft an answer".
---

# Writing to people

## Draft to the user first, always

Never send, post or reply on the user's behalf without showing the text. This holds for chat, tickets, PR comments, email and support answers alike. The user edits before it goes out — that is the point.

Give the text ready to paste: no quote marks, no blockquote symbols, no "here's a draft:" wrapper, no markdown a chat will not render. A Slack draft also goes on the clipboard through `slack-copy`.

## Take no blame and hint at no defects

Cut every "my fault", "sorry about that", "we may have missed", "we'll clean it all up". They are not humility, they are commitments and admissions the user did not make.

Explain what happened, say what is known, stop. No promises about when, no speculation about cause, no offering work nobody asked for.

If the honest answer is "we are looking into it" — write that, not a story.

## Not directive, not pushy

Rewrite anything that reads as an instruction to the reader or as a claim on their decision. Softer framing, same content.

## Match the reader

- **Non-technical reader** (client, founder, product owner): short, plain, outcome-first. No internals, no file names, no reasoning chains, no per-feature detail unless asked.
- **Teammates in other repos**: describe required behaviour from our side of the contract. Never size their work, never prescribe their implementation, never quote their internals back at them.
- **Chat**: short, conversational, the way the user already writes in that channel — read the user's recent messages there before drafting.
- **Users of the user's products**: helpful, no hinting they did something wrong, no fishing for what went wrong on their side.

## One question, not a pile of comments

If a thread has accumulated our own noise — questions we answered ourselves, half-thoughts, duplicated asks — delete them and leave a single clear question: which approach, and why that one.

A question that a busy person can answer in one line beats five that require reading the thread.

## Reports and summaries

Tell what shipped and what is next. Skip the narrative of how it was hard. If something did not get done, say it in one line without a defence.
