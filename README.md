# useful-ai-tooling

A repository of reusable AI/LLM agent assets: skills, commands, instructions,
and AGENTS.md tips. Point new OpenCode (or Claude Code) projects at this repo
to pull in skills that fit that project.

## How to use this repo

### Include skills in an OpenCode project

1. Clone this repo somewhere stable, e.g. `~/code/useful-ai-tooling`.
2. Add the skills directory to the project's `opencode.json`:

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "skills": [
    "~/code/useful-ai-tooling/skills"
  ]
}
```

Only relevant skills get advertised to the model — the `description`
frontmatter controls discovery, so enabling the whole directory is cheap.

Alternatives:

- **Copy** a single skill directory into the project's `.opencode/skills/`.
- **HTTP catalog**: this repo can double as a catalog (see `skills/index.json`
  if present) via a raw GitHub URL in the `skills` array.
- **Claude Code**: symlink or copy `skills/<category>/<id>/` into
  `.claude/skills/<id>/` — the SKILL.md format is shared.

## Structure

```
skills/            Agent skills in SKILL.md format, grouped by category
├── python/          Skills useful for Python projects
│   ├── verify-imports/
│   └── pip-over-uv/
├── code-style/      Writing consistent code in existing repos
│   └── match-existing-style/
├── testing/
│   └── verify/        Drive the changed surface; VERIFIED/SKIP/BLOCKED verdicts
├── development/
│   └── subagent-driven-development/   Plan execution via per-task subagent dispatch
├── memory/
│   └── memory/        Persistent file-based memory across sessions
├── git/
└── documentation/

agents-md/         AGENTS.md content library
├── snippets/         Drop-in rule blocks for project AGENTS.md files
├── templates/        Starter AGENTS.md skeletons

commands/          Tool-specific custom command definitions
└── opencode/        OpenCode command markdown files (verify, remember, design)
```

## Conventions

- **Skill IDs are path-derived**: for `skills/<category>/<id>/SKILL.md` the ID
  is `<id>` (the last directory segment). Keep every tail directory name
  unique across all categories — two categories can't both contain a skill
  called `release`.
- Every skill needs a `description` frontmatter; without it the model never
  discovers it.
- IDs are lowercase kebab-case, matching their directory name.
- Snippets are self-contained blocks meant to be pasted (or referenced) from a
  project's AGENTS.md; keep each snippet about one topic.