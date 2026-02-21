---
description: Implement highest priority small item in backlog.md with worktree setup
---

## PART I - IF AN ITEM IS MENTIONED

0c. read the selected item from ./backlog.md
0d. read the item details to understand the implementation plan and any concerns

## PART I - IF NO ITEM IS MENTIONED

0.  read ./backlog.md
    0a. find the top priority items in status "ready for dev"
    0b. select the highest priority SMALL or XS issue from the list (if no SMALL or XS issues exist, EXIT IMMEDIATELY and inform the user)
    0c. read the selected item from backlog.md
    0d. read the item details to understand the implementation plan and any concerns

## PART II - NEXT STEPS

think deeply

1. mark the item as "in dev" in ./backlog.md
   1a. identify the linked implementation plan document
   1b. if no plan exists, mark as "ready for spec" and EXIT with an explanation

think deeply about the implementation

2. set up worktree for implementation:
   2a. read `hack/create_worktree.sh` and create a new worktree with the item id: `./hack/create_worktree.sh [item-id] BRANCH_NAME`
   2b. implement the plan, and when you are done implementing and all tests pass, read ./claude/commands/commit.md and create a commit, then read ./claude/commands/describe_pr.md and create a PR, then update the item in backlog.md with the PR link

think deeply, use TodoWrite to track your tasks. When reading from backlog.md, get the top items by priority but only work on ONE item - specifically the highest priority SMALL or XS sized issue.
