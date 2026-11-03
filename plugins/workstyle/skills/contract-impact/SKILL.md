---
name: contract-impact
description: Show how an API contract change and the frontend affect each other — what a contract PR or spec change forces in the frontend, or what a UI or design needs that the contract lacks. Triggers — "/contract-impact", "does the frontend need changes", "is the contract ready for this UI", "does the spec match this", "do we need to change the contract".
---

The repos, their clones and how the client is generated come from `~/.claude/workstyle-context.md`. A value missing there → ask once and add it.

Two directions. Pick by the input.

## A. Contract → frontend (a contract PR or spec diff)

1. The diff of the spec: changed paths, schemas, fields, enums, required lists. Mind `allOf` composition: one model can feed create, update and response.
2. The generated names they become in the frontend's generated client. For an exact answer, regenerate in a throwaway worktree from the contract branch and diff the generated folder.
3. Consumers of each changed name: LSP find references, Grep as a fallback.
4. Each change is one of:
   - breaking: a field removed or renamed, a new required request field, a type change, a new enum value in an exhaustive switch;
   - additive: a new optional field. No frontend change, unless the UI must show it;
   - text only: descriptions and examples. No frontend change.

## B. Frontend or design → contract (a UI, Figma frames, a spec)

Every value the screen shows or sends → the schema field that carries it. Missing → what the contract must add, as a data shape only. Do not prescribe how the backend makes it.

## Rules

- The contract describes the shape of data, not the implementation behind it.
- Do not shape the contract around one consumer's convenience, ours included.
- The frontend ticket for the change: exists or not, and in which release.

## Answer

```
| change | where in the frontend | what to change | ticket |
```

The verdict first, in one line: "frontend: nothing" / "frontend: N changes" / "contract: N fields missing".
