---
description: Implement technical plans from thoughts/shared/plans with verification
---

# Implement Plan

You are tasked with implementing an approved technical plan from `thoughts/shared/plans/`. These plans contain phases with specific changes and success criteria.

## Getting Started

When given a plan path:

- Read the plan completely and check for any existing checkmarks (- [x])
- Read the original ticket and all files mentioned in the plan
- **Read files fully** - never use limit/offset parameters, you need complete context
- Think deeply about how the pieces fit together
- Create a todo list to track your progress
- Start implementing if you understand what needs to be done
- Start implementing only after creating tests that specify the expected behavior (TDD-first). The LLM must:
    1. Author tests for the phase that target full coverage for the new/changed code (unit and integration as appropriate).
    2. Add the tests first and run them to observe the expected failures.
    3. Implement the minimal code changes to make the tests pass.
    4. Iterate until tests pass and coverage targets are met.

If no plan path provided, ask for one.

## Implementation Philosophy

Plans are carefully designed, but reality can be messy. Your job is to:

- Follow the plan's intent while adapting to what you find
- Implement each phase fully before moving to the next
- Verify your work makes sense in the broader codebase context
- Update checkboxes in the plan as you complete sections

When things don't match the plan exactly, think about why and communicate clearly. Enforce TDD discipline: always write tests before changing production code and iterate until test suites pass.

If you encounter a mismatch:

- STOP and think deeply about why the plan can't be followed
- Present the issue clearly:

    ```
    Issue in Phase [N]:
    Expected: [what the plan says]
    Found: [actual situation]
    Why this matters: [explanation]

    How should I proceed?
    ```

## Verification Approach

After implementing a phase:

- Run the success criteria checks (usually `make check test` covers everything)
- Fix any issues before proceeding
- Update your progress in both the plan and your todos
- Check off completed items in the plan file itself using Edit
- **Pause for human verification**: After completing all automated verification for a phase, pause and inform the human that the phase is ready for manual testing. Use this format:

    ```
    Phase [N] Complete - Ready for Manual Verification

    Automated verification passed:
    - [List automated checks that passed]

    Please perform the manual verification steps listed in the plan:
    - [List manual verification items from the plan]

    Let me know when manual testing is complete so I can proceed to Phase [N+1].
    ```

If instructed to execute multiple phases consecutively, skip the pause until the last phase. Otherwise, assume you are just doing one phase.

do not check off items in the manual testing steps until confirmed by the user.

## If You Get Stuck

When something isn't working as expected:

- First, make sure you've read and understood all the relevant code
- Consider if the codebase has evolved since the plan was written
- Present the mismatch clearly and ask for guidance

Use sub-tasks sparingly - mainly for targeted debugging or exploring unfamiliar territory.

## Resuming Work

If the plan has existing checkmarks:

- Trust that completed work is done
- Pick up from the first unchecked item
- Verify previous work only if something seems off

Remember: You're implementing a solution, not just checking boxes. Keep the end goal in mind and maintain forward momentum.

## Implementation Details

When implementing, follow these principles:

- **Follow SOLID and KISS** throughout implementation.

    - **SOLID**: Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion. Example: extract one responsibility into a small class or function; depend on abstractions (interfaces) not concrete implementations.
    - **KISS**: Keep It Simple, Stupid — prefer clear, small functions over complex one-offs. Example: split a large 500-line function into smaller well-named helpers that each do one thing.

- **Code snippet policy**: include focused snippets showing only the lines that must change and any small surrounding context (preferably <= 120 lines). Do NOT paste entire files (no massive 1000+ line dumps). For larger contexts, provide file:line references and short summaries instead of full contents.
