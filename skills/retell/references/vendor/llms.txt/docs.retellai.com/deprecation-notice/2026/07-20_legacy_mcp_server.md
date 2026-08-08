> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Legacy MCP server (retell.stlmcp.com) removed (07/20/2026)

> The legacy hosted MCP server at retell.stlmcp.com shuts down on 07/20/2026. Migrate your MCP client to mcp.retellai.com — auth is unchanged.

## The legacy hosted MCP server is being removed

**Removal date:** 07/20/2026

The hosted MCP server at `retell.stlmcp.com` will be shut down. Use the current
Retell MCP server at `mcp.retellai.com` instead — see the
[Retell MCP server](/get-started/mcp-server) guide for setup.

## Migration

* **Change the MCP server URL** from `https://retell.stlmcp.com` to `https://mcp.retellai.com`.
* **Auth is unchanged:** send your Retell API key as a `Bearer` token.
* **LLM clients (Claude, Cursor, etc.):** no code changes needed — they discover the available tools automatically.
* The local `npx @retell-ai/mcp-server` package is not affected.
