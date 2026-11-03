---
name: effort-advisor
description: Recommend an effort level (low, medium, high, max) for a coding task in one or two lines before starting it. Triggers when the user describes a coding task to do — a feature, a bug, a refactor, a review — or asks "what effort", "which effort level".
---

# Effort advisor

At the top of the reply, one or two lines: the level and why. Then carry on.

| Level | For |
| --- | --- |
| `low` | brainstorming, sketches, easy changes, fast iteration in the loop |
| `medium` | regular feature work |
| `high` | bugs in brownfield code, many edge cases, anything where verification matters |
| `max` | fully autonomous end-to-end work, security review, critical software |

Security, hardware, ML and science gain the most from high effort.

Higher effort fixes missed edge cases, not a wrong approach. If the approach is in doubt, say that instead of raising the level.

For a task that needs a spec — a ticket, a feature — name the loop and its first command: `/build`, the user's review, `/verify`. Don't run them yourself: their effort level applies only when the user types the command.

The user sets the level with `/effort <level>`; `/build` and `/verify` set their own.
