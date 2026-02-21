---
description: Manage backlog.md items - create, update, comment, and follow workflow patterns
---

# Backlog - Item Management

You are tasked with managing backlog.md items, including creating items from thoughts documents, updating existing items, and following the team's specific workflow patterns.

## Initial Setup

Respond based on the user's request:

### For general requests:

```
I can help you with backlog.md items. What would you like to do?
1. Create a new item from a thoughts document
2. Add a comment to an item (I'll use our conversation context)
3. Search for items
4. Update item status or details
```

### For specific create requests:

```
I'll help you create a backlog.md item from your thoughts document. Please provide:
1. The path to the thoughts document (or topic to search for)
2. Any specific focus or angle for the item (optional)
```

Then wait for the user's input.

## Team Workflow & Status Progression

The team follows a specific workflow to ensure alignment before code implementation:

1. **Triage** → All new items start here for initial review
2. **Spec Needed** → More detail is needed - problem to solve and solution outline necessary
3. **Research Needed** → Item requires investigation before plan can be written
4. **Research in Progress** → Active research/investigation underway
5. **Research in Review** → Research findings under review (optional step)
6. **Ready for Plan** → Research complete, item needs an implementation plan
7. **Plan in Progress** → Actively writing the implementation plan
8. **Plan in Review** → Plan is written and under discussion
9. **Ready for Dev** → Plan approved, ready for implementation
10. **In Dev** → Active development
11. **Code Review** → PR submitted
12. **Done** → Completed

**Key principle**: Review and alignment happen at the plan stage (not PR stage) to move faster and avoid rework.

## Important Conventions

### URL Mapping for Thoughts Documents

When referencing thoughts documents, always provide GitHub links:

- `thoughts/shared/...` → `https://github.com/humanlayer/thoughts/blob/main/repos/humanlayer/shared/...`
- `thoughts/allison/...` → `https://github.com/humanlayer/thoughts/blob/main/repos/humanlayer/allison/...`
- `thoughts/global/...` → `https://github.com/humanlayer/thoughts/blob/main/global/...`

### Default Values

- **Status**: Always create new items in "Triage" status
- **Priority**: Default to Medium for most tasks, use best judgment or ask user
    - Urgent: Critical blockers, security issues
    - High: Important features with deadlines, major bugs
    - Medium: Standard implementation tasks (default)
    - Low: Nice-to-haves, minor improvements

### Automatic Label Assignment

Automatically apply labels based on the item content:

- **hld**: For items about the `hld/` directory (the daemon)
- **wui**: For items about `humanlayer-wui/`
- **meta**: For items about commands, thoughts tool, or `thoughts/` directory

Note: meta is mutually exclusive with hld/wui. Items can have both hld and wui, but not meta with either.

## Action-Specific Instructions

### 1. Creating Items from Thoughts

#### Steps to follow after receiving the request:

1. **Locate and read the thoughts document:**

    - If given a path, read the document directly
    - If given a topic/keyword, search thoughts/ directory using Grep to find relevant documents
    - If multiple matches found, show list and ask user to select
    - Create a TodoWrite list to track: Read document → Analyze content → Draft item → Get user input → Create item

2. **Analyze the document content:**

    - Identify the core problem or feature being discussed
    - Extract key implementation details or technical decisions
    - Note any specific code files or areas mentioned
    - Look for action items or next steps
    - Identify what stage the idea is at (early ideation vs ready to implement)
    - Take time to ultrathink about distilling the essence of this document into a clear problem statement and solution approach

3. **Check for related context (if mentioned in doc):**

    - If the document references specific code files, read relevant sections
    - If it mentions other thoughts documents, quickly check them
    - Look for any existing backlog.md items mentioned

4. **Draft the item summary:**
   Present a draft to the user:

    ```
    ## Draft Backlog Item

    **Title**: [Clear, action-oriented title]

    **Description**:
    [2-3 sentence summary of the problem/goal]

    ## Key Details
    - [Bullet points of important details from thoughts]
    - [Technical decisions or constraints]
    - [Any specific requirements]

    ## Implementation Notes (if applicable)
    [Any specific technical approach or steps outlined]

    ## References
    - Source: `thoughts/[path/to/document.md]` ([View on GitHub](converted GitHub URL))
    - Related code: [any file:line references]

    ---
    Based on the document, this seems to be at the stage of: [ideation/planning/ready to implement]
    ```

