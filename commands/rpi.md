---
description: Orchestrate a full Research, Plan, Implement cycle using parallel subagents with iterative approval and verification
---

# RPI - Research, Plan, Implement Cycle

You are tasked with orchestrating a complete Research, Plan, Implement (RPI) cycle for software development tasks. This command manages subagents through each phase, with iterative human-like approval at key points. The process ensures high-quality outcomes through structured documentation, verification, and proper git workflow.

## Initial Setup

When this command is invoked:

1. **Parse the input**:

    - Extract the task description from the user input
    - Determine the type of work (feature, refactor, hotfix, etc.) to inform branch naming
    - If no specific task provided, ask for clarification

2. **Branch Creation**:

    - Analyze the task to determine appropriate branch prefix:
        - `feat/` for new features or enhancements
        - `refactor/` for code restructuring
        - `hotfix/` for critical bug fixes
        - `chore/` for maintenance tasks
    - Check available branches: `git branch -r`
    - Create new branch: `git checkout -b {prefix}/{task-slug}`
    - Push the branch: `git push -u origin {branch-name}`

3. **Initial Response**:

    ```
    I'll orchestrate a complete RPI cycle for your task. This involves:
    1. Research phase with parallel subagents
    2. Plan creation with iterative approval
    3. Implementation with verification
    4. Final review and git workflow

    Starting with branch creation and research...
    ```

## RPI Process Phases

### Phase 1: Research

1. **Spawn Research Subagent**:

    - Spawn a subagent and instruct it to use the `rpi-research` skill to conduct comprehensive research on the parts of the codebase relevant to the implementation. The agent type should remain default. IT SHOULD DO THE RESEARCH ON IT'S OWN, WITHOUT SPAWNING SUBAGENTS WITHIN ITS OWN PROCESS.
    - Prompt: "Use the `rpi-research` skill and conduct comprehensive research for the task: {task description}. Make sure to add and commit your research doc to git."

2. **Subagent Execution**:

    - The RPI research skill will:
        - find relevant files and components
        - Understand the current implementation
        - Identify existing patterns
        - Gather historical context
        - Search for external best practices on the web if needed (using web search tool)

3. **Research Output Review**:
    - Read the generated research document from `thoughts/shared/research/YYYY-MM-DD-{task-slug}.md`
    - Evaluate completeness, accuracy, and relevance to topic/implementation
    - If insufficient, provide specific feedback to the RPI skill for additional research
    - Approve or request revisions before proceeding

### Phase 2: Plan

1. **Spawn Planning Subagent**:

    - Spawn a subagent and tell to use the `rpi-plan` skill to create a comprehensive plan for implementation. Be sure to pass the research document generated in the last phase in there as well. Include the research doc, and the goal of the plan. Keep the agent type default. Make sure to tell it that IT SHOULD DO THE PLANNING ON IT'S OWN, WITHOUT SPAWNING SUBAGENTS WITHIN ITS OWN PROCESS.
    - Prompt example: "Use the `rpi-plan` skill, and create a detailed implementation plan based on the research in thoughts/shared/research/YYYY-MM-DD-{task-slug}.md."

2. **Subagent Execution**:

    - The RPI planning skill will:
        - Analyze research findings
        - Generate a comprehensive plan using existing plan templates
        - Include success criteria, phases, and verification steps

3. **Plan Review and Iteration**:
    - Read the generated plan from `thoughts/shared/plans/YYYY-MM-DD-{task-slug}.md`
    - Review for:
        - Technical feasibility
        - Completeness of phases
        - Measurable success criteria
        - Risk assessment
    - Provide specific feedback for revisions if needed
    - Iterate until plan meets quality standards

### Phase 3: Implement

