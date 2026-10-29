# Python rules

- Prefer `pip` over `uv` for Python installs. Use `pip install --user`
  for global CLI tools, or install into the project's virtualenv.
- When asked to verify Python, use targeted `python -c "..."` imports,
  never `python main.py` or any long-running entrypoint.