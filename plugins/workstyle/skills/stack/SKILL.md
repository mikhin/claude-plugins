---
name: stack
description: How work gets verified — the frontend checking stack expected on every change, how layout is reviewed across viewports, and what matters on a product the user runs. Meant to be always on (SessionStart hook), not trigger-loaded.
---

# How work gets checked

The user is a senior frontend engineer. Do not explain the tools below, and do not ask the user to choose things the project has already standardised on. The tools are examples: use what the project has.

## The checking stack

A change is not trusted because it compiles. On a normal change these are expected:

- **typecheck + lint** — with custom rules for the codebase; when a rule can encode an architectural constraint, write the rule, not a review comment. Layer boundaries are lint-enforced.
- **unit tests** — over pure business logic, with the API mocked at the network layer (MSW).
- **mutation testing** — Stryker over business logic. A survivor is a missing assertion, usually shape asserted instead of content. The score is a detector, not a target.
- **e2e** — Playwright. Run in CI mode locally when checking flakiness; the default parallel run flakes on its own.
- **dead code / duplication** — dead-export sweeps (knip), reachability from the entry point, copy-paste detection (jscpd).
- **generated API clients** — generated from the spec and never hand-edited; a contract change goes spec first, regenerate after.
- **production signal** — error tracking, product analytics, deploy logs when staging misbehaves.

When something escapes all of this, the interesting question is which of them should have caught it.

## Layout review

Layout is checked across viewports, on real renders — not on a description of them. Narrow first: 320, 375, 430, then tablet, then desktop. What to look for:

- composition and rhythm at 320, not just "it fits"
- sticky first column and horizontal scroll behaviour on wide grids
- tables: same solution everywhere, no second implementation for the second table
- skeletons matching the real layout
- empty states and buttons at the narrowest width

"It works on desktop" is not a result. A screenshot of the actual page is.

## Contract and process

- API-first: the spec is the source of truth, the backend implements it, the client is generated. Not "the spec documents what was built".
- Tickets, releases and branch flow are already set up per project — read the repo's own notes rather than proposing a new process.
- Design lives in Figma and is the reference for UI work; if the implementation diverges, that is a finding.

## Products the user runs

On a product the user owns, "what is technically possible" is rarely the question. The question is what a change does to conversion, cost, or support load.
