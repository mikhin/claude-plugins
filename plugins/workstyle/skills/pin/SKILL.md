---
name: pin
description: Pin this session so `pins` in the terminal resumes it from any directory
disable-model-invocation: true
allowed-tools: Bash
---

!`f=~/.claude/pins.tsv; touch $f; grep -v "^${CLAUDE_SESSION_ID}" $f > $f.tmp; printf '%s\t%s\t%s\n' "${CLAUDE_SESSION_ID}" "$PWD" "$ARGUMENTS" >> $f.tmp; mv $f.tmp $f; tail -1 $f`

Reply only "📌" and the pinned line above.
