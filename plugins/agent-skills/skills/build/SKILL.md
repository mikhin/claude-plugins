---
name: build
description: Implement a spec in one pass at medium effort, then list the assumptions made so they can be reviewed. The spec is usually the output of grill-me.
effort: medium
disable-model-invocation: true
argument-hint: "[spec, or a path to it]"
---

# Build

Spec: $ARGUMENTS. If empty, the spec from the conversation.

1. Read the code the change touches first: its callers and the helpers that already exist.
2. Implement it in one pass. Keep it simple: nothing the spec does not ask for.
3. End with a short list of the assumptions you made where the spec was silent, then the next step: the user reviews them and the diff, then `/verify`.
