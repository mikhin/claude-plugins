---
name: buy
description: Pick one thing to buy — criteria, the user's marketplaces in order, real reviews, original or copy — and give the top three with one pick. Never orders or pays. Triggers — "/buy", "what should I buy", "find me a replacement", "original or copy", "search the marketplace".
---

The marketplaces to search, in order, come from `~/.claude/workstyle-context.md`. None named → ask once and add them.

## 1. Criteria

From the message and the chat before it: what it is for, the budget, must-have parameters (size, power, series, connector), original or a copy is fine, how soon. A missing criterion that changes the pick → one question. Otherwise assume and state the assumption in one line.

When the purpose decides the answer ("I only need the tip cleaned"), check first whether something cheaper or already owned covers it, and say so.

## 2. Search

- The first marketplace of the list, through Claude in Chrome: fast delivery and easy returns.
- The next one when the original is sold only there or costs much less, or the first has none.
- A link from the user → start from it.

## 3. Check every candidate

- Seller: the brand store or a verified original vs a reseller. Copy markers: a price far below the rest, a renamed brand, stock photos only.
- Rating and the number of reviews. Read the 1–3★ reviews: how it fails and after how long.
- Exact fit to the criteria: the part number, not the title.
- Price compared with the same item from other sellers.

## 4. Answer

```
pick: <name> — <price> — <link>
why: <one clause>

| # | item | price | ★ / reviews | upside | risk |
```

At most three rows. Long research → `workstyle:desk`.

Never add to the cart, order or pay unless the user asks. Even then, stop at the payment step.
