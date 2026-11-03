---
name: figma-diff
description: Compare a PR's UI with its Figma frames, state by state and at every viewport, and put paired screenshots plus a findings list on the Desktop. Triggers — "/figma-diff", "compare with the design", "does it match Figma", "compare the PR with Figma", "how will it look in the release".
---

## 1. The frames

The link from the issue or spec. No link → search the file by the text on screen: the official Figma MCP `use_figma` with `findAll` over the `characters` of TEXT nodes, one call per page. Frame names mislead. Hit the call limit → a REST-based Figma MCP (`get_figma_data`). Known frames may be in memory.

## 2. The states

List every state the frames show: empty, filled, loading, error, a dialog, each role that sees something different. Add the states the frames do not show (long text, many rows, narrow width). They are questions for the designer, not bugs.

## 3. The PR render

The PR branch on mocks, or on staging. Screenshot each state at 320, 375, 430, 768 and 1440 in Chrome, through the project's viewport stand when it has one.

## 4. Compare

- copy, word for word;
- order and grouping of elements;
- spacing, sizes, colors against the tokens;
- states present: empty, error, disabled, loading skeleton matching the layout;
- icons, alignment;
- at 320: composition, sticky first column, horizontal scroll.

Each difference is one of: a bug (the code is wrong), intended (a decision exists, with a link), a question for the designer.

## 5. Answer

`workstyle:desk` → `~/Desktop/<ticket>-figma-diff/`: pairs `N-<state>-figma.png` / `N-<state>-<width>.png` and `diff.txt`, one line per finding: state, what differs, the Figma node link, `file:line`, kind.

In chat: the counts per kind, then the bugs. Questions for the designer → offer a draft for the designer, or `workstyle:pending` on the PR.
