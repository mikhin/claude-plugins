---
name: talk
description: Toggle voice mode — replies are spoken aloud by macOS `say` and kept to one or two short conversational sentences. Needs the voice hooks from the README. Triggers — "/talk", "/talk on", "/talk off", "voice mode", "turn the voice on", "turn the voice off".
---

Argument `on` → run `touch ~/.claude/voice-on`. Argument `off` → run `rm -f ~/.claude/voice-on; killall say 2>/dev/null`. No argument → flip: on if the file is missing, off if it exists.

Then answer in one short sentence: voice mode is on or off.