1. **Spawn Implementation Subagent**:

    - Spawn a subagent that uses the `rpi-implement` skill to carry out implementation following TDD. Keep the agent type default.
    - Prompt example: "Use the `rpi-implement` skill to implement the plan from thoughts/shared/plans/YYYY-MM-DD-{task-slug}.md. Ensure all of your code is of the highest quality, follows SOLID and KISS principles, enforces TDD (tests-first). Provide progress reports after each test/code iteration."

2. **Subagent Execution (TDD-first)**:

    - The RPI implement skill will follow a strict TDD workflow for each implementation phase:
        - Write tests first: the LLM must author unit/integration tests that provide full coverage for the planned change (happy paths, edge cases, and error handling) before adding production code.
        - Run tests: execute the test suite; expect failures initially.
        - Implement to satisfy tests: make minimal, focused code changes to make tests pass.
        - Iterate: expand tests as needed for missing edge cases, then adjust code until all tests pass and coverage targets are met.
        - Create worktree if needed and stage changes as distinct test-first commits (tests added first, then code changes that make them pass).
        - Run automated tests and verifications after each change and document progress and any deviations.

3. **Implementation Monitoring**:
    - Monitor subagent progress through intermediate outputs
    - Enforce TDD discipline: each phase must show test files created first, failing tests demonstrating the expected behavior, then commits that implement the behavior and make tests pass
    - Intervene if issues arise, providing guidance
    - Ensure each phase's success criteria (including tests passing and coverage thresholds) are met before proceeding

### Phase 4: Verification

1. **Verify Correctness**:

    - Verify the implementation against the plan requirements. Run all automated tests, check success criteria, and identify any remaining issues.

2. **Final Review**:
    - Perform your final check:
        - Code quality and patterns
        - Test coverage
        - Documentation completeness
        - Edge case handling
    - Approve final implementation or request fixes

## Git Workflow and PR Creation (DO NOT do the PR part if the user requests otherwise)

### Commit Changes

1. **Stage and Commit**:
    - Run `git add .`
    - Use the `commit` command with appropriate message based on the task type
    - Ensure commit follows repository conventions

### PR Creation

1. **Determine Target Branch**:

    - Check available branches: `git branch -r | grep -E "(dev|development|develop|staging)"`
    - Prefer `dev`, `development`, or `develop` branch
    - Only target `main`/`master` if no development branch exists

2. **Create PR Description**:

    - Generate detailed PR description using the `describe_pr` command workflow
    - Include links to research and planning documents
    - Write comprehensive PR description file to `./prs/{pr-number}_description.md` with:
        - Task summary and objectives
        - Research findings summary
        - Implementation approach
        - Key changes and technical decisions
        - Testing performed
        - Links to research: `thoughts/shared/research/YYYY-MM-DD-{task-slug}.md`
        - Links to plan: `thoughts/shared/plans/YYYY-MM-DD-{task-slug}.md`

3. **Create PR**:
    - Use `gh pr create --title "{task title}" --body-file ./prs/{pr-number}_description.md --base {target-branch}`
    - Ensure PR has proper labels and assignees if applicable

## Quality Assurance

- **Iterative Approval**: At each phase transition, review subagent outputs thoroughly
- **Documentation Links**: Ensure all artifacts are properly linked and accessible
- **Testing Emphasis**: Require automated test success at each implementation phase
- **Risk Assessment**: Evaluate and mitigate technical risks throughout the process
- **Branch Strategy**: Never work directly on main/master; always use feature branches

## Error Handling

- If research is insufficient, loop back with specific additional requirements
- If plan has gaps, request targeted revisions
- If implementation fails verification, provide specific fix guidance
- If git operations fail, diagnose and resolve branch/PR issues
- Always maintain clear communication about progress and issues

## Success Criteria

- Research document provides complete technical context
- Plan includes measurable success criteria for each phase
- Implementation passes all automated verification steps
- Code follows established patterns and conventions
- Documentation is comprehensive and linked
- PR is created with detailed description and proper targeting
- All changes are committed with clear messages

This command ensures a disciplined, high-quality development process with parallel efficiency and iterative improvement.
