# Manual Installation (Without Plugin Manager)

This guide covers installing Superpowers Optimized manually without using Claude Code's plugin manager.

## Why Manual Installation?

- Plugin manager unavailable in your environment
- Need specific version pinned to a commit
- Offline installation
- Custom fork with local modifications

## Prerequisites

- Claude Code installed
- Git (for cloning the repository)
- File system access to Claude Code's config directory

## Find Your Claude Code Config Directory

Your config directory location depends on your OS:

| OS | Config Path |
|---|---|
| Windows | `%USERPROFILE%\.claude` |

## Installation Steps

### 1. Determine Target Directory

Inside your `.claude` folder, create the plugins directory if it doesn't exist:

```
.claude/
└── plugins/
```

### 2. Clone the Repository

Clone into a folder named after the plugin:

```powershell
# Windows (PowerShell)
git clone https://github.com/REPOZY/superpowers-optimized.git "%USERPROFILE%\.claude\plugins"
```

### 3. Required Files

The plugin requires these files to function:

```
plugins/superpowers-optimized/
├── .claude-plugin/
│   ├── plugin.json       # Plugin manifest (version: 6.6.1)
│   └── marketplace.json  # Marketplace metadata
├── hooks/
│   ├── hooks.json        # Hook registry
│   ├── context-engine.js
│   ├── session-start.js
│   ├── skill-activator.js
│   ├── track-edits.js
│   ├── track-session-stats.js
│   ├── stop-reminders.js
│   ├── block-dangerous-commands.js
│   ├── protect-secrets.js
│   ├── bash-compress-hook.js
│   ├── subagent-guard.js
│   └── safety/
│       └── ...           # Safety modules
├── skills/               # 24 skill directories
│   └── ...
├── agents/               # Custom agents (code-reviewer, red-team)
│   └── ...
├── lib/
│   └── skills-core.js   # Shared library
└── plugin.universal.yaml # Source of truth for config generation
```

**Do NOT copy these items from the repo:**

- `.git/` folder (optional, saves space)
- `node_modules/` (not needed)
- `tests/` folder (not needed)
- `media/` folder (not needed)
- `docs/` folder (not needed)
- Root-level files: README.md, LICENSE, VERSION, RELEASE-NOTES.md

### 4. Register the Plugin

After cloning, you may need to restart Claude Code or reload plugins. The plugin should be automatically detected.

If needed, manually reference the plugin by creating a symlink or referencing the path in your Claude settings.

## Updating Manually

To update to a newer version:

```powershell
# Navigate to the plugin directory
cd "%USERPROFILE%\.claude\plugins\superpowers-optimized"

# Pull the latest changes
git pull

# Or pin to a specific version/commit
git checkout v6.6.1
```

## Uninstalling

Simply remove the plugin directory:

```powershell
# Windows (PowerShell)
Remove-Item -Recurse -Force "%USERPROFILE%\.claude\plugins\superpowers-optimized"
```

## Troubleshooting

### Plugin Not Loading

1. Verify the directory structure matches the requirements above
2. Check that `.claude-plugin/plugin.json` exists and is valid JSON
3. Restart Claude Code
4. Check Claude Code logs for errors

### Hooks Not Running

- Verify `hooks/hooks.json` exists and references the correct JS files
- Ensure the `hooks/` folder contains all required `.js` files
- Check that hooks have execute permissions

### Skills Not Activating

- Confirm `skills/` directory contains all 24 skill folders
- Each skill folder must have a `SKILL.md` file
- Verify `lib/skills-core.js` is present

## Quick Install Script

Create a script to automate the process:

```powershell
# install-superpowers.ps1

$PLUGIN_DIR = "$env:USERPROFILE\.claude\plugins\superpowers-optimized"

git clone --depth 1 https://github.com/REPOZY/superpowers-optimized.git "$PLUGIN_DIR"

# Remove non-essential files to save space
Set-Location "$PLUGIN_DIR"
Remove-Item -Recurse -Force .git, tests, media, docs -ErrorAction SilentlyContinue

Write-Host "Installation complete. Restart Claude Code."
```

Run with: `.\install-superpowers.ps1`
