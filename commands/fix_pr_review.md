---
description: Inspect the current PR review comments, fix the correct ones, commit and push changes to the PR branch (conventional commits)
---

# Fix PR Review Comments

This command inspects the current pull request, reads reviewer comments, decides which comments are correct and safe to fix, applies the fixes with TDD where appropriate, and pushes focused commits to the PR branch. Commits follow conventional commit style.

IMPORTANT: This command operates on the current git working tree and remote branch. It MUST NOT force push or rewrite published history. If a suggested change is risky (public API, large behavioral change, or unclear intent), the command should NOT apply it automatically — instead leave a reply comment summarizing the reasoning and request clarification.

## High-level workflow

1. Identify the current PR and branch
2. Fetch review comments and inline code review notes
3. Triage comments into: fixable automatically, require clarification, out-of-scope, or defer
4. For each fixable comment:
   - Create or reuse feature branch/ worktree for fixes (prefer the PR branch)
   - Follow TDD: add failing tests when applicable, run tests, implement minimal changes to make tests pass
   - Keep commits atomic and conventional
5. Run full test suite and linters. If failing, revert or stop and report
6. Push commits to the PR branch and add a PR comment summarizing changes and which review comments were addressed

## Detailed steps and guidance

1) Locate PR and branch

- Determine the branch for the current PR:
  - If invoked inside the PR worktree, use `git rev-parse --abbrev-ref HEAD` to get current branch
  - Otherwise use `gh pr view --json headRefName,number` to discover the PR's branch name and number

2) Collect review comments

- Use `gh pr reviews <number>` and `gh pr view <number> --comments` (or equivalent) to fetch all review comments and PR review threads.
- For each comment/thread gather: author, file, line/position, comment body, suggestion blocks (if present), and review state (requested changes / comment / approved).

3) Triage comments

- Classify each comment into one of:
  1. Fixable automatically: small code changes, formatting, missing null checks, typos, test additions, small refactors.
  2. Needs clarification: ambiguous, design-level decisions, or behavioral changes that could break callers.
 3. Out-of-scope: unrelated to this PR's intent (note and skip).
 4. Risky: public API signature changes, database migrations, or large rewrites — DO NOT auto-apply.

- Criteria for automatic fix:
  - The change is local to a few lines and has clear guidance in the comment.
  - Tests can assert the expected behavior or there's a clear suggested code change in the comment's suggestion block.
  - No change to public APIs or persistent data schema.

4) Apply fixes (TDD-first)

- For each comment classified as fixable automatically:
  1. Create a focused commit series. Prefer making changes directly on the PR branch. If you prefer isolation, create `git checkout -b fix/pr-<num>/short-desc` and push to origin; otherwise work on the PR branch.
  2. Add failing tests FIRST if the comment concerns behavior, edge cases, or bugs.
  3. Run tests to confirm failing tests appear.
  4. Implement the minimal change to make the tests pass.
  5. Run full test suite and relevant linters.
  6. Commit changes in conventional style. Examples:

    - `fix(review): correct null check in pkg/foo.go to avoid panic`
    - `style(review): apply formatting and remove unused import in pkg/bar.go`
    - `test(review): add unit test for edge case when input is empty`

- Keep commits small and self-contained. Use one logical issue per commit.

5) Tests & Validation

- After every commit (or small set of commits), run the repo test suite: `make test` or `npm test` / language-specific command.
- Run linting and type checks if configured: `make lint`, `npm run typecheck`, `golangci-lint run`, etc.
- If a fix causes regressions outside the intended slice, stop and report; do NOT proceed further until reviewed.

6) Push and report

- Push commits to the PR branch: `git push origin HEAD` (no --force).
- Add a PR comment summarizing:
  - Which review comments were addressed (quote the comment and file:line), what changes were made (commit hashes), and which comments remain (with reasons: needs clarification, risky, out-of-scope).
  - If tests were added, indicate test command and coverage impact.

7) When skipping a comment

- Leave a reply inline explaining the reason (e.g., requires design decision, potential API break, or needs a follow-up PR), optionally propose a path forward and tag the reviewers.

## Safety and constraints

- NEVER force-push to the PR branch. Use feature branch approach if necessary and open a follow-up PR.
- Avoid touching unrelated files. Restrict changes to files referenced by review comments unless necessary for tests.
- For any change that alters behavior broadly or public APIs, create a proposal comment and request explicit reviewer approval before making the change.

## SOLID, KISS & TDD

- While applying fixes, follow SOLID and KISS principles. Prefer small focused methods and interfaces; avoid introducing complexity.
- Always prefer tests-first (TDD) for behavioral fixes to ensure no regressions.

## Example flow (summary)

```
# discover PR number
gh pr view --json number,headRefName

# fetch comments
gh pr view <num> --comments

# triage and apply fix 1
git checkout <pr-branch>
# write failing test
make test # see failures
# implement minimal fix
make test # should pass
git add -A && git commit -m "fix(review): description"
git push origin HEAD

# post PR comment summarizing addressed comments and commits
gh pr comment <num> --body "..."
```

## When to ask the human

- If more than N (configurable, default 3) review comments are ambiguous or risky, pause and ask the human which ones to prioritize.
- If a requested change touches public API, schema, or backward-incompatible behavior, request explicit approval before proceeding.

This command makes review-driven fixes safe, TDD-enforced, and traceable in conventional commits so reviewers can easily verify changes in the PR.
