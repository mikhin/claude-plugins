---
name: verify
description: Verify an implementation at high effort — reproduce first, adversarial self-review, randomized tests against a reference, tests that fail on a reverted fix, large inputs — and report what was checked, what failed, what was fixed.
effort: high
disable-model-invocation: true
argument-hint: "[what to verify]"
---

# Verify

Target: $ARGUMENTS. If empty, the change just built.

1. Reproduce the bug or behaviour before changing code.
2. Review your own implementation adversarially.
3. Read the relevant library or parser source if its behaviour is unclear.
4. Write randomized tests against a simple reference implementation.
5. Confirm the tests fail on a half-finished or reverted fix.
6. Test performance on large inputs, not just small ones.
7. Where several approaches are valid (data prep, interpretation), try two and investigate if the results differ.
8. Use standard test suites where they exist (e.g. XSS suites).

A step that does not apply is skipped and named in the report.

## Report

- **Checked**: each step and what it covered.
- **Failed**: what broke, with the input that broke it.
- **Fixed**: what changed.
