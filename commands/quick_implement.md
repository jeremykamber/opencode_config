---
description: One-shot workflow for quickly implementing smaller features with high quality
---

# Quick Implement

You are tasked with implementing a smaller feature or change in one streamlined workflow. This combines research, planning, implementation, and testing into a cohesive process.

## Initial Setup

When invoked, respond with:

```
I'm ready to implement your feature. Please provide:
1. The feature description or ticket (or path to a file)
2. Any specific requirements or constraints
3. Links to related research (optional)

I'll handle research, planning, implementation, and testing in one flow.
```

Then wait for the user to provide the feature context.

## Phase 1: Research & Planning (Combined)

### Step 1: Gather Context

Spawn a subagent and instruct it to:

1. **Read all mentioned files**:
   - If the user provides a ticket file, JSON, or docs, read them FULLY first.
   - Use the Read tool WITHOUT limit/offset.
   - Read these files yourself in the main context before proceeding.
   - If the user doesn't provide any ticket file, JSON, or docs, just proceed with the process.

2. **Initial Codebase Research**:
   - Find relevant files.
   - Understand how current code works.
   - Find historical context (previous plans, research in `thoughts/`)

3. **Analyze and Synthesize**:
   - Break down the feature into specific implementation steps.
   - Identify specific files that need changes.
   - Determine how to verify the feature works.
   - Create a todo list to track the implementation
    - Filename for todo list: `thoughts/shared/todo_lists/YYYY-MM-DD-ENG-XXXX-description.md`
        - Format: `YYYY-MM-DD-ENG-XXXX-description.md` where:
            - YYYY-MM-DD is today's date
            - ENG-XXXX is the ticket number (omit if no ticket)
            - description is a brief kebab-case description of the feature the todo list is for

### Step 2: Create Implementation Plan (Internal)

Do NOT write a plan file. Instead, create a detailed mental plan and structure your todo list accordingly. Your todo list should contain:

1. **Research Tasks**: Files to read, patterns to find.
2. **Test Writing Tasks**: Specific tests to write (TDD).
3. **Implementation Tasks**: Specific code changes to make.
4. **Verification Tasks**: Commands to run, things to verify.

Example structure:

```markdown
- [ ] Research: Analyze existing auth patterns in auth/
- [ ] Research: Find related API endpoints for user profile
- [ ] Test: Write unit tests for new user service methods
- [ ] Test: Write integration test for API endpoint
- [ ] Impl: Add user service methods for profile update
- [ ] Impl: Create API endpoint for profile update
- [ ] Verify: Run unit tests
- [ ] Verify: Run integration tests
- [ ] Verify: Run E2E test with browser agent
```

### Step 3: Present Plan

Present a summary of your understanding and the plan to the user:

```
Based on my research, here's my plan:

## Summary
[1-2 sentence description of what we're building]

## Implementation Steps
1. [Step 1] - [Brief description]
2. [Step 2] - [Brief description]

## Key Files
- `path/to/file1.ext` - [What changes]
- `path/to/file2.ext` - [What changes]

## Testing Strategy
- Unit tests: [Where and what]
- Integration tests: [Where and what]
- Manual/E2E: [What to verify]

Does this look right?
```

Wait for user confirmation before proceeding.

## Phase 2: Implementation (TDD)

### Step 1: Write Tests First

**CRITICAL**: You must write tests BEFORE writing any production code.

Spawn a subagent to:

1. **Write unit tests**:
   - Create test files in the appropriate test directory.
   - Follow existing test patterns in the codebase.
   - Write tests that cover the expected behavior of your changes.
   - Run tests to see them FAIL (this confirms your tests are valid). Follow RED (w/ skeleton(s) so it compiles) --> GREEN TDD pattern.

2. **Write integration tests** (if applicable):
   - Add tests for API endpoints or service integrations.
   - Ensure they fail initially.

That's ALL this subagent should do. When you spawn the subagent, make sure you give it the goal and functioanlity you want to achieve + test for.

### Step 2: Implement Feature

1. **Implement the minimal code** to make tests pass:
   - Follow **SOLID** and **KISS** principles.
   - Keep changes focused and small.
   - Reference existing patterns in the codebase.

2. **Run tests frequently**:
   - Run unit tests after each small change.
   - Fix failures immediately.

3. **Iterate**:
   - Add more tests if you find edge cases.
   - Refactor if needed, but keep tests passing.

### Step 3: Verify Implementation

1. **Run full test suite**:
   - Run `make check` or equivalent linter/type checker.
   - Run `make test` or equivalent test runner.
   - Fix any errors.

2. **Update todo list**:
   - Mark implementation tasks as complete.
   - Keep verification tasks pending.

## Phase 3: Testing (E2E)

### Step 1: Automated Verification

Run all automated checks:

- [ ] Linting passes
- [ ] Type checking passes
- [ ] Unit tests pass
- [ ] Integration tests pass

### Step 2: End-to-End Testing

If the feature has a UI or requires browser interaction:

1. **Launch Browser Agent**:
   - Use Task tool with `subagent_type: "browser"`.
   - Prompt it to navigate to the relevant URL.
   - Prompt it to perform the specific feature flow.
   - Prompt it to verify expected behavior.

2. **Report Results**:
   - If browser agent finds issues, fix them.
   - Re-run tests and browser agent until successful.

### Step 3: Final Report

Present the final results to the user:

```
## Implementation Complete

### Summary
[Brief description of what was implemented]

### Changes Made
- [File 1]: [Changes]
- [File 2]: [Changes]

### Tests Added
- [Test file]: [What it tests]

### Verification
- [x] Linting/Type Checking
- [x] Unit Tests
- [x] Integration Tests
- [x] E2E Testing (if applicable)

### Git
- Changes are ready for review.
```

## Important Principles

- **Todo List Stewarding**: ALWAYS keep your todo list up to date as you make changes and accomplish tasks. Always, before moving on to the next step, consult your todo list as well.
- **TDD First**: Always write tests before production code.
- **SOLID & KISS**: Keep code simple and well-structured.
- **Small Changes**: Prefer incremental changes over big bang implementations.
- **Iterate**: Don't be afraid to go back and adjust the plan if you discover something.
- **Verify Early**: Run tests frequently to catch issues early.
- **One-Shot**: While you iterate within phases, try to complete the feature in one flow rather than asking for multiple rounds of feedback.

## Handling Issues

If you encounter blockers:

1. **Stop**: Don't power through if something is wrong.
2. **Analyze**: Understand why the issue exists.
3. **Present**: Explain the issue clearly to the user.
4. **Ask**: Request guidance on how to proceed.

Example:

```
I ran into an issue:
Expected: [What should happen]
Found: [What actually happened]

How should I proceed?
```
