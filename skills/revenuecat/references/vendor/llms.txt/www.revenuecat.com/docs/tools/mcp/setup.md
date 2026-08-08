---
id: "tools/mcp/setup"
title: "RevenueCat MCP Server Setup"
description: "Authentication overview"
permalink: "/docs/tools/mcp/setup"
slug: "setup"
version: "current"
original_source: "docs/tools/mcp/setup.mdx"
---

> **AI agents:** This is the Markdown version of a RevenueCat documentation page. For the complete documentation index, see [llms.txt](https://www.revenuecat.com/docs/llms.txt).

## Authentication overview

RevenueCat MCP Server offers two authentication methods depending on your client:

- **OAuth authentication**: Available for Claude, Codex, Copilot, Cursor, VS Code, and several other clients — provides seamless authentication without managing API keys
- **API v2 secret key**: Supported by all MCP clients — requires a RevenueCat API v2 secret key

Choose the method that works best for your client setup.

## Cloud MCP server setup

:::info\[AI Toolkit]
The recommended way to get started with the MCP server is to use the [RevenueCat AI Toolkit](https://www.revenuecat.com/docs/tools/ai-toolkit). Follow the instructions below for a manual setup.
:::

### Using with Claude Code

Add the server via the `claude` CLI command:

```bash
claude mcp add --transport http revenuecat https://mcp.revenuecat.ai/mcp
```

### Using with OpenAI Codex

The Codex app, CLI, and IDE Extension share MCP settings. If you've already configured MCP servers in one, they're automatically adopted by the others.

You can add the MCP server via the Codex app:

1. Open **Settings** → **Plugins**, select **MCPs**, then choose **Add server**.
2. Give it a name, choose **Streamable HTTP**, enter the URL `https://mcp.revenuecat.ai/mcp`, and click **Save**.

To add the MCP server via the Codex CLI:

```bash
codex mcp add revenuecat --url https://mcp.revenuecat.ai/mcp
```

Or add the MCP server manually to your Codex configuration file (`~/.codex/config.toml`):

**macOS/Linux:**

```toml
[mcp_servers.revenuecat]
command = "npx"
args = ["mcp-remote", "https://mcp.revenuecat.ai/mcp", "--header", "Authorization: Bearer ${AUTH_TOKEN}"]
env = { AUTH_TOKEN = "YOUR_API_V2_SECRET_KEY" }
type = "stdio"
startup_timeout_ms = 20_000
```

**Windows:**

```toml
[mcp_servers.revenuecat]
command = 'C:\Program Files\nodejs\npx.cmd'
args = ["mcp-remote", "https://mcp.revenuecat.ai/mcp", "--header", "Authorization: Bearer ${AUTH_TOKEN}"]
env = {
    APPDATA = 'C:\Users\USERNAME\AppData\Roaming',
    LOCALAPPDATA = 'C:\Users\USERNAME\AppData\Local',
    HOME = 'C:\Users\USERNAME',
    SystemRoot = 'C:\Windows',
    AUTH_TOKEN = "YOUR_API_V2_SECRET_KEY"
}
type = "stdio"
startup_timeout_ms = 20_000
```

:::info\[Windows]
On Windows, replace `USERNAME` with your actual Windows username.
:::

### Using with Cursor

You can add the MCP server to Cursor by clicking the button below:

**Add to Cursor:** Install the `revenuecat` MCP server in Cursor.

Or you can also add it manually by adding the following to your `mcp.json` file:

```json
{
    "servers": {
        "revenuecat": {
            "url": "https://mcp.revenuecat.ai/mcp",
            "headers": {
                "Authorization": "Bearer {your API v2 token}"
            }
        }
    }
}
```

### Using with VS Code Copilot

Add to your Visual Studio Code `mcp.json`:

```json
{
	"servers": {
		"revenuecat-mcp": {
			"url": "https://mcp.revenuecat.ai/mcp",
			"type": "http"
		}
	},
	"inputs": []
}
```

### Using with Claude Web and Desktop

Use Anthropic's **custom connectors** with a **remote MCP** server. Full product steps and security notes are in [Get started with custom connectors using remote MCP](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp).

**Remote MCP server URL:** `https://mcp.revenuecat.ai/mcp`

#### Pro and Max (individual)

1. Open **Settings** → **Connectors**.
2. In the Connectors section, choose **Add custom connector**.
3. Enter the URL above.
4. Choose **Add**, then **Connect** and finish authentication when prompted.

#### Team and Enterprise

1. An **Owner** (or Primary Owner) adds the connector under **Organization settings** → **Connectors** → **Add custom connector** using the URL above.
2. Each member opens **Settings** → **Connectors**, finds the custom connector (labeled **Custom**), and chooses **Connect** to authenticate.

:::note

Custom remote MCP connectors are in beta on Claude; free plans can use one custom connector.

:::

### Using with MCP Inspector

For testing and development:

```bash
npx @modelcontextprotocol/inspector@latest
```

Configure the inspector with:

- **Transport Type**: Streamable HTTP
- **URL**: `https://mcp.revenuecat.ai/mcp`
- **Authentication**: Bearer Token with your RevenueCat API v2 secret key

## Authentication methods

RevenueCat Cloud MCP Server supports two authentication methods:

### OAuth authentication (recommended)

OAuth provides a seamless authentication experience: log in to your RevenueCat account and grant access to the MCP server, with no API keys to manage. We support OAuth authentication for an ever-growing list of clients. If your client does not support OAuth with RevenueCat, you can still use the API v2 secret key authentication method.

### API v2 secret key authentication

All MCP clients support API v2 secret key authentication. This method requires manually providing your RevenueCat API v2 secret key in the client configuration.

#### Getting your API key

1. Open your [RevenueCat dashboard](https://app.revenuecat.com/).
2. Navigate to your project's [**API Keys** page](https://www.revenuecat.com/docs/projects/authentication#finding-api-keys).
3. **Create a new API v2 secret key** and copy it.

:::info\[API key management]
Create a dedicated API key for the MCP server to keep your credentials organized.
:::

:::warning\[Permissions]

- Use a **write-enabled key** if you plan to create/modify resources
- A **read-only key** works if you only need to view data
  :::
