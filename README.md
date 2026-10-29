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

## Bootstrap prompt for new projects

Paste this prompt to the agent in a new project to have it set
useful-ai-tooling up for you. It browses the repo, recommends a subset that
fits the project, and wires it in after your approval.

```text
Set up useful-ai-tooling for this project.

There is a repository at https://github.com/jakestrouse00/useful-ai-tooling
which stores reusable agent assets: skills (SKILL.md format grouped by
category), custom agent commands, and AGENTS.md snippets/templates.

1. If the repo is not already checked out, clone it to
   ~/code/useful-ai-tooling (use git pull first if it already exists).
2. Read its README.md, then review the available assets:
   - skills/: every SKILL.md's name and description frontmatter
   - commands/opencode/: each command's purpose from its frontmatter
   - agents-md/: the AGENTS.template.md starter and the snippets
3. Recommend which assets fit THIS project, based on its stack, structure,
   and how agents will be used here. Present a short table: asset, why it
   fits, and any you considered but rejected with a one-line reason.
   Wait for my confirmation before installing anything.
4. After I confirm, wire them in for this tool:
   - OpenCode: add {"$schema": "https://opencode.ai/config.json",
     "skills": ["~/code/useful-ai-tooling/skills"]} to opencode.json
     (relative paths resolve from the working directory; the clone path
     above is absolute, so use it as-is). Copy any approved command files
     into .opencode/command/. Create or update AGENTS.md from
     agents-md/templates/AGENTS.template.md, merging in the approved
     snippets content.
   - Claude Code: copy the approved skill directories into .claude/skills/
     (the SKILL.md format is shared) and follow the AGENTS.md steps above.
5. Finish by reporting exactly what you installed, anything that needs a
   follow-up (e.g. a skill that only makes sense once part of the project
   exists), and how to undo it.
```

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