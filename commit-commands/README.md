# Commit Commands

Local Codex plugin that provides git workflow skills.

## Skills

Use these from the Codex `$` skill picker:

```text
$commit
$commit-push-pr
$clean-gone
```

| Skill | Purpose |
| --- | --- |
| `commit` | Stage relevant changes and create one git commit. |
| `commit-push-pr` | Commit changes, push the branch, and create a GitHub PR. |
| `clean-gone` | Remove local branches whose upstream branches are gone. |

## Source Layout

```text
.codex-plugin/plugin.json
skills/commit/SKILL.md
skills/commit-push-pr/SKILL.md
skills/clean-gone/SKILL.md
```

## Marketplace

This plugin is installed from the `codex-plugins` marketplace:

```text
/Users/bj/Code/github.com/cloudhsiao/codex-plugins/.agents/plugins/marketplace.json
```

The marketplace entry points to:

```text
./commit-commands
```

## Update Workflow

After editing this plugin:

```bash
python3 /Users/bj/.codex/skills/.system/plugin-creator/scripts/update_plugin_cachebuster.py /Users/bj/Code/github.com/cloudhsiao/codex-plugins/commit-commands
codex plugin marketplace upgrade
codex plugin add commit-commands@codex-plugins
```

Start a new Codex thread after reinstalling so updated skills are loaded.
