---
name: trade-off
description: A hard client date and work that does not fit — find what the client needs on that date, the cheapest path to it (hardcode, manual step, cut scope) with its debt written down, and draft the proposal to the team. Client business before code purity. Triggers — "/trade-off", "we won't make it", "what can we cut", "the deadline is burning", "propose an alternative", "can we hardcode it", "the client matters more than the code".
---

The principle: the client's business on that date matters more than clean code. Debt is fine when it is written down and its cleanup is planned.

## 1. The date

- What exactly happens on the date (a new customer onboards, a demo, a contract), who said it, quote + link from Slack, Linear or a call transcript.
- Fixed or wished? "The date cannot move, they said so explicitly" is fixed. "It would be nice" is not.

## 2. The gap

- Remaining work by facts: open issues of the release, their PRs, what is merged where.
- Speed from the previous releases (dates of Linear projects and merged PRs), adjusted for who does the work now.
- The gap in days, with an optimistic, realistic and pessimistic date.

## 3. What the date needs

Walk the client's event step by step: which screens, flows and data does it touch? Everything outside that is not needed on the date.

## 4. Options

For each option:
- what the client gets on the date;
- what we fake: a hardcode, a config value, a manual step done by us, a feature flag, a hidden UI;
- the debt and its cleanup cost after the date;
- the risk.

Typical moves: cut scope to the event's path, do a rare action manually (we run it, no UI), hardcode a value that one customer needs, ship read-only first.

## 5. Recommendation

One option, why it wins, and the loser compared. Then the cleanup issue for the next release, created together with the shortcut.

A decision that belongs to another team (backend, design) → say what the date requires from them and stop. Do not size their work or prescribe how they do it.

## 6. Draft

The proposal to the team or client in the user's voice (`workstyle:comms`): confident, no blame, not scary, one question at the end. Long analysis → `workstyle:desk`.
