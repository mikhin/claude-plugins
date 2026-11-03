---
name: desk
description: Put the result of the session — txt, png, pdf, screenshots or a draft — in the user's output folder, in the subfolder of its topic, and answer with the path. Triggers — "/desk", "save it to the desktop", "put a txt on the desktop", "screenshots to the desktop", "save the result".
---

The output folder comes from `~/.claude/workstyle-context.md`, `~/Desktop` when it names none.

## Folder

1. List the output folder and reuse the subfolder of this topic when one exists (a release folder, a ticket folder `ABC-321`, …). A ticket key in the topic → the subfolder named by that key.
2. Otherwise create `<output folder>/<topic in words>/`. A single file with no topic goes to the output folder itself.

Never the scratchpad, never `/tmp`: the user opens these files by hand.

## Files

- Name: `<what in words>.txt`, a series numbered in reading order: `1-empty-state.png`, `2-filled.png`.
- Text → `.txt`, plain: no markdown headers or bold, a blank line between blocks, every claim with its link.
- Screens → `.png` of the real render, not a mock-up.
- For print → `.pdf`.
- A file with that name exists → read it. Same document → update it in place. Different one → pick another name. Never overwrite blindly.
- A draft for a person → the text exactly as it is pasted, no quotes, no preamble.

## Answer

The absolute path(s), then a tldr of at most three lines. Short content the user will copy by hand (a command, a value) goes into the chat instead of a file.
