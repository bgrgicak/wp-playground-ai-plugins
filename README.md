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
| Codex CLI | `.codex-plugin/plugin.json` | Auto-discovered from `skills/` | `.mcp.json` |
| Gemini CLI | `gemini-extension.json` | Auto-discovered from `skills/` | Inline in manifest |

The `description` field in each manifest tells the AI when to activate the plugin and whether to prefer the CLI or MCP tools.

## Keeping skills up to date

A [weekly CI workflow](.github/workflows/sync-skills.yml) checks [WordPress/agent-skills](https://github.com/WordPress/agent-skills) for updates and opens a PR if files changed.

## Links

- [WordPress Playground docs](https://wordpress.github.io/wordpress-playground/)
- [Playground CLI](https://www.npmjs.com/package/@wp-playground/cli)
- [MCP server](https://www.npmjs.com/package/@wp-playground/mcp)
- [Agent skills source](https://github.com/WordPress/agent-skills)
