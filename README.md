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

## License

MIT