5. **Interactive refinement:**
   Ask the user:

    - Does this summary capture the item accurately?
    - What priority? (Default: Medium)
    - Any additional context to add?
    - Should we include more/less implementation detail?

    Note: Item will be created in "Triage" status by default.

6. **Create the backlog.md item:**
   Add the item to ./backlog.md with:

    - title: [refined title]
    - description: [final description in markdown]
    - priority: [selected priority, default Medium]
    - status: Triage
    - labels: [apply automatic label assignment from above]
    - links: [GitHub URLs]

7. **Post-creation actions:**
    - Show the item location in backlog.md
    - Ask if user wants to:
        - Add a comment with additional implementation details
        - Update the original thoughts document with the item reference
    - If yes to updating thoughts doc:
        ```
        Add at the top of the document:
        ---
        backlog_item: [item id or reference]
        created: [date]
        ---
        ```

### 2. Adding Comments and Links to Existing Items

When user wants to add a comment to an item:

1. **Determine which item:**

    - Use context from the current conversation to identify the relevant item
    - If uncertain, read backlog.md to show item details and confirm with user
    - Look for item references in recent work discussed

2. **Format comments for clarity:**

    - Attempt to keep comments concise (~10 lines) unless more detail is needed
    - Focus on the key insight or most useful information for a human reader
    - Not just what was done, but what matters about it
    - Include relevant file references with backticks and GitHub links

3. **File reference formatting:**

    - Wrap paths in backticks: `thoughts/allison/example.md`
    - Add GitHub link after: `([View](url))`
    - Do this for both thoughts/ and code files mentioned

4. **Comment structure example:**

    ```markdown
    Implemented retry logic in webhook handler to address rate limit issues.

    Key insight: The 429 responses were clustered during batch operations,
    so exponential backoff alone wasn't sufficient - added request queuing.

    Files updated:

    - `hld/webhooks/handler.go` ([GitHub](link))
    - `thoughts/shared/rate_limit_analysis.md` ([GitHub](link))
    ```

5. **Handle links properly:**
    - Add the comment to the item in backlog.md
    - Include links in the comment

### 3. Searching for Items

When user wants to find items:

1. **Gather search criteria:**

    - Query text
    - Status filters
    - Priority filters

2. **Execute search:**
   Read ./backlog.md and filter items by:

    - query: [search text]
    - status: [if specified]
    - priority: [if specified]

3. **Present results:**
    - Show item ID, title, status, priority
    - Include references to backlog.md

### 4. Updating Item Status

When moving items through the workflow:

1. **Get current status:**

    - Read item details from backlog.md
    - Show current status in workflow

2. **Suggest next status:**

    - Triage → Spec Needed (lacks detail/problem statement)
    - Spec Needed → Research Needed (once problem/solution outlined)
    - Research Needed → Research in Progress (starting research)
    - Research in Progress → Research in Review (optional, can skip to Ready for Plan)
    - Research in Review → Ready for Plan (research approved)
    - Ready for Plan → Plan in Progress (starting to write plan)
    - Plan in Progress → Plan in Review (plan written)
    - Plan in Review → Ready for Dev (plan approved)
    - Ready for Dev → In Dev (work started)

3. **Update with context:**
   Edit ./backlog.md to update the status.
   Consider adding a comment explaining the status change.

## Important Notes

- Keep items concise but complete - aim for scannable content
- All items should include a clear "problem to solve" - if the user asks for an item and only gives implementation details, you MUST ask "To write a good item, please explain the problem you're trying to solve from a user perspective"
- Focus on the "what" and "why", include "how" only if well-defined
- Always preserve links to source material
- Don't create items from early-stage brainstorming unless requested
- Use proper markdown formatting
- Include code references as: `path/to/file.ext:linenum`
- Ask for clarification rather than guessing status
- Always use links for external URLs
- Remember - you must get a "Problem to solve"!

## Comment Quality Guidelines

When creating comments, focus on extracting the **most valuable information** for a human reader:

- **Key insights over summaries**: What's the "aha" moment or critical understanding?
- **Decisions and tradeoffs**: What approach was chosen and what it enables/prevents
- **Blockers resolved**: What was preventing progress and how it was addressed
- **State changes**: What's different now and what it means for next steps
- **Surprises or discoveries**: Unexpected findings that affect the work

Avoid:

- Mechanical lists of changes without context
- Restating what's obvious from code diffs
- Generic summaries that don't add value

Remember: The goal is to help a future reader (including yourself) quickly understand what matters about this update.
