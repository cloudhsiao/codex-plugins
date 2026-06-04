# Codex Plugins

Git marketplace repo for local Codex plugin source folders.

The marketplace manifest is stored at:

```text
.agents/plugins/marketplace.json
```

## Installed Plugins

| Plugin | Purpose |
| --- | --- |
| `commit-commands` | Git workflow skills for commit, PR, and stale branch cleanup tasks. |

## Update Workflow

Install this marketplace from GitHub:

```bash
codex plugin marketplace add cloudhsiao/codex-plugins --ref main
codex plugin add commit-commands@codex-plugins
```

After editing a plugin source folder and pushing the change:

```bash
python3 /Users/bj/.codex/skills/.system/plugin-creator/scripts/update_plugin_cachebuster.py /Users/bj/Code/github.com/cloudhsiao/codex-plugins/<plugin-name>
codex plugin marketplace upgrade
codex plugin add <plugin-name>@codex-plugins
```

Start a new Codex thread after reinstalling so updated skills are loaded.
