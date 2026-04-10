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

Run the interactive setup command in Claude Code:

```
/bito-ai-architect:bito-setup
```

This will guide you through configuring your workspace ID, bearer token, and email. Alternatively, you can set the environment variables manually:

```bash
export BITO_WORKSPACE_ID="your-workspace-id"
export BITO_MCP_TOKEN="your-bearer-token"
export BITO_EMAIL="your-email@company.com"
```

Restart Claude Code, then run `/mcp` to verify the server is connected.

## Available Commands

Once installed, the following slash commands are available in Claude Code:

| Command | Description |
|---------|-------------|
| `/bito-ai-architect:bito-setup` | Interactive setup to configure credentials |
| `/bito-ai-architect:codebase-explorer` | Deep codebase exploration across repositories |
| `/bito-ai-architect:feature-plan` | Build a detailed implementation plan for a complex feature |
| `/bito-ai-architect:prd` | Write a Product Requirements Document grounded in real system context |
| `/bito-ai-architect:trd` | Produce a Technical Requirements Document by analyzing existing architecture |
| `/bito-ai-architect:production-triage` | Diagnose production incidents with cross-repo context and blast radius mapping |
| `/bito-ai-architect:epic-to-plan` | Convert an approved epic or PRD into a complete, sprint-ready implementation plan |
| `/bito-ai-architect:feasibility` | Produce a go/no-go feasibility and impact analysis before committing to implementation |
| `/bito-ai-architect:scope-to-plan` | Convert any approved work item into a complete, sprint-ready implementation plan |
| `/bito-ai-architect:spike` | Conduct a structured technical investigation when the team doesn't know enough to plan |
| `/bito-ai-architect:commit-review` | Pre-commit code review analyzing staged changes for issues and cross-repo impact |

## Feature Requests

To request new features or skills, send an email to support@bito.ai.
