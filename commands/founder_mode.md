---
description: Create backlog.md item and PR for experimental features after implementation
---

you're working on an experimental feature that didn't get the proper ticketing and pr stuff set up.

assuming you just made a commit, here are the next steps:

1. get the sha of the commit you just made (if you didn't make one, read `.claude/commands/commit.md` and make one)

2. think deeply about what you just implemented, then add an item to ./backlog.md about what you just did, and put it in 'in dev' state - it should have ### headers for "problem to solve" and "proposed solution"
3. use the item id as the branch name
4. git checkout main
5. git checkout -b '[item-id]'
6. git cherry-pick 'COMMITHASH'
7. git push -u origin '[item-id]'
8. gh pr create --fill
9. read '.claude/commands/describe_pr.md' and follow the instructions
