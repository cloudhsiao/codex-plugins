---
name: clean-gone
description: Use when the user wants Codex to clean local git branches whose upstream remote branches are marked gone.
---

# Clean Gone Branches

When this skill is used:

1. If an `origin` remote exists, run `git fetch --prune origin` to refresh remote-tracking branch state. If it fails, report the failure and continue using existing local branch metadata.
2. Inspect the current branch with `git branch --show-current`.
3. Inspect worktrees with `git worktree list`.
4. Find gone branch candidates with this command:

   ```bash
   git for-each-ref refs/heads --format='%(refname:short)%09%(upstream:short)%09%(upstream:track)'
   ```

   Treat each output line as three tab-separated fields:

   - local branch: `%(refname:short)`
   - upstream branch: `%(upstream:short)`
   - upstream tracking status: `%(upstream:track)`

   The source of truth for cleanup candidates is the upstream tracking status field. Only branches whose tracking status field is exactly `[gone]` are gone candidates.
5. You may inspect `git branch -vv` as auxiliary debug output, but do not use it as the primary gone-branch detector. Some git wrappers simplify or rewrite `git branch -vv` output and may hide upstream tracking fields.
6. Do not delete the current branch. If the current branch is marked `[gone]`, clearly report that:

   - the branch is gone
   - it is currently checked out and cannot be deleted
   - the user needs to switch to another branch, such as `main`, before cleanup can delete it

   Do not automatically switch branches unless the user explicitly requested that behavior and the repository rules allow it.
7. Do not delete branches whose upstream tracking status field is not exactly `[gone]`, including branches with no upstream.
8. If a gone branch is used by a worktree, do not delete the branch directly. Remove the associated worktree only when that is clearly part of the requested cleanup and safe for the repository; otherwise report the branch as skipped because it is worktree-bound.
9. Delete gone local branches that are neither current nor worktree-bound.
10. In the final report, include:

    - removed branches
    - removed worktrees
    - skipped current branch
    - skipped worktree-bound branches
    - no cleanup needed, when no branches or worktrees were removed
