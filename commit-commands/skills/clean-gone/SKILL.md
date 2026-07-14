---
name: clean-gone
description: Use when the user wants Codex to clean local git branches whose upstream remote branches are marked gone.
---

# Clean Gone Branches

Clean stale local branches conservatively. A remote branch being deleted is not, by itself, permission to discard local worktree contents or force-delete local commits.

## Refresh and detect candidates

1. If an `origin` remote exists, run `git fetch --prune origin` before inspecting branches.
   - If fetch or prune fails, report the error and stop all cleanup. Do not use existing `[gone]` metadata after a failed refresh because it may be stale.
2. Inspect the current branch with `git branch --show-current`.
3. Inspect worktrees with `git worktree list --porcelain`. Use the porcelain records to map branch names to worktree paths; do not parse paths with `grep` or `awk`.
4. Find gone branch candidates with:

   ```bash
   git for-each-ref refs/heads --format='%(refname:short)%09%(upstream:short)%09%(upstream:track)'
   ```

   Treat each output line as three tab-separated fields:

   - local branch: `%(refname:short)`
   - upstream branch: `%(upstream:short)`
   - upstream tracking status: `%(upstream:track)`

   The source of truth is the upstream tracking status field. Only a field exactly equal to `[gone]` is a cleanup candidate. Do not use `git branch -v`, `grep '\[gone\]'`, commit subjects, or branch display text as the primary detector. `git branch -v` does not show upstream status reliably; `-vv` is still display text rather than the structured source of truth.
5. Do not delete branches whose tracking status is not exactly `[gone]`, including branches with no upstream.

## Current gone branch

Never delete the currently checked-out branch directly.

If the current branch is a gone candidate:

1. Check the current worktree with:

   ```bash
   git status --porcelain=v1 --untracked-files=all --ignored
   ```

   Any output means the worktree is dirty, including ignored files. Report the branch and stop. Do not stash, discard, or switch branches automatically.
2. Confirm that local `main` exists and is not already checked out in another worktree. If either check fails, report the reason and stop.
3. Use the `userask` tool with these choices:
   - `切換到 main 並清理`
   - `不動作`

   The default choice must be `不動作`.
4. If the user chooses `不動作`, stop the entire run. Do not clean other branches or worktrees.
5. If the user chooses `切換到 main 並清理`, run `git switch main`. If switching fails, report the error and stop. Only after the switch succeeds may the remaining candidates be processed.
6. After switching to `main`, run:

   ```bash
   git merge --ff-only origin/main
   ```

   If the fast-forward fails, report the error and stop all cleanup. Do not use a stale local `main` as the deletion baseline, and do not create a merge commit automatically. Only after the fast-forward succeeds may the remaining candidates be processed.

## Worktree and branch cleanup

Process each gone candidate independently so one failure does not prevent reporting or processing other candidates.

1. If the candidate is attached to a worktree, check that worktree with:

   ```bash
   git -C <worktree-path> status --porcelain=v1 --untracked-files=all --ignored
   ```

   Any output means the worktree is dirty, including ignored files. Skip it, do not remove it, and do not delete its branch.
2. If the attached worktree is completely clean, remove it with `git worktree remove <worktree-path>` without `--force`. If removal fails, report the failure and do not delete the branch.
3. Delete the local branch with `git branch -d <branch>`. If safe deletion fails, retain the branch, report the failure, and continue with other candidates. Do not use `-D` unless the user explicitly requests force deletion.
4. A worktree can have been removed while its branch remains when step 3 fails. Report both results separately.

## Final report

Include:

- removed branches
- removed worktrees
- skipped current branch, including whether the user chose `不動作`
- skipped dirty worktree-bound branches
- failed worktree removals
- failed safe branch deletions
- fetch/prune, branch-switch, or `main` fast-forward failures
- no cleanup needed when no branch or worktree was removed
