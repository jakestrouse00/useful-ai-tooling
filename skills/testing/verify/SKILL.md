---
name: Verify
description: Verify a code change by exercising it end-to-end and observing behavior — drive the affected code path, don't settle for tests or import-and-call. Adapted from Claude Code 2.1.303's shipped verify skill.
---

**Verification is observation of the change actually running.** Not "tests pass". A test
that ran green proves your assertions matched the code; it proves nothing about the change
until the affected code path itself has been driven and its behavior observed.

**The diff is ground truth. Any description of the change is a claim about the diff.**
Establish the full scope first (branch range or working tree):

```bash
git diff HEAD --stat          # uncommitted
git log --oneline -3 --stat   # recent commits
```

Read the diff. If a summary (task, ticket, message) disagrees with it, that is a finding.

## Surface

The surface is where a user — human or programmatic — meets the change. That is where
you observe. Identify which surface the diff actually reaches, then drive that surface:

| Change reaches | Surface here | You |
|---|---|---|
| Library/module code | the library boundary | import the package from a caller's perspective and drive the public API; never poke deep internals directly |
| CLI | the command line | run the subcommand the change affects, with flags it documents; capture the output |
| HTTP/web service | the endpoint or server | exercise the route — against a mock/recorded transport, a test harness, or a locally running instance; watch request/response + logs |
| UI | the screen | open it, interact with the changed flow, observe what renders or fails |
| MCP/tool server | the tool interface | call the tool a client would, via the real server protocol |
| Tests themselves | no runtime surface | SKIP — say so in one line; do not run the suite to fill the space |

Keep the repo's own discipline when driving surfaces: if AGENTS.md or project docs
prescribe offline modes, recorded fixtures, background runs, or targeted checks
(never long-running entrypoints), follow those rules as your verification environment.

**Internal function? Not a surface.** Something in the repo calls it and that caller ends at
one of the rows above. Follow it there.

## Report

End with one of:

- **VERIFIED** — what you drove, what you observed, one path from diff → surface → evidence.
- **SKIP — no runtime surface: (reason)** — test/doc-only diffs, config with no behavioral
  change. One line, no filler runs.
- **BLOCKED — (reason)** — say exactly what you could not reach and why.

A verdict without evidence is a hypothesis. Show the observation or change the verdict.