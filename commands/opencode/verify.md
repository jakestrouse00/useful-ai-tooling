---
description: Verify the current change by driving it — adapted from Claude Code's verify skill
---

Invoke the @verify skill on the current uncommitted change (or the change identified by
$ARGUMENTS). Follow it exactly: establish the diff scope, find the surface, drive that
surface while honoring this repo's documented verification rules, and report
VERIFIED, SKIP, or BLOCKED with evidence.