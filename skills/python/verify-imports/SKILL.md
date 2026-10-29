---
name: Verify Python Imports
description: Verify Python code compiles and imports correctly with targeted checks instead of running entrypoints
---

Use targeted module checks to verify Python changes. Do not run application
entrypoints (long-running servers, scripts with side effects) to test code.

## Workflow

1. For each edited module, run an import check first:

```sh
python -c "import package.module"
```

2. For a quick syntax check of a single file:

```sh
python -m py_compile path/to/file.py
```

3. When a small amount of behavior needs validating, use inline snippets:

```sh
python -c "from package import thing; thing.function()"
```

4. Only run test suites or full entrypoints when the AGENTS.md documents
   them or the user explicitly asks.