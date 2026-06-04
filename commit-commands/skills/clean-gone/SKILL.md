---
name: clean-gone
description: Use when the user wants Codex to clean local git branches whose upstream remote branches are marked gone.
---

# Clean Gone Branches

When this skill is used:

1. Inspect `git branch -v` and `git worktree list`.
2. Do not delete the current branch.
3. Do not delete branches unless Git reports them as `[gone]`.
4. Remove associated worktrees for gone branches when needed.
5. Delete the gone local branches.
6. Report which branches and worktrees were removed.
7. If no gone branches exist, report that no cleanup was needed.
