# Bito Claude Plugins

Official Claude Code plugin marketplace by [Bito, Inc.](https://bito.ai)

## Available Plugins

| Plugin | Description |
|--------|-------------|
| **bito-ai-architect** | BitoAIArchitect MCP server for cross-repo intelligence — search code, explore dependencies, analyze architecture, and discover patterns across all your organization's repositories. |

## Installation

### Step 1: Add the marketplace

In Claude Code, run:

```
/plugin marketplace add gitbito/claude-plugins
```

### Step 2: Install a plugin

```
/plugin install bito-ai-architect@bito-claude-plugins
```

### Step 3: Configure credentials

After installing the BitoAIArchitect plugin, set your environment variables:

```bash
export BITO_WORKSPACE_ID="your-workspace-id"
export BITO_MCP_TOKEN="your-bearer-token"
export BITO_EMAIL="your-email@company.com"
```

Restart Claude Code, then run `/mcp` to verify the server is connected.

## For Plugin Developers

To add a new plugin to this marketplace, submit a PR adding your plugin entry to `.claude-plugin/marketplace.json`.
