---
name: commit-push-pr
description: Use when the user wants Codex to commit current changes, push the branch, and create a pull request with gh pr create.
---

# Commit, Push, And Open PR

When this skill is used:

1. Inspect `git status --short --branch`, `git diff HEAD`, and the current branch.
2. If on `main` or `master`, create an appropriately named feature branch before committing.
3. Respect repo instructions for branch naming, tests, commit conventions, push rules, and PR target branch.
4. Avoid committing secrets such as `.env`, credentials, keys, or unrelated generated artifacts.
5. Stage only relevant files and create one concise commit.
6. Push the branch to `origin` with upstream tracking.
7. Create a PR using `gh pr create`; target `main` unless repo instructions say otherwise.
8. Include a concise PR summary and test plan.
9. Report the commit hash, branch, PR URL, and final git status.
