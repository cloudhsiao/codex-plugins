---
name: code-simplifier
description: Use when the user asks Codex to simplify, refine, clean up, or improve recently modified code for clarity, consistency, and maintainability while preserving behavior.
---

# Code Simplifier

This skill is adapted from Anthropic's `code-simplifier` Claude Code plugin.

When this skill is used:

1. Inspect the requested scope. If the user did not name files or functions, focus on code modified in the current session or visible in `git diff`.
2. Read applicable project instructions, including `AGENTS.md`, repo-specific style guides, and nearby code patterns.
3. Identify simplifications that improve readability, consistency, maintainability, or debugging without changing behavior.
4. Make only scoped edits that are directly tied to simplification. Do not broaden the task into unrelated refactors.
5. Preserve public APIs, outputs, side effects, error behavior, data formats, timing assumptions, and compatibility unless the user explicitly asks for behavior changes.
6. Prefer explicit, readable code over compact or clever code.
7. Reduce unnecessary nesting, duplication, dead abstractions, redundant comments, and avoidable indirection.
8. Keep useful abstractions when they clarify ownership, testability, or future maintenance.
9. Avoid nested ternary expressions, dense one-liners, and changes that make the code harder to step through or debug.
10. Verify the result with the narrowest relevant tests, build checks, type checks, or lint checks available. If verification cannot be run, state the reason.
11. Report only meaningful changes and verification results.

Do not use this skill to redesign architecture, add new features, remove required behavior, or rewrite broad areas of the codebase unless the user explicitly asks for that larger scope.
