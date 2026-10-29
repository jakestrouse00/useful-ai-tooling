---
name: Memory
description: Persistent, file-based memory for future sessions — save user/feedback/project/reference facts that are not derivable from the code. Adapted from Claude Code 2.1.303's built-in auto-memory system.
---

Memory lives at `.opencode/memory/` in this project. It exists so future conversations can
start knowing who you are, how you like to collaborate, and what is in flight — without
re-learning it from `git log` every session.

## When to save

- Immediately when the user asks you to remember something (or to forget something — find and
  remove the entry then).
- Any time you learn the user's role, preferences, or knowledge; whenever the user corrects
  your approach or confirms a non-obvious approach worked (save successes too, not only
  corrections); whenever you learn ongoing-work context that is not derivable from the code.

## Types

- `user` — role, goals, expertise. Write nothing negative or irrelevant.
- `feedback` — guidance about how to work here. Lead with the rule, then a **Why:** line and a
  **How to apply:** line. Record corrections AND validated judgment calls.
- `project` — ongoing work, decisions, deadlines not derivable from code or git history.
  Convert relative dates to absolute ones ("Thursday" → an actual date).
- `reference` — pointers to where information lives in external systems.

## Format

One file per memory with frontmatter, then index it:

````markdown
---
name: short-kebab-slug
description: one-line summary, specific enough to judge relevance later
metadata:
  type: user|feedback|project|reference
---

content (feedback/project: rule, **Why:**, **How to apply:**; link related memories as [[slug]])
````

Add one line per memory to `.opencode/memory/MEMORY.md` (under ~150 chars): `- [Title](file.md) — hook`.
MEMORY.md is an index, never a memory body.

## What NOT to save

- Code patterns, conventions, architecture, paths — derivable by reading the project.
- Git history or who-changed-what — `git log`/`git blame` are authoritative. (In this
  workspace, repo findings already live in `research/` reports — link them, don't copy them.)
- Fix recipes — the fix is in the code; the commit carries the context.
- Anything already in the project's `AGENTS.md`.
- Ephemeral task state (this conversation's plan, todos).

These exclusions hold even when the user explicitly asks to save; if forced anyway, save only
the surprising, non-obvious part.

## Before acting on a memory

A memory naming a file, function, or flag is a claim about the past, not the present: check
it (file exists? identifier still greps?) before relying on it — especially for "recent state"
questions, where the repo now wins over the memory. If stale, trust the observation, update or
remove the memory, and move on.