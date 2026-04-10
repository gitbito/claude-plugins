# BitoAIArchitect Plugin for Claude Code

A Claude Code plugin that installs the **BitoAIArchitect MCP server** — giving Claude cross-repository intelligence to search code, explore dependencies, analyze architecture, and discover patterns across all your organization's repositories.

## Quick Start

### 1. Install the Plugin

```
/plugin marketplace add gitbito/claude-plugins
/plugin install bito-ai-architect@bito-claude-plugins
```

### 2. Set Environment Variables

Add these to your shell profile (`~/.zshrc`, `~/.bashrc`, etc.):

```bash
export BITO_WORKSPACE_ID="your-workspace-id"
export BITO_MCP_TOKEN="your-bearer-token"
export BITO_EMAIL="your-email@company.com"
```

> **Where to find these:** Log in to [app.bito.ai](https://app.bito.ai) → Workspace Settings → API/MCP section.

### 3. Restart Claude Code

Restart Claude Code to pick up the new environment variables and activate the plugin.

### 4. Verify

- Run `/mcp` — you should see `BitoAIArchitect` listed with a green status
- Run `/bito-setup` if you need guided setup help
- Try: *"List all repositories in my organization"*

## What's Included

| Component | Path | Description |
|-----------|------|-------------|
| Plugin manifest | `.claude-plugin/plugin.json` | Plugin metadata + MCP server config |
| Guidelines | `CLAUDE.md` | Auto-loaded instructions teaching Claude when/how to use BitoAIArchitect tools |
| Setup skill | `skills/bito-setup/SKILL.md` | Interactive credential setup guide |
| Setup command | `commands/bito-setup.md` | `/bito-setup` slash command |
| Commit review | `skills/commit-review/SKILL.md` | Pre-commit review with cross-repo impact analysis |
| Feature plan | `skills/feature-plan/SKILL.md` | Complex feature planning with cross-repo context |
| PRD | `skills/prd/SKILL.md` | Product Requirements Document generation |
| TRD | `skills/trd/SKILL.md` | Technical Requirements Document generation |
| Production triage | `skills/production-triage/SKILL.md` | Production incident diagnosis and triage |
| Codebase explorer | `skills/codebase-explorer/SKILL.md` | Explore and understand codebase architecture |
| Epic to plan | `skills/epic-to-plan/SKILL.md` | Convert epics or PRDs into sprint-ready plans |
| Feasibility | `skills/feasibility/SKILL.md` | Go/no-go feasibility and impact analysis |
| Spike | `skills/spike/SKILL.md` | Structured technical investigation |
| Scope to plan | `skills/scope-to-plan/SKILL.md` | Convert any work item into a sprint-ready plan |
| MCP config | `.mcp.json` | Standalone MCP config (can be copied to other projects) |
| Hooks | `hooks/hooks.json` | Placeholder for future event hooks |
| Settings | `settings.json` | Documents required env vars and defaults |

## Available MCP Tools

Once connected, Claude gets access to these tools:

| Tool | Description |
|------|-------------|
| `listRepositories` | Browse all repos in your organization |
| `searchRepositories` | Find repos by technology, functionality, or keywords |
| `getRepositoryInfo` | Get detailed repo metadata, structure, and dependencies |
| `searchCode` | Search code across repos using zoekt index |
| `searchSymbols` | Find function/class/method definitions |
| `getCode` | Retrieve actual source code from files |
| `listClusters` | View groups of related repositories |
| `getClusterInfo` | Examine a specific cluster's architecture |
| `queryFieldAcrossRepositories` | Compare data across multiple repos |
| `searchWithinRepository` | Search metadata within a single repo |
| `getRepositorySchema` | Discover repo data structure |
| `getFieldPath` | Extract specific nested fields efficiently |

## Extending the Plugin

### Adding Hooks

Edit `hooks/hooks.json` to add event-driven automation:

```json
{
  "hooks": {
    "pre-commit": {
      "command": "node",
      "args": ["./hooks/pre-commit-check.js"]
    }
  }
}
```

### Adding Skills

Create a new directory under `skills/` with a `SKILL.md`:

```
skills/
└── my-new-skill/
    └── SKILL.md
```

### Adding Commands

Drop a markdown file in `commands/`:

```
commands/
└── my-command.md    →  available as /my-command
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| MCP server disconnected | Check env vars are **exported** (not just set). Restart Claude Code. |
| Auth errors (401/403) | Verify `BITO_MCP_TOKEN` hasn't expired. Regenerate in Bito workspace. |
| Workspace not found (404) | Double-check `BITO_WORKSPACE_ID` value. |
| Tools not appearing | Run `/mcp` to check server status. Run `/plugins` to verify plugin is enabled. |

## License

Internal use — Bito, Inc.
