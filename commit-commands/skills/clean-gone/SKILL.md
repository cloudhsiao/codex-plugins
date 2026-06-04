---
name: clean-gone
description: Use when the user wants Codex to clean local git branches whose upstream remote branches are marked gone.
---

# Clean Gone Branches

When this skill is used:

1. If an `origin` remote exists, run `git fetch --prune origin` to refresh remote-tracking branch state. If it fails, report the failure and continue using existing local branch metadata.
2. Inspect `git branch -vv` and `git worktree list`.
3. Do not delete the current branch.
4. Do not delete branches unless Git reports them as `[gone]`.
5. Remove associated worktrees for gone branches when needed.
6. Delete the gone local branches.
7. Report which branches and worktrees were removed.
8. If no gone branches exist, report that no cleanup was needed.
