---
description: Analyze the entire codebase for SOLID and KISS compliance, slice it, and produce a consolidated actionable report
---

# SOLID & KISS Codebase Analysis

This command analyzes a repository at scale using parallel subagents, breaks the codebase into logical "slices", evaluates each slice for SOLID and KISS adherence, and generates a single concise but actionable report at:

    thoughts/shared/solid_analyses/YYYY-MM-DD-HHmm-full-solid-analysis.md

Behavioral overview:

- Use many focused subagents in parallel to explore, analyze, and score the codebase.
- Produce a reproducible report with a clear remediation plan (prioritized, with examples and small patch suggestions) — concise but actionable.
- Do NOT dump entire files into the report. Provide short focused snippets (<= 120 lines) when necessary, otherwise use file:line references and summaries.

## When to run

- Run this when you want a repository-wide health assessment focusing on architecture, modularity, and maintainability.

## Process

1. Parse input

    - If a path or target modules are provided, focus there; otherwise analyze the whole repo root.
    - If no repo context available, ask for the repository path.

2. Spawn parallel subagents (skill instructions)

    - IMPORTANT: keep agent type DEFAULT, but instruct the RPI-style orchestrator to spawn subagents that USE the following skills (these are skill instructions, not agent-type changes):
        - `solid-broad-analysis` — scan repository layout and propose slices
        - `solid-slice-analysis` — inspect a single slice in depth for SOLID/KISS adherence
        - `codebase-locator` — find files, tests, and build scripts per slice
        - `codebase-analyzer` — extract control/data flow and dependencies
        - `codebase-pattern-finder` — discover repeating patterns and antipatterns
        - `thoughts-locator` — fetch any existing architecture docs or previous notes
        - `tests-analyzer` (or equivalent) — evaluate existing tests and coverage

    - Spawn many slice-analysis subagents in parallel: each subagent gets a specific slice (package/folder/component) and runs the deeper `solid-slice-analysis` skill.

3. Slicing strategy

    - The broad-analysis agent should propose slices using heuristics: package boundaries, folders, modules, feature areas, and high cyclomatic complexity hotspots.
    - Each slice should have a short identifier and list of files (file:line ranges) that belong to it.

4. Per-slice analysis

    Each `solid-slice-analysis` subagent must produce:

    - Slice summary: purpose, public surface, key entry points
    - SOLID adherence checklist (one-paragraph for each SOLID principle): where it's followed, where it's not
    - KISS assessment: areas that are unnecessarily complex and suggestions to simplify
    - Code smells / antipatterns found (with file:line refs)
    - Test coverage summary for the slice and missing test targets
    - Concrete, minimal refactor suggestions (one per issue) with example snippet <= 120 lines or a patch idea and approximate effort (small/medium/large)
    - Suggested priority (1-5) for each suggested refactor and rationale

5. Synthesis and scoring

    - Combine slice outputs into a repository-level report with:
        - Global snapshot: total slices, avg SOLID score, avg complexity, test coverage
        - Top 10 refactor opportunities (prioritized)
        - Risk map: parts likely to cause regressions
        - Quick wins vs long-term work

6. Output format (report)

    The report written to `thoughts/shared/solid_analyses/YYYY-MM-DD-HHmm-full-solid-analysis.md` should follow this structure:

    ## Executive Summary

    - One-paragraph health summary (scores and top 3 priorities)

    ## Metrics

    - Slices analyzed: N
    - Average SOLID score: X/100 (explain scoring: each principle 0-20)
    - Average KISS score: Y/100
    - Test coverage (repo / weighted-by-slice)

    ## Top Priorities (1-10)

    - For each: slice id, short description, impact, suggested immediate action

    ## Per-Slice Findings (concise)

    - Slice: <id> — short summary
        - SOLID score: 74/100
        - KISS score: 62/100
        - Key issues:
            1. [file:line] - Single function > 400 LOC; violates SRP. Suggest: split into X, Y.
            2. [file:line] - Concrete dependencies instead of interfaces; violates DIP. Suggest: introduce adapter interface.
        - Suggested change (small snippet or patch idea):
            ```patch
            # short patch suggestion (<=120 lines) or callable refactor plan
            ```

    ## Test Coverage Gaps

    - List of critical uncovered behaviors per slice that should be covered by tests

    ## Recommended Implementation Plan

    - Short roadmap (phases) that the implement command should follow

    ## Appendix: Tooling & Data

    - How slices were derived, the subagents run, and raw metrics location (if any)

7. Write the report

    - Create directory `thoughts/shared/solid_analyses/` if missing and write the file using the timestamp format above.
    - Ensure the report contains file:line references, focused snippets, and clear next steps.

## Important Guidelines

- Do NOT paste entire files. Limit code snippets to <= 120 lines. Use file:line references for larger contexts.
- Follow SOLID and KISS definitions below when evaluating and recommending changes.

## SOLID and KISS (definitions for the analysis)

- SOLID:
  - Single Responsibility Principle (SRP): a class/module should have one reason to change.
  - Open/Closed Principle (OCP): modules should be open for extension but closed for modification.
  - Liskov Substitution Principle (LSP): subclasses must be substitutable for their base types.
  - Interface Segregation Principle (ISP): prefer many specific interfaces over a fat one.
  - Dependency Inversion Principle (DIP): depend upon abstractions, not concretions.

- KISS: Keep It Simple, Stupid — favor simple straightforward solutions and small well-named functions.

## Verification

- The command should run the repo's test suite (if available) and gather coverage numbers but must not modify code. It only writes the analysis report.

## Example invocation

```
/solid_analysis at ./ (or at path/to/repo)
```

The report is intended to be a high-signal roadmap that a follow-up command can consume to implement prioritized changes.
