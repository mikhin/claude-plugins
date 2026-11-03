# agent-skills

Claude Code skills for getting a second opinion before you trust the first one — AI code review by another model, multi-agent debate, multi-agent deep research — and an effort loop that builds at medium and verifies at high.

| Skill | What it does |
| --- | --- |
| [`codex-code-review`](skills/codex-code-review) | Code review by OpenAI Codex, judged by Claude. Your diff, commit or plan reviewed by a model that didn't write it. |
| [`multi-agent-debate`](skills/multi-agent-debate) | Three agents get contradicting hypotheses and must refute each other. Exit on agreement, you get the survivor. |
| [`multi-agent-deep-research`](skills/multi-agent-deep-research) | Four researchers on one topic, one report, no blind spots. |
| [`effort-advisor`](skills/effort-advisor) | Recommends an effort level for the task in one or two lines; for a ticket, names the loop's next command. |
| [`build`](skills/build) | `/build`: implements a spec in one pass at medium effort, lists its assumptions. |
| [`verify`](skills/verify) | `/verify`: at high effort — reproduce, adversarial review, randomized tests, reverted-fix check, large inputs. |
| [`overbuild`](skills/overbuild) | Finds code in a diff that handles cases no user reaches: guards, queues, refs, counters, outputs nothing reads. |

## Effort loop

`grill-me` → `/build` → review → `/verify`

`grill-me` turns an idea into a spec, `/build` reads the code it touches, implements the spec in one pass and lists what it assumed, you review the assumptions, `/verify` hunts the edge cases. `/build` and `/verify` set their effort level themselves, for their own reply only and only when typed — a skill the model invokes on its own runs at the session level, so the loop doesn't chain itself; each step names the next command instead. Higher effort finds missed edge cases; a wrong approach is caught at review.

## Install

Any agent, via [skills.sh](https://skills.sh):

```
npx skills add mikhin/claude-plugins
```

Claude Code, as a plugin:

```
/plugin marketplace add mikhin/claude-plugins
/plugin install agent-skills@mikhin
```

Skills then show up namespaced: `agent-skills:codex-code-review`, `agent-skills:multi-agent-debate`, `agent-skills:multi-agent-deep-research`, `agent-skills:build`, `agent-skills:verify`, `agent-skills:overbuild`.

## Requirements

- Claude Code with sub-agents for `multi-agent-debate` and `multi-agent-deep-research`
- [Codex CLI](https://github.com/openai/codex) signed in for `codex-code-review`, optional for the fourth voice in `multi-agent-debate`
- A `grill-me` skill for the effort loop, optional. The effort levels in `/build` and `/verify` apply in Claude Code only

## License

MIT
