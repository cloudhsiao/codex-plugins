---
name: commit
description: Use when the user wants Codex to create a git commit for current repository changes, including requests like commit my changes, create a commit, or git commit this work.
---

# Commit Changes

When this skill is used:

1. Inspect `git status --short --branch`, `git diff HEAD`, the current branch, and recent commit messages.
2. Respect repo instructions for branch naming, tests, and commit conventions.
3. Check the change theme against the current branch prefix before committing when repo instructions require it.
4. Avoid committing secrets such as `.env`, credentials, keys, or unrelated generated artifacts.
5. Stage only relevant files.
6. Create one concise commit matching the repo style.
7. Report the commit hash and final git status.
