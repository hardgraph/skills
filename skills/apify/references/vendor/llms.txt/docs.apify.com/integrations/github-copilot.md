---
title: GitHub Copilot integration
url: https://docs.apify.com/integrations/github-copilot.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
previous: [Flowise](https://docs.apify.com/integrations/flowise.md)
next: [Google ADK](https://docs.apify.com/integrations/google-adk.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# GitHub Copilot integration

[GitHub Copilot](https://github.com/features/copilot) is GitHub's AI coding assistant. In VS Code, its agent mode reads and edits your workspace, runs commands, and completes multi-step development tasks.

The [Apify plugin for GitHub Copilot](https://github.com/apify/apify-github-copilot-plugin) connects Copilot to Apify's library of [Actors](https://apify.com/store) and bundles:

* The [Apify MCP server](https://docs.apify.com/integrations/mcp.md) for searching Apify Store, running Actors, and retrieving datasets through the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/docs/getting-started/intro).
* An `apify` routing agent that picks the right tool or skill from a natural-language request.
* Five built-in skills for common workflows (see Bundled skills below).

This guide covers setup in VS Code.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Prerequisites

* [An Apify account](https://console.apify.com/sign-up) - sign up for free if you don't have one.
* [VS Code](https://code.visualstudio.com/) version 1.120 or newer, with the [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) extension installed and signed in with Copilot access.

## Install the plugin

Install the plugin directly from its GitHub repository - there's no need to clone it.

1. Copy the plugin repository URL:


   ```text
   https://github.com/apify/apify-github-copilot-plugin
   ```


2. In VS Code, open the Command Palette (`Ctrl+Shift+P`, or `Cmd+Shift+P` on macOS) and run **Chat: Install Plugin from Source**.

   ![VS Code Command Palette with the Chat: Install Plugin from Source command selected](/assets/images/install-plugin-command-cbb74b31637b8de6d703281bcb413085.webp)

3. Paste the repository URL into the input field and press Enter.

   ![VS Code input field with the Apify plugin repository URL pasted in](/assets/images/install-plugin-url-722055f27ab230ceec1ce33f7b4415cf.webp)

4. In the trust dialog, review the source and select **Trust**.

   ![VS Code dialog asking whether to trust plugins from the Apify repository](/assets/images/install-plugin-trust-47cc66a8b17468230d20507ba5732985.webp)

5. VS Code installs the plugin and opens the **Plugin: apify** panel, which shows **Disable** and **Uninstall** actions. You can reopen it anytime from the Command Palette (`Ctrl+Shift+P`, or `Cmd+Shift+P` on macOS) with **Chat: Plugins**, which lists your installed agent plugins.

   ![VS Code Plugin: apify panel open over the Agent Plugins list, showing the installed Apify plugin](/assets/images/plugin-installed-5c40118a96e76d444b45ec9c71c0c620.webp)

6. Open Copilot Chat, open the mode picker, and select the **apify** agent.

   ![Copilot Chat mode picker with the apify agent selected](/assets/images/agent-picker-20e5cef29e4d68bd4d46a63a02a32fc7.webp)

## Connect the Apify MCP server

The plugin bundles the Apify MCP server (`https://mcp.apify.com/`) - its configuration ships in the plugin's `.mcp.json`, so you don't add it manually. Read-only tools like searching Apify Store and fetching Actor details work without signing in, but you need to authenticate to run Actors and access your account data.

1. Open the tools picker in Copilot Chat and confirm that **Apify MCP Server** is selected for the `apify` agent.

   ![Configure Tools dialog with Apify MCP Server enabled for the apify agent](/assets/images/configure-tools-5eb1e906890b3708ffa9e5209a036f9c.webp)

2. Open the plugin's `.mcp.json`. A **Start** action appears above the `apify-mcp-server` entry - select it to start the server.

   ![Bundled .mcp.json with the Start action above the apify-mcp-server entry](/assets/images/mcp-start-7aa05accd3185c5db367701f514909e5.webp)

3. VS Code prompts that the MCP server wants to authenticate to `console-backend.apify.com`. Select **Allow**, complete the Apify OAuth flow in your browser, and choose the account to connect.

   ![VS Code dialog asking to allow the apify-mcp-server to authenticate to console-backend.apify.com](data:image/webp;base64,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)

4. The `apify-mcp-server` entry shows **Running** and exposes its tools.

   ![Bundled .mcp.json showing the apify-mcp-server as Running with Stop and Restart actions](/assets/images/mcp-running-1ff7ab0c4687a79ef294c9d6fc002ae9.webp)

Session persistence

The connection stays authenticated for future sessions. You can revoke access at any time in [Apify Console > Settings > Integrations](https://console.apify.com/settings/integrations).

## Run your first prompt

Select the **apify** agent and describe what you want in natural language. The agent routes the request to the right tool or skill, so you don't need to name tools yourself.

> Use Apify to find a good Actor for scraping Google Maps places. Show me the best option, its input requirements, pricing model, and what kind of dataset output it returns. Do not run the Actor yet.

The agent searches Apify Store, fetches the top Actor's details through the Apify MCP server, and summarizes its inputs, pricing, and output - all without running the Actor.

To check what's available, ask the agent to list its Apify tools.

![Copilot Chat listing the available Apify MCP tools](/assets/images/verify-tools-6ae26e50bb21b6fdecd8808a9934f488.webp)

## Bundled skills

| Skill                          | Description                                                                                              |
| ------------------------------ | -------------------------------------------------------------------------------------------------------- |
| `apify-ultimate-scraper`       | CLI-driven extraction using existing Actors for multi-step scraping and lead-generation workflows.       |
| `apify-actor-development`      | Full Actor lifecycle - template selection, development, local testing, and deployment with `apify push`. |
| `apify-actorization`           | Converts existing JavaScript, TypeScript, Python, or CLI projects into Apify Actors.                     |
| `apify-generate-output-schema` | Generates dataset and key-value store schemas for existing Actors.                                       |
| `apify-sdk-integration`        | Integrates Actor execution into applications using the `apify-client` package.                           |

Example prompts that route to specific skills:

*Ultimate scraper:*

> Find 10 highly rated coffee shops in Seattle with name, address, rating, phone, and website.

*Actor development:*

> Create an Apify Actor that accepts a `startUrl` and `maxPages` input, crawls the site, and stores each page title and URL.

*SDK integration:*

> Add Apify to this project. The Node.js API route should run an Actor and return dataset items as JSON.

## Authentication paths

The `apify` agent uses the transport that fits the task, and each one authenticates differently:

* For MCP tools (search, run, retrieve data), authenticate with OAuth through the browser, as described in Connect the Apify MCP server. No token setup is needed.
* For the Apify CLI (building Actors, actorization, CLI fallback), run `apify login` once, or set `APIFY_TOKEN` in headless environments. Get your token from [Apify Console > Settings > Integrations](https://console.apify.com/settings/integrations).
* For SDK integration with `apify-client`, set the `APIFY_TOKEN` environment variable in your application's environment.

## Troubleshooting

### The `apify` agent doesn't appear in the picker

Confirm that the plugin is installed and trusted (rerun **Chat: Install Plugin from Source** if needed), that **Chat: Plugins** is enabled in Settings, and that you reloaded the window after installing. Plugin support requires VS Code 1.120 or newer.

### The Apify MCP server won't start

Open the plugin's `.mcp.json` and select the **Start** action above the `apify-mcp-server` entry, then complete the **Allow** prompt and account selection. Check the **Output** panel (**MCP: apify-mcp-server**) - a successful start ends with a line about the discovered tools. Confirm the **Apify MCP Server** tool is enabled for the `apify` agent in the tools picker.

### Browser doesn't open, or OAuth fails

If the browser doesn't open automatically, copy the OAuth URL from the VS Code dialog and paste it into your browser manually.

If you're running VS Code over SSH, in a dev container, or in any environment without a browser, the MCP OAuth flow can't complete. Authenticate locally first so the connection is stored, or use the CLI and SDK paths instead - run `apify login`, or set `APIFY_TOKEN`:


```bash
export APIFY_TOKEN=<YOUR_API_TOKEN>
```


### The agent picks the wrong skill or transport

Start from the **apify** agent. It is the single entry point that detects the available transport and routes each request to the correct tool or skill.

## Limitations

* Copilot plugin support is a preview feature in VS Code, so its settings and behavior may change between releases.
* The plugin is installed from its GitHub repository URL; it isn't published to a plugin marketplace yet.
* Long-running Actors may exceed the time a single tool call waits for completion. Reduce the scope or split the work across multiple prompts.
* Each Actor run consumes usage on the Apify platform from your plan in addition to any Copilot usage. See the [Apify billing documentation](https://docs.apify.com/account/billing.md) for details.
* Skills that edit files in your project (Actor development, actorization, SDK integration) make local changes - review them before deploying or committing.

## Related integrations

* [Claude Code CLI integration](https://docs.apify.com/integrations/claude-code-cli.md) - Install the Apify plugin in the Claude Code CLI
* [MCP server integration](https://docs.apify.com/integrations/mcp.md) - Use the Apify MCP server with other clients

## Resources

* [Apify plugin for GitHub Copilot](https://github.com/apify/apify-github-copilot-plugin) - Source repository and full README with advanced setup notes
* [GitHub Copilot documentation](https://docs.github.com/en/copilot) - Official GitHub Copilot docs
* [Apify MCP server documentation](https://docs.apify.com/integrations/mcp.md) - Connect the Apify MCP server to other clients
* [Apify Store](https://apify.com/store) - Browse Actors you can run from Copilot
