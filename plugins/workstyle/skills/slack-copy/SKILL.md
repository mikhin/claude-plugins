---
name: slack-copy
description: Put a drafted Slack message on the macOS clipboard so it pastes with working links and code formatting. Never sends. Triggers — "/slack-copy", "copy for slack", "pbcopy this for slack", "with links for slack", "put it in the clipboard for slack".
---

Slack takes the plain text from the clipboard and ignores HTML and RTF, so write plain text with markup and copy it with `pbcopy`:

- Link: `[ABC-1](https://…)`, the word carries the link. No bare URLs, no URL in brackets after the word.
- Branch, file, field, command: in backticks.
- Lists: `-` at the start of the line.
- Bold: `**text**`.

```bash
printf '%s' '<message>' | pbcopy
pbpaste
```

Show `pbpaste` output to confirm the text, then tell the user to paste. Sending stays with the user.

If the links arrive as literal `[text](url)`, the "Format messages with markup" setting in Slack is off. Say so, and copy the message with bare URLs instead.
