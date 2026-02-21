---
description: Create implementation plan for highest priority item in backlog.md ready for spec
---

## PART I - IF AN ITEM IS MENTIONED

0c. read the item from ./backlog.md
0d. read the item details to learn about past implementations and research, and any questions or concerns about them

### PART I - IF NO ITEM IS MENTIONED

0.  read ./backlog.md
    0a. find the top priority items marked "ready for spec"
    0b. select the highest priority SMALL or XS issue from the list (if no SMALL or XS issues exist, EXIT IMMEDIATELY and inform the user)
    0c. read the selected item from backlog.md
    0d. read the item details to learn about past implementations and research, and any questions or concerns about them

### PART II - NEXT STEPS

think deeply

1. mark the item as "plan in progress" in ./backlog.md
   1a. read ./claude/commands/create_plan.md
   1b. determine if the item has a linked implementation plan document
   1d. if the plan exists, you're done, respond with a link to the item in backlog.md
   1e. if the research is insufficient or has unanswered questions, create a new plan document following the instructions in ./claude/commands/create_plan.md

think deeply

2. when the plan is complete, update the item in ./backlog.md with the plan link and mark as "plan in review"

think deeply, use TodoWrite to track your tasks. When reading from backlog.md, get the top items by priority but only work on ONE item - specifically the highest priority SMALL or XS sized issue.

### PART III - When you're done

Print a message for the user (replace placeholders with actual values):

```
✅ Completed implementation plan for [item id]: [item title]

Approach: [selected approach description]

The plan has been:

Created at thoughts/shared/plans/YYYY-MM-DD-[item-id]-description.md
Updated in backlog.md
Item marked as "plan in review" status

Implementation phases:
- Phase 1: [phase 1 description]
- Phase 2: [phase 2 description]
- Phase 3: [phase 3 description if applicable]

View the item in backlog.md
```
