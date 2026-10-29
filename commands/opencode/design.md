---
description: Produce a phased design/implementation plan before coding
---

Before writing any code for $ARGUMENTS, produce a plan as a message (not a file unless asked):

1. **Context** — what you learned from the relevant code (with `file:line` refs) and the
   constraints that shape the approach.
2. **Approach** — the recommended design plus at most two rejected alternatives, each with
   its main tradeoff. One recommendation, not a survey.
3. **Verification** — how the result will be exercised end-to-end (which surface, which
   offline/test path, what you expect to observe).

Stop and wait for the user's go-ahead. Implement only after they agree. Keep changes scoped
to what the plan requires.