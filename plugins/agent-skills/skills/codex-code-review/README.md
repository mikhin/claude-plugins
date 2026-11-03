# codex-code-review — your diff, reviewed by a model that didn't write it

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill for AI code review with a second opinion from OpenAI Codex. Claude hands the diff to [OpenAI Codex CLI](https://github.com/openai/codex), Codex reads and reports, Claude judges every finding against the code and only then edits.

One or two rounds. Codex never writes.

## What it reviews

| Argument | Target |
|---|---|
| none / `base=<branch>` / PR URL | branch diff against its base |
| `wt` | working tree, including untracked files |
| `commit=<sha>` | one commit |
| `spec=<file>` | any of the above, checked against a plan |
| `plan=<file>` | the plan itself, no diff |

## Flow

1. Shows the Codex model from `~/.codex/config.toml` before spending quota.
2. Runs `codex exec` read-only, output to a scratch file.
3. Claude answers each finding in one line: accepted, or rejected with an anchor (code, test, contract). "Seems fine" is not an anchor.
4. Waits for your go, then applies the accepted ones.
5. Round 2 only if something changed: Codex gets its own list back and says what is fixed, what is not, and what the fixes broke. There is no round 3.

## Installation

Install via [skills.sh](https://skills.sh/mikhin/claude-plugins), the command is on the package page. Or copy `SKILL.md` to `~/.claude/skills/codex-code-review/SKILL.md`.

## Requirements

- Claude Code CLI
- [Codex CLI](https://github.com/openai/codex) signed in, with a model set in `~/.codex/config.toml`

## Usage

```
/codex-code-review
/codex-code-review wt
/codex-code-review commit=abc123
/codex-code-review plan=plan.txt
```

Or ask: "codex review", "second opinion on this diff", "let codex look at this".

## License

MIT
