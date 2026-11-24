# claude-plugins

Claude Code plugins and skills, in one marketplace.

```
/plugin marketplace add mikhin/claude-plugins
/plugin install <name>@mikhin
```

Skills for any agent, via [skills.sh](https://skills.sh):

```
npx skills add mikhin/claude-plugins
```

| Plugin | What it does |
| --- | --- |
| [`workstyle`](plugins/workstyle) | How to decide, review, write to people, run releases and work with a team |
| [`agent-skills`](plugins/agent-skills) | Codex code review, multi-agent debate, multi-agent deep research, overbuild check, build/verify effort loop |
| [`shot-preview`](https://github.com/mikhin/claude-shot-preview) | Draws a thumbnail under a reply that names a screenshot path |
| [`iphone-cam`](https://github.com/mikhin/claude-iphone-cam) | Snaps a frame from your iPhone camera and sends it to Claude |
| [`tldr-length`](https://github.com/mikhin/claude-tldr-length) | Sends a long answer back once to be rewritten as a tldr |
| [`lsp-first`](https://github.com/mikhin/claude-lsp-first) | Sends grep for code symbols to the LSP tool |
| [`quota-budget`](https://github.com/mikhin/claude-quota-budget) | Paces a weekly model quota at 1/7 per day |
| [`context-budget`](https://github.com/mikhin/claude-context-budget) | Warns when context grows past a threshold and offers a handoff |

## Skills

### workstyle

| Skill | What it does |
| --- | --- |
| [`decisions`](plugins/workstyle/skills/decisions) | Answer the why, look it up before inventing it, fix the class not the call site |
| [`stack`](plugins/workstyle/skills/stack) | How work gets checked: the frontend checking stack, viewport review, product trade-offs |
| [`comms`](plugins/workstyle/skills/comms) | Writing to people: draft first, no blame, one question instead of a pile of comments |
| [`trade-off`](plugins/workstyle/skills/trade-off) | A hard client date and work that does not fit: the cheapest path to the date |
| [`review`](plugins/workstyle/skills/review) | An independent review, every finding judged against the code, a merge verdict |
| [`all-check`](plugins/workstyle/skills/all-check) | Every check the project has except e2e, scoped to the files of the current PR |
| [`pending`](plugins/workstyle/skills/pending) | Turn a thought about a PR into comments in a pending GitHub review |
| [`contract-impact`](plugins/workstyle/skills/contract-impact) | What an API contract change forces in the frontend, or what a UI needs that the contract lacks |
| [`figma-diff`](plugins/workstyle/skills/figma-diff) | A PR's UI against its Figma frames, state by state and at every viewport |
| [`stage-test`](plugins/workstyle/skills/stage-test) | One subagent saves a test plan, another tests staging and reports release blockers |
| [`next`](plugins/workstyle/skills/next) | The one next step: what blocks others first, then your own work |
| [`report`](plugins/workstyle/skills/report) | Draft your update for the client channel since the last one |
| [`answer`](plugins/workstyle/skills/answer) | What a teammate actually asked, what was already decided, a draft reply |
| [`release-close`](plugins/workstyle/skills/release-close) | Every issue of a release checked against its PRs, tails listed with an action each |
| [`pulse`](plugins/workstyle/skills/pulse) | A teammate's activity day by day against their own pace, and a non-toxic way to ask |
| [`org-research`](plugins/workstyle/skills/org-research) | One topic across every repo, Linear and Slack, as a dated, linked report |
| [`digest`](plugins/workstyle/skills/digest) | The last week across Slack, Linear, GitHub, the calendar and Zoom, grouped by topic |
| [`prune`](plugins/workstyle/skills/prune) | Tidy git: merged branches, stray worktrees, fast-forward to the active release branch |
| [`desk`](plugins/workstyle/skills/desk) | Put the result of a session in your output folder |
| [`buy`](plugins/workstyle/skills/buy) | Pick one thing to buy: criteria, real reviews, top three and one pick |
| [`talk`](plugins/workstyle/skills/talk) | Toggle voice mode: replies spoken by macOS `say` |
| [`slack-copy`](plugins/workstyle/skills/slack-copy) | Put a Slack message on the clipboard with working links and code formatting |
| [`pin`](plugins/workstyle/skills/pin) | Pin a session so a shell function resumes it from any directory |

### agent-skills

| Skill | What it does |
| --- | --- |
| [`codex-code-review`](plugins/agent-skills/skills/codex-code-review) | Code review by OpenAI Codex, judged by Claude |
| [`multi-agent-debate`](plugins/agent-skills/skills/multi-agent-debate) | Three agents get contradicting hypotheses and must refute each other |
| [`multi-agent-deep-research`](plugins/agent-skills/skills/multi-agent-deep-research) | Four researchers on one topic, one report |
| [`effort-advisor`](plugins/agent-skills/skills/effort-advisor) | Recommends an effort level for the task |
| [`build`](plugins/agent-skills/skills/build) | Implements a spec in one pass at medium effort, lists its assumptions |
| [`verify`](plugins/agent-skills/skills/verify) | Hunts edge cases at high effort: reproduce, adversarial review, randomized tests |
| [`overbuild`](plugins/agent-skills/skills/overbuild) | Finds code in a diff that handles cases no user reaches |

## License

MIT
