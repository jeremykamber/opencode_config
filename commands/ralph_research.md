---
description: Research highest priority item in backlog.md needing investigation
---

## PART I - IF AN ITEM IS MENTIONED

0c. read the selected item from ./backlog.md
0d. read the item details to understand what research is needed and any previous attempts

## PART I - IF NO ITEM IS MENTIONED

0.  read ./backlog.md
    0a. find the top priority items in status "research needed"
    0b. select the highest priority SMALL or XS issue from the list (if no SMALL or XS issues exist, EXIT IMMEDIATELY and inform the user)
    0c. read the selected item from backlog.md
    0d. read the item details to understand what research is needed and any previous attempts

## PART II - NEXT STEPS

think deeply

1. mark the item as "research in progress" in ./backlog.md
   1a. read any linked documents to understand context
   1b. if insufficient information to conduct research, add a comment asking for clarification and move back to "research needed"

think deeply about the research needs

2. conduct the research:
   2a. read .claude/commands/research_codebase.md for guidance on effective codebase research
   2b. if the item details suggest web research is needed, use WebSearch to research external solutions, APIs, or best practices
   2c. search the codebase for relevant implementations and patterns
   2d. examine existing similar features or related code
   2e. identify technical constraints and opportunities
   2f. Be unbiased - don't think too much about an ideal implementation plan, just document all related files and how the systems work today
   2g. document findings in a new thoughts document: `thoughts/shared/research/YYYY-MM-DD-[item-id]-description.md`
    - Format: `YYYY-MM-DD-[item-id]-description.md` where:
        - YYYY-MM-DD is today's date
        - [item-id] is the item identifier
        - description is a brief kebab-case description of the research topic
    - Examples:
        - With item: `2025-01-08-item-123-parent-child-tracking.md`
        - Without item: `2025-01-08-error-handling-patterns.md`

think deeply about the findings

3. synthesize research into actionable insights:
   3a. summarize key findings and technical decisions
   3b. identify potential implementation approaches
   3c. note any risks or concerns discovered

4. update the item:
   4a. update the item in ./backlog.md with the research document link
   4b. add a comment summarizing the research outcomes
   4c. mark the item as "research in review" in ./backlog.md

think deeply, use TodoWrite to track your tasks. When reading from backlog.md, get the top items by priority but only work on ONE item - specifically the highest priority issue.

## PART III - When you're done

Print a message for the user (replace placeholders with actual values):

```
✅ Completed research for [item id]: [item title]

Research topic: [research topic description]

The research has been:

Created at thoughts/shared/research/YYYY-MM-DD-[item-id]-description.md
Updated in backlog.md
Item marked as "research in review" status

Key findings:
- [Major finding 1]
- [Major finding 2]
- [Major finding 3]

View the item in backlog.md
```
