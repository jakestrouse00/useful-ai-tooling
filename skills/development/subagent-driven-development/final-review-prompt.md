# Final Whole-Branch Review Prompt Template

Use this template when dispatching the final whole-branch reviewer after all
tasks are complete. The reviewer reads the branch's net diff once and judges
the change as a merge candidate.

```
Subagent (general-purpose):
  description: "Final whole-branch review"
  model: [MODEL — REQUIRED: use the most capable available model per
         SKILL.md Model Selection; an omitted model silently inherits the
         session's model]
  prompt: |
    You are performing the final whole-branch code review before merge. A
    series of task-scoped reviews (spec + quality per task) already passed;
    your job is the cross-cutting view those scoped reviews cannot take.

    ## Scope

    **Base (merge-base):** [BASE_SHA]
    **Head:** [HEAD_SHA]
    **Diff file:** [DIFF_FILE]

    Read the diff file once — it contains the commit list, a stat summary,
    and the full net diff with surrounding context. Do not re-run git
    commands and do not crawl the broader codebase; inspect code outside
    the diff only to evaluate a concrete, named risk.

    Your review is read-only on this checkout. Do not mutate the working
    tree, the index, HEAD, or branch state in any way.

    ## You Do Not Dispatch Subagents

    Do all of this review yourself. Never spawn a subagent to review part
    of the diff, and never spawn another reviewer for a second opinion.

    ## What to Review (whole-branch concerns)

    - **Coherence:** the tasks compose into the stated design — no half
      wiring, duplicated helpers, or two tasks solving the same problem two
      ways. Interfaces introduced by one task match how later tasks use them.
    - **Cross-task regressions:** the net effect of a change each task-scoped
      review could not see (the branch touches the same file twice, an
      earlier task's behavior was overwritten by a later fix).
    - **Integration seams:** errors crossing layer boundaries, config keys
      used by one task but created by another, migrations ordering.
    - **Test integrity:** the branch's tests assert real behavior; nothing
      was weakened to make a fix round pass.
    - **Requirements coverage:** the plan's Global Constraints hold across
      the whole branch, not just within single tasks.

    Do not re-review each task's internal quality in depth — that already
    happened. Findings should be things only the whole-branch view can see.

    ## Deferred Findings Triage

    The controller will list deferred minors and parked findings from the
    task reviews. Triage each: must be fixed before merge, or acceptable to
    merge as-is (say why). Judge them on their merits — the list is input,
    not verdict.

    ## Output Format

    ### Strengths
    [What's well done? Be specific.]

    ### Deferred/Parked Triage
    [One line each: fix before merge | acceptable — why]

    ### Issues

    #### Critical (Must Fix)
    #### Important (Should Fix)
    #### Minor (Nice to Have)

    For each issue: file:line, what's wrong, why it matters, how to fix
    (if not obvious).

    ### Assessment

    **Branch quality:** [Approved | Needs fixes — one fix dispatch will run;
    keep the list minimal and load-bearing]

    Your final message is the report itself: begin directly with Strengths.
```

**Placeholders:**
- `[MODEL]` — REQUIRED: the most capable available model (final review is an
  architecture-class judgment per SKILL.md Model Selection)
- `[BASE_SHA]` — merge-base (the commit the branch started from, e.g.
  `git merge-base main HEAD`)
- `[HEAD_SHA]` — current commit
- `[DIFF_FILE]` — REQUIRED: path the controller wrote the branch review
  package to (`bash scripts/review-package PLAN_FILE MERGE_BASE HEAD`)

**Reviewer returns:** Strengths, deferred/parked triage, whole-branch Issues
(Critical/Important/Minor), Branch quality verdict.