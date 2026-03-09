---
name: setup-bito
description: Guide the user through configuring BitoAIArchitect MCP credentials
---

# Setup BitoAIArchitect

Help the user configure their BitoAIArchitect MCP server credentials.

## Steps

1. **Ask for credentials** — You need three values from the user:
   - **Workspace ID**: Their Bito workspace identifier (found at https://app.bito.ai under workspace settings)
   - **Bearer Token**: Their MCP authentication token (found in Bito workspace under API/MCP settings)
   - **Email**: The email address associated with their Bito account

2. **Validate the endpoint** — Once you have all three values, test the connection by describing what a test call would look like:
   ```
   URL: https://mcp.bito.ai/{WORKSPACE_ID}/mcp
   Headers: Authorization: Bearer {TOKEN}, x-email-id: {EMAIL}
   ```

3. **Set environment variables** — Guide the user to add these to their shell profile (`~/.zshrc`, `~/.bashrc`, or equivalent):
   ```bash
   export BITO_WORKSPACE_ID="<their-workspace-id>"
   export BITO_MCP_TOKEN="<their-token>"
   export BITO_EMAIL="<their-email>"
   ```

4. **Verify plugin activation** — After setting env vars and restarting Claude Code:
   - Run `/mcp` to check the BitoAIArchitect server appears
   - Try a simple test: ask to list repositories using `listRepositories`

## Troubleshooting

- If the MCP server shows as disconnected, check that env vars are exported (not just set)
- If auth fails, verify the token hasn't expired
- If workspace ID is wrong, the URL will return a 404
- Remind users to restart Claude Code after changing environment variables
