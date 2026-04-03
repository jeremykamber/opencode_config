---
description: Implement the recommended SOLID & KISS refactors from a solid analysis report
---

# Implement SOLID & KISS Refactors

This command consumes a report created by `solid_analysis` (a file in `thoughts/shared/solid_analyses/YYYY-MM-DD-HHmm-full-solid-analysis.md`) and implements the recommended prioritized refactors to make the codebase more SOLID and KISS-compliant.

Behavioral overview:

- This is the second step in a 1 -> 2 workflow: `solid_analysis` produces the report; `implement_solid` applies the changes.
- The command must use many focused subagents to implement changes slice-by-slice, following the prioritized roadmap in the report.
- All implementation MUST follow TDD: tests are written first (by LLM), tests run and fail, then the minimal code changes are implemented to make tests pass.

## Process

1. Input

    - Require a path to the solid analysis report. If not provided, list recent reports and ask the user to choose.

2. Read the report

    - Parse the report fully, extracting the top priorities, per-slice recommendations, and suggested roadmap.

3. Create worktree & branch

    - Create a dedicated worktree/branch for the implementation: `solid/refactor/YYYY-MM-DD-HHmm`.

4. Spawn implementation subagents

    - For each prioritized item (from highest priority downward), spawn a subagent that:
        - Is instructed to follow TDD strictly (add tests first, run tests, implement minimal code, iterate until passing).
        - Uses the `rpi-implement` or `solid-slice-analysis` skill for guidance when performing non-trivial refactors.
        - Produces small, focused commits: tests first commit, implementation commit, refactor cleanup commit.

5. Commit strategy

    - Keep commits small and descriptive. For each refactor:
        1. `test: add failing tests for <slice>/<issue>`
        2. `fix: implement <small change> to satisfy tests`
        3. `refactor: cleanup <slice> to follow SOLID/KISS`

6. Verification after each item

    - Run full test suite (or slice-level tests) and check coverage metrics; ensure no regressions outside the slice.

7. Reporting progress

    - After finishing each prioritized item, append a short progress note to the original solid analysis report under a new section `Implementation Progress` with timestamps, commits, and brief rationale.

8. Finalization

    - When all top-priority items are implemented (or you stop per user instruction), push the branch and prepare a PR using the standard `describe_pr` flow.

## Safety & Constraints

- Non-destructive defaults: do not force-push main branches; use feature branches and leave final merge to human reviewers.
- Do not attempt large-scale automated API changes without human confirmation — if a refactor may break public APIs, mark it as `requires human review` and stop.

## SOLID and KISS reminder

- While implementing, follow SOLID and KISS as defined in the analysis command. Favor small focused classes/functions and interface-driven design.

## Example invocation

```
/implement_solid at thoughts/shared/solid_analyses/2026-02-28-1345-full-solid-analysis.md
```

This command is intended to be conservative and TDD-driven — it automates refactors where safe and small, and surfaces anything that needs human judgment.
