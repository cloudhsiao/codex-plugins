# Code Simplifier

Codex plugin that ports Anthropic's Claude Code `code-simplifier` agent into a Codex skill.

## Skill

Use from the Codex `$` skill picker:

```text
$code-simplifier
```

| Skill | Purpose |
| --- | --- |
| `code-simplifier` | Simplify recently modified or explicitly selected code while preserving behavior. |

## Source Layout

```text
.codex-plugin/plugin.json
skills/code-simplifier/SKILL.md
LICENSE
```

## Upstream

Adapted from:

```text
https://github.com/anthropics/claude-plugins-official/tree/main/plugins/code-simplifier
```

The upstream Claude plugin is an agent definition. This Codex version translates that behavior into a skill workflow and replaces Claude-specific fields such as `model: opus` and `CLAUDE.md` references with Codex/repo instruction handling.
