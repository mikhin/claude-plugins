# workstyle

Skills for a senior frontend engineer who leads a project: how to decide, review, write to people, run releases and work with a team. They were extracted from real Claude Code sessions — the corrections people actually make, not aspirations.

## How to work

| Skill | What it does |
| --- | --- |
| [`decisions`](skills/decisions) | Answer the why, look it up before inventing it, fix the class not the call site. Always on. |
| [`stack`](skills/stack) | How work gets checked: the frontend checking stack, viewport review, product trade-offs. Always on. |
| [`comms`](skills/comms) | Writing to people: draft first, no blame, no hinted defects, one question instead of a pile of comments. |
| [`trade-off`](skills/trade-off) | A hard client date and work that does not fit: what the date really needs, the cheapest path to it, the debt written down. |

## Code and review

| Skill | What it does |
| --- | --- |
| [`review`](skills/review) | `/review`: an independent review at medium, every finding judged against the code, a merge verdict. Plus how bugs get handled: why didn't the tests catch it, failing test before fix, two rounds and stop. |
| [`all-check`](skills/all-check) | Every check the project has except e2e, scoped to the files of the current PR, one line per check. |
| [`pending`](skills/pending) | Turn a thought about a PR into comments in a pending GitHub review. Never submits on its own. |
| [`contract-impact`](skills/contract-impact) | What an API contract change forces in the frontend, or what a UI needs that the contract lacks. |
| [`figma-diff`](skills/figma-diff) | A PR's UI against its Figma frames, state by state and at every viewport, with screenshots. |
| [`stage-test`](skills/stage-test) | One subagent verifies deployments and saves a test plan; another tests staging and reports release blockers. |

## Project and team

| Skill | What it does |
| --- | --- |
| [`next`](skills/next) | The one next step: what blocks others first, then your own work. |
| [`report`](skills/report) | Draft your update for the client channel since the last one. |
| [`answer`](skills/answer) | A teammate asked something: what is actually asked, what was already decided, a draft reply. |
| [`release-close`](skills/release-close) | Every issue of a release checked against its PRs, tails listed with an action each. |
| [`pulse`](skills/pulse) | A teammate's activity day by day against their own pace, and a non-toxic way to ask. |
| [`org-research`](skills/org-research) | One topic across every repo, Linear and Slack, as a dated, linked report. |
| [`prune`](skills/prune) | Tidy git: merged branches, stray worktrees, fast-forward to the active release branch. |

## Small helpers

| Skill | What it does |
| --- | --- |
| [`desk`](skills/desk) | Put the result of a session in your output folder, in the subfolder of its topic. |
| [`buy`](skills/buy) | Pick one thing to buy: criteria, real reviews, original or copy, top three and one pick. |
| [`talk`](skills/talk) | Toggle voice mode: replies spoken by macOS `say`. |
| [`pin`](skills/pin) | Pin a session so a shell function resumes it from any directory. |

## Install

```
/plugin marketplace add mikhin/claude-plugins
/plugin install workstyle@mikhin
```

Skills then show up namespaced: `workstyle:review`, `workstyle:next`.

## Context file

The project skills hold no project names. They read your values from `~/.claude/workstyle-context.md`, a private file that stays on your machine. A skill that misses a value asks once and adds it there. Start from this:

```
# Work context

## Me
- GitHub login:
- Slack id:
- Time zone for reports:
- Output folder: ~/Desktop
- Marketplaces to buy from, in order:

## Project: <name>
- GitHub org:
- Repos and local clones (short names you use for them):
  - frontend —
  - contract —
  - backend — (read only if another team owns it)
- Slack channels (name = id), the report channel:
- Linear team key, release project naming, release branch: release/<slug>
- Backlog projects:
- Staging: the frontend command, the backend app, where its logs are
- Figma file:
- Report leaves out:
```

Secrets stay out of it. `org-research` reads `LINEAR_API_KEY`, or runs `LINEAR_KEY_COMMAND`. The command can be a secret manager read, for example `op read 'op://<vault>/<item>/credential'`.

## Always-on skills

`decisions` and `stack` describe constants, so they are useless behind a trigger. The
plugin ships a `SessionStart` hook that prints them into every session instead.
Pick a different set with `AUTOLOAD_SKILLS="a b"`.

## Requirements

- `talk`: two hooks in `~/.claude/settings.json`. `UserPromptSubmit` prints a "reply in one or two spoken sentences" instruction while `~/.claude/voice-on` exists. `Stop` pipes `.last_assistant_message` from the hook input through `say` while that file exists.
- `pin`: a shell function that reads `~/.claude/pins.tsv` (session id, directory, name), picks a line with `fzf`, then runs `cd <directory> && claude --resume <id>`.
- `next`, `report`, `answer`, `release-close`, `pulse`, `org-research`: `gh`, the Slack and Linear MCP servers.

## License

MIT
