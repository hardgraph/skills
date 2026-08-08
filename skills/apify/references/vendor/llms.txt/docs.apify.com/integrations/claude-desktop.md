---
title: Claude Desktop integration
url: https://docs.apify.com/integrations/claude-desktop.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
  - [Claude](https://docs.apify.com/integrations/claude.md)
previous: [Claude Code CLI](https://docs.apify.com/integrations/claude-code-cli.md)
next: [CrewAI](https://docs.apify.com/integrations/crewai.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Claude Desktop integration

Connect [Claude Desktop](https://claude.ai/download) to the [Apify MCP server](https://docs.apify.com/integrations/mcp.md) to give your conversations access to thousands of Actors from [Apify Store](https://apify.com/store). Once connected, Claude can search for, run, and retrieve results from Actors directly in your chat.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Prerequisites

* An [Apify account](https://console.apify.com/sign-up)
* [Claude Desktop](https://claude.ai/download) installed

## Connect to Apify

Choose one of the following methods:

* Remote server - recommended, automatic updates, OAuth support, no local dependencies
* One-click installation - install from the connector directory

### Remote server (recommended)

The remote server at `https://mcp.apify.com` is the recommended way to connect. Key advantages:

* Automatic updates - always runs the latest version of the Apify MCP server
* OAuth authentication - secure sign-in through your browser, no API token needed
* No local dependencies - nothing to install or maintain on your machine

To set up the remote server, [add a custom connector](https://support.claude.com/en/articles/11175166) in Claude Desktop and use `https://mcp.apify.com` as the server URL.

On first connection, your browser opens to sign in to Apify and authorize the connection.

### One-click installation

Search for "Apify" in the [Claude Desktop connector directory](https://support.claude.com/en/articles/11175166) and install the connector.

Alternatively, download and open the [Apify MCP server .mcpb file](https://github.com/apify/actors-mcp-server/releases/latest/download/apify-mcp-server.mcpb) to register the connector automatically.

## Verify the connection

1. Restart Claude Desktop after saving configuration changes.
2. Open a new conversation.
3. Check that Apify tools are available in the tools list.
4. Test with a prompt like: "Search for web scraping Actors on Apify."

## Troubleshooting

If the steps below don't resolve your issue, [submit a GitHub issue](https://github.com/apify/apify-mcp-server/issues) or contact [Apify support](https://apify.com/contact).

Quick checks

Before diving into specific issues, check these two things first:

1. *Check the Claude Desktop logs.* Look for files with `mcp` in the name.
2. *Check tool permissions.* The connector may block some tools by default.

#### Tools fail to load

The MCP server shows as connected but Apify tools don't appear in the tools list, or Claude doesn't recognize any Apify tools in conversation.

* *Check tool permissions.* Individual connector tools can be blocked in your [connector settings](https://support.anthropic.com/en/articles/11175166-how-to-manage-and-remove-integrations-in-claude). Verify that Apify tools are set to **Always allow** or **Ask first**, not blocked.
* *Check the connector version.* Claude Desktop may silently downgrade the connector to an older version. If tools aren't appearing despite the connector showing as enabled, remove and re-add the connector to trigger an update.
* *Restart Claude Desktop.* Configuration changes only take effect after a restart.
* *Reinstall the connector.* Remove the Apify connector and add it again.

#### Authentication errors

Authentication errors occur when the MCP server can't verify your identity. You may see "Unauthorized" or "Invalid token" messages, or Actor runs may fail silently.

* *Re-authorize the connection.* Remove and re-add the Apify connector in Claude Desktop to trigger a new OAuth flow.

#### Claude Desktop logs

Check the Claude Desktop logs for MCP-related errors:

* macOS: `~/Library/Logs/Claude/`
* Linux: `~/.config/Claude/logs/`
* Windows: `%APPDATA%\Claude\logs\`

Look for files with `mcp` in the name for server-specific error messages.

### One-click installation issues

The following troubleshooting steps apply specifically to the one-click installation method. The remote server runs entirely on Apify's infrastructure, so there are no local logs or configuration to debug. If you experience issues with the remote server, contact [Apify support](https://apify.com/contact).

#### "Unable to connect to extension server" error

This is the most common issue. It typically appears when installing from the Claude Desktop connector directory. In some cases, the MCP server starts and communicates correctly, but Claude Desktop still shows the error.

1. *Consider switching to the remote server setup.* The remote server is the most reliable option.
2. *Uninstall and reinstall the extension.* In Claude Desktop, disable the Apify extension, remove it, then add it again.
3. *Clear the npx cache.* A stale cache can cause connection failures. Follow the steps in Corrupted npx cache.
4. *Check the Claude Desktop logs* for specific error messages.
5. *Check your network.* Ensure your firewall or VPN is not blocking the connection.
6. *Still not working?* [Submit a GitHub issue](https://github.com/apify/apify-mcp-server/issues) or contact [Apify support](https://apify.com/contact).

#### Corrupted npx cache

A stale or corrupted npx cache can prevent the server from starting. Clear the cache and retry:

**macOS and Linux**


```bash
rm -rf ~/.npm/_npx
```


**Windows**


```text
rmdir /s /q %LOCALAPPDATA%\npm-cache\_npx
```


After clearing the cache:

1. Restart Claude Desktop to re-download the server package.
2. Check the Claude Desktop logs for errors.
3. If the issue persists, switch to the remote server setup, which doesn't rely on local packages.

#### Connector does not work in Claude Cowork

The Apify connector shows as connected in Cowork, but Claude doesn't recognize the tools or can't call them. The same connector works fine in regular Claude Chat.

Cowork launches local MCP servers with a different mechanism than Claude Chat and requires a system-wide `node` binary. If Node.js is only installed through a version manager (such as nvm, fnm, nodenv, or Volta), Cowork cannot find it and the server fails to start. The Claude Desktop logs show `spawn node ENOENT` in this case.

Install Node.js system-wide and restart Claude Desktop:

**macOS**


```bash
brew install node
```


Or download the installer from [nodejs.org](https://nodejs.org).

**Windows**

Download and run the installer from [nodejs.org](https://nodejs.org).

**Linux**


```bash
# Debian/Ubuntu

sudo apt install nodejs



# Fedora

sudo dnf install nodejs
```


A system-wide installation can coexist with an existing version-manager installation. If installing Node.js system-wide is not an option, switch to the remote server setup, which doesn't require a local Node.js runtime.

## Known limitations

* Claude Desktop may silently downgrade an installed connector to an older version (for example, from 0.9.14 back to 0.9.6). This can cause tools to stop loading even though the connector still shows as enabled. Removing and re-adding the connector may prompt an update, but doesn't always resolve the issue.
* Some Claude Desktop versions have inconsistent behavior with remote MCP server connections. Update to the latest version if you experience issues.
* If the connector directory installation fails, use the remote server at `https://mcp.apify.com` instead.

## Next steps

* [Apify MCP server](https://docs.apify.com/integrations/mcp.md) - Explore tool selection, available tools, telemetry, and rate limits
* [Apify MCP server configurator](https://mcp.apify.com) - Select tools visually and copy configuration
* [Apify MCP server on GitHub](https://github.com/apify/apify-mcp-server) - Report bugs and suggest features
