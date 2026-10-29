---
name: Prefer pip
description: Install Python packages with pip, not uv, when setting up tools or dependencies
---

Prefer `pip` over `uv` for Python package and tool installation.

- Global CLI tools: `pip install --user <tool>`
- Project dependencies: install into the project's virtualenv (activate it or
  target `venv/bin/pip` directly)

Do not switch an existing project to `uv` for the sake of it; if the project
already uses `uv` or another manager documented in its AGENTS.md, follow the
project's convention instead.