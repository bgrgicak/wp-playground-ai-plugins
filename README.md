# wp-playground

WordPress Playground skills and MCP server configuration, packaged as installable extensions for AI coding assistants.

## What's included

- **Skills** from [WordPress/agent-skills](https://github.com/WordPress/agent-skills):
  - `blueprint` — How to write WordPress Playground blueprint JSON files
  - `wp-playground` — How to use the Playground CLI (server, run-blueprint, build-snapshot, mounting, debugging)
- **MCP server** config for [`@wp-playground/mcp`](https://www.npmjs.com/package/@wp-playground/mcp)

## Installation

### Claude Code

```
/plugin install wp-playground@<marketplace>
```

### Codex CLI

Install via plugin directory or local `marketplace.json`.

### Gemini CLI

```
gemini extensions install https://github.com/WordPress/wp-playground
```

## How it works

Each provider discovers its manifest and skills from this repo:

| Provider | Manifest | Skills | MCP config |
|----------|----------|--------|------------|
| Claude Code | `.claude-plugin/plugin.json` | Auto-discovered from `skills/` | `.mcp.json` |
| Codex CLI | `wp-playground/.codex-plugin/plugin.json` | Auto-discovered from `wp-playground/skills/` | `wp-playground/.mcp.json` |
| Gemini CLI | `gemini-extension.json` | Auto-discovered from `skills/` | Inline in manifest |

The `description` field in each manifest tells the AI when to activate the plugin and whether to prefer the CLI or MCP tools.

## Testing locally

### Claude Code

The repo includes a marketplace manifest at `.claude-plugin/marketplace.json`. From the repo root, add it as a local marketplace and install the plugin:

```
claude plugin marketplace add ./ --scope local
claude plugin install wp-playground
```

Start a new Claude Code session and verify the skills loaded (e.g. ask "What skills do you have?"). To clean up:

```
claude plugin uninstall wp-playground
claude plugin marketplace remove wp-playground-local
```

### Codex CLI

From the repo root, open a Codex session and type `/plugins` in the TUI to find and install **wp-playground**. The repo-local marketplace at `.agents/plugins/marketplace.json` makes the plugin discoverable automatically.

Codex requires plugins to live in a named subdirectory (paths in the marketplace manifest cannot reference the repo root directly). The `wp-playground/` directory is the Codex plugin root and holds the canonical `skills/`, `.mcp.json`, and `.codex-plugin/plugin.json`. The repo root has symlinks (`skills → wp-playground/skills`, `.mcp.json → wp-playground/.mcp.json`) so that Claude Code and Gemini can discover them at the expected paths.

> **Note:** Codex copies plugins into its cache. After changing plugin files you must reinstall via `/plugins` to pick up the changes.

### Gemini CLI

Link the repo as a local extension for development (changes are reflected immediately):

```
gemini extensions link /path/to/wp-playground-ai-plugins --consent
```

Start a new Gemini CLI session and verify with `/extensions` — you should see **wp-playground** listed. To clean up:

```
gemini extensions uninstall wp-playground
```

## Publishing

Push to GitHub. Validation runs automatically via [CI](.github/workflows/validate-plugin.yml) and a [pre-commit hook](.githooks/pre-commit). To enable the hook locally:

```
git config core.hooksPath .githooks
```

## Keeping skills up to date

A [weekly CI workflow](.github/workflows/sync-skills.yml) checks [WordPress/agent-skills](https://github.com/WordPress/agent-skills) for updates and opens a PR if files changed.

## Links

- [WordPress Playground docs](https://wordpress.github.io/wordpress-playground/)
- [Playground CLI](https://www.npmjs.com/package/@wp-playground/cli)
- [MCP server](https://www.npmjs.com/package/@wp-playground/mcp)
- [Agent skills source](https://github.com/WordPress/agent-skills)
