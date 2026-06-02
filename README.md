> All Playground skills will be published together with [WordPress Agent skills](https://github.com/WordPress/agent-skills/)

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

### Validation

Validation runs automatically via [CI](.github/workflows/validate-plugin.yml) on every push and pull request. A [pre-commit hook](.githooks/pre-commit) also runs `claude plugin validate .` and `gemini extensions validate .` locally. To enable the hook:

```
git config core.hooksPath .githooks
```

### Claude Code

**Official marketplace:** Submit the plugin at https://claude.ai/settings/plugins/submit (or https://platform.claude.com/plugins/submit for Console).

**Custom marketplace:** Push to GitHub. Users add the repo as a marketplace and install:

```
/plugin marketplace add WordPress/wp-playground
/plugin install wp-playground@wp-playground-local
```

### Codex CLI

The Codex plugin root is the `wp-playground/` subdirectory (with its own `.codex-plugin/plugin.json`, `skills/`, and `.mcp.json`).

**Official marketplace (via Apps SDK):** Self-serve plugin publishing is not yet available. The current path to the official Codex Plugin Directory is through the [OpenAI Apps SDK submission process](https://developers.openai.com/apps-sdk/deploy/submission):

1. Host the MCP server on a publicly accessible domain.
2. Complete organization verification in the [OpenAI Platform Dashboard](https://platform.openai.com/).
3. Submit for review from the dashboard with app name, description, privacy policy URL, MCP server details, screenshots, and test prompts.
4. Once approved, the plugin appears in both the ChatGPT Apps Directory and the Codex Plugin Directory.

> **Note:** Self-serve publishing is [coming soon](https://developers.openai.com/codex/plugins) according to OpenAI.

**Local / team distribution:** Push to GitHub. Users clone the repo and run Codex from the repo root — the `.agents/plugins/marketplace.json` makes the plugin auto-discoverable via `/plugins` in the TUI.

### Gemini CLI

Gemini extensions are installed directly from a public GitHub repo. Push to GitHub and users can install via:

```
gemini extensions install https://github.com/WordPress/wp-playground
```

The `gemini-extension.json` at the repo root is discovered automatically.

## Keeping skills up to date

A [weekly CI workflow](.github/workflows/sync-skills.yml) checks [WordPress/agent-skills](https://github.com/WordPress/agent-skills) for updates and opens a PR if files changed.

## Links

- [WordPress Playground docs](https://wordpress.github.io/wordpress-playground/)
- [Playground CLI](https://www.npmjs.com/package/@wp-playground/cli)
- [MCP server](https://www.npmjs.com/package/@wp-playground/mcp)
- [Agent skills source](https://github.com/WordPress/agent-skills)
