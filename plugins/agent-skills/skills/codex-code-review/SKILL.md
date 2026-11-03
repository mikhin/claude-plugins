---
name: codex-code-review
description: 'Code review by OpenAI Codex, judged by Claude. A second opinion on a branch diff, the working tree, a commit or a plan from a model that did not write it: Codex reads and reports, Claude verifies every finding against the code, then edits. One or two rounds, Codex never writes. Cross-model AI code review, lighter than a debate. Triggers — "codex review", "codex code review", "second opinion", "let codex look at this", "/codex-code-review".'
---

# Codex code review

Codex reads, Claude judges. Codex writes nothing.

## Target — from the arguments, otherwise the branch diff

| Argument | `$DIFF` in the prompt |
|---|---|
| nothing / `base=<branch>` / PR URL | `git diff <branch>...HEAD`; default base is `gh pr view --json baseRefName -q .baseRefName`, without a PR — `main`; `git fetch` first and use `origin/<branch>` |
| `wt` | `git diff HEAD; git ls-files --others --exclude-standard` |
| `commit=<sha>` | `git show <sha>` |
| `spec=<file>` | on top of any diff: add to the prompt `Also check the diff against the plan in $SPEC — what the plan asked for and the diff lacks, and what the diff does beyond it.` |
| `plan=<file>` | no diff — the "Plan" recipe below |

Everything goes through `codex exec`: `codex review` (0.149.1) does not accept a custom prompt together with `--base`, it exits with code 2 and empty output.

## Running

Before the first round show the model — `grep '^model' ~/.codex/config.toml` — so the user can stop before quota is spent.

Always: `< /dev/null` (otherwise `codex` waits for stdin and hangs), `-s read-only -c sandbox_mode="read-only"`, tool timeout `600000`. Do not pin the model — it comes from the config. Output goes to `$SCRATCH/codex-r<N>.md`, stderr to `$SCRATCH/codex-r<N>.err`, where `N` is the round number.

Diff:

```bash
codex exec -s read-only -c sandbox_mode="read-only" -o "$OUT" \
  "Adversarial review of the diff from '$DIFF': run it and review it. Concrete bugs, missed edge cases, contract violations, duplicated or dead code. For an edge case, name the user action or the contract response that reaches it; skip ones that need sub-second timing or a response the contract does not describe. Also report code that guards against such cases. One line per finding: file:line — problem — fix; prefer a fix that removes code or state over one that adds a guard. No praise, no style nits, nothing a linter or type checker would catch. Read the repo as needed, modify nothing." \
  < /dev/null 2>"$ERR" >/dev/null
```

Plan:

```bash
codex exec -s read-only -c sandbox_mode="read-only" -o "$OUT" \
  "Adversarial review of the plan in $PLAN. Wrong assumptions, missing steps, what breaks, a simpler alternative. One line per finding with a fix. Read the repo as needed, modify nothing." \
  < /dev/null 2>"$ERR" >/dev/null
```

An empty `$OUT` means the run failed (auth, model, arguments). The reason is in the tail of `$ERR`. Show it to the user, do not retry blindly.

## Arbitration

Each finding in one line: `accepted` / `rejected: <anchor>` / `comment: <why it stays>`. An anchor is code, a test, a contract, the project rules; "seems fine" is not an anchor. `comment` is for a real case no normal path reaches: it changes nothing. A finding that adds a guard is accepted only with the normal path that reaches it. Show the list, wait for "ok". Edits come only after that.

## Round 2 — only if something was changed

Same way, but embed `codex-r1.md` and what was done into the prompt:

```
Prior findings: <r1 verbatim>. Applied: <list>. Rejected: <list with reasons>.
Say which findings are fixed, which are not, and anything new the fixes introduced. Do not re-review from scratch.
```

This way Codex checks its own list instead of starting over. There is no round 3: "is everything ok" is a bottomless question. Remaining disagreement — name it and hand it to the user.

## Do not

- pin `-m`
- let Codex write
- run it on trivial diffs
