# AGENTS.md content library

Content for project `AGENTS.md` files, split by reuse:

- `templates/` — starter skeletons to copy into a new project and fill in.
- `snippets/` — one file per rule topic, self-contained markdown blocks meant
  to be pasted into (or referenced from) a project's AGENTS.md.

## Adding a snippet

- One topic per file, kebab-case filename (e.g. `error-handling.md`).
- Open with a short `# Topic` header, then the rules as a flat list.
- Keep it portable: no project-specific paths or names.
- If a snippet and a skill in `skills/` cover the same rule, keep both in
  sync — snippets are for AGENTS.md, skills are for on-demand discovery.