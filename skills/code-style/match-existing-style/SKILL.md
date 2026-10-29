---
name: Match Existing Code Style
description: Blend into the surrounding codebase; don't add unrequested comments, formatting churn, or extra tooling
---

Treat unfamiliar or modified files as someone's work — investigate before
changing or deleting them.

## Rules

- Match the structure, naming, style, and patterns of the surrounding code.
- Do not add comments unless asked. Code should explain itself.
- Do not reformat, rename, or reorganize unrelated code while making a change.
- Don't run linters or test suites unless the project's AGENTS.md documents
  them — many repos have none, and a missing linter is not an invitation to
  install one.
- Keep the diff minimal: only what the change requires.