---
title: Claude Code CLI integration
url: https://docs.apify.com/integrations/claude-code-cli.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
  - [Claude](https://docs.apify.com/integrations/claude.md)
previous: [Claude](https://docs.apify.com/integrations/claude.md)
next: [Claude Desktop](https://docs.apify.com/integrations/claude-desktop.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Claude Code CLI integration

[Claude Code CLI](https://code.claude.com/docs/en/overview) is Anthropic's agentic coding tool that runs in your terminal. It reads and edits your codebase, runs commands, and completes multi-step development tasks.

The [Apify plugin for Claude Code](https://github.com/apify/apify-claude-code-plugin) connects Claude Code to Apify's library of [Actors](https://apify.com/store) and bundles:

* The [Apify MCP server](https://docs.apify.com/integrations/mcp.md) for searching the Store, running Actors, and retrieving datasets through the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/docs/getting-started/intro).
* An `apify` routing agent that picks the right tool or skill from a natural-language request.
* Five built-in skills for common workflows (see Bundled skills below).

This guide covers installation in the Claude Code CLI. Support for the Claude Code app is still in development.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Prerequisites

* [An Apify account](https://console.apify.com/sign-up) - sign up for free if you don't have one.
* [Claude Code CLI](https://code.claude.com/docs/en/overview) - installed and authenticated locally.

## Install the plugin

1. In Claude Code, run `/plugins` to open the plugin manager.

2. Open the **Marketplaces** tab and select **+ Add Marketplace**.

   ![Plugins Marketplaces tab with + Add Marketplace at the top of the list](/assets/images/02-marketplaces-tab-02a777d23d6cc923bc8691d222259371.webp)

3. Paste the Apify plugin repository URL and press Enter:


   ```text
   https://github.com/apify/apify-claude-code-plugin
   ```


4. Open the **Discover** tab. The `apify` plugin appears under **Install Plugins**. Press Enter to view its details.

5. Review the plugin details, choose an install scope (**Install for you (user scope)** is the typical choice), and press Enter.

6. Run `/reload-plugins` to activate the plugin in the current session.

7. Open the **Installed** tab to confirm the `apify` plugin is listed as enabled.

## Authenticate to Apify

The plugin bundles the Apify MCP server. Read-only tools like searching the Store and fetching Actor details work without signing in, but you need to authenticate to run Actors and access your account data.

1. Run `/mcp` to open the MCP server manager.

2. Find **plugin:apify<!-- -->:apify** in the list (currently **disabled**) and press Enter to open it.

   ![MCP server list with plugin:apify shown as disabled](/assets/images/09-mcp-needs-auth-f1d86aec8a3563b38467906bc1930d8a.webp)

3. Select **Enable**. Claude Code enables the server and returns you to the `/mcp` list.

   ![plugin:apify MCP server detail with Enable and Authenticate options](/assets/images/10-mcp-enable-6acabbaea9eca53f7972f4b09eb88592.webp)

4. Find **plugin:apify<!-- -->:apify** again and press Enter to reopen it, then select **Authenticate**. Claude Code opens a browser tab for the Apify OAuth flow.

   ![plugin:apify MCP server detail with the Authenticate option](/assets/images/11-mcp-authenticate-8c888737150efaf1639e2d8f045cec4f.webp)

5. Review the permissions and click **Allow access**.

   Dynamic registration warning

   The OAuth page shows a warning that the application was registered dynamically and wasn't verified by Apify. This is expected for the current plugin release - the plugin uses dynamic OAuth client registration. Make sure you trust this installation before allowing access.

6. Back in the terminal, you'll see `Authentication successful. Connected to plugin:apify:apify`.

   ![Terminal showing successful authentication to plugin:apify](/assets/images/13-auth-success-879e2f1cf8a4250fb7ab5a5e72a168c1.webp)

Session persistence

The connection stays authenticated for future sessions. You can revoke access at any time in [Apify Console > Settings > Integrations](https://console.apify.com/settings/integrations).

## Run your first prompt

Describe what you want in natural language. The `apify` agent routes the request to the right tool or skill, so you don't need to name tools yourself.

> "Use Apify to find a good Actor for scraping Google Maps places. Show me the best option, its input requirements, pricing model, and what kind of dataset output it returns. Do not run the Actor yet."

The agent searches Apify Store, fetches the top Actor's details through `plugin:apify:apify`, and summarizes its inputs, pricing, and output - all without running the Actor.

![Claude Code session calling plugin:apify and returning Google Maps Actor details](/assets/images/14-example-prompt-7c7863e126b6092b183c6077b8e009e1.webp)

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

> "Find 10 highly rated coffee shops in Seattle with name, address, rating, phone, and website."

*Actor development:*

> "Create an Apify Actor that accepts a `startUrl` and `maxPages` input, crawls the site, and stores each page title and URL."

*SDK integration:*

> "Add Apify to this project. The Node.js API route should run an Actor and return dataset items as JSON."

## Troubleshooting

### The `apify` plugin is disabled

Run `/plugins`, open the **Installed** tab, select the `apify` plugin, and choose **Enable plugin**. If the action reads **Disable plugin** instead, the plugin is already enabled - the MCP server may need authentication; see Authenticate to Apify.

![apify plugin detail in the Installed tab with the enable/disable actions](/assets/images/08-installed-detail-2c9e617f1bc74028b54525d5fb374aa3.webp)

### The `/plugins` command isn't available

Plugins require a local installation of the Claude Code CLI. They aren't available in remote or web sessions (claude.ai/code). Install or update the Claude Code CLI locally.

### Browser doesn't open, or OAuth fails

If the browser doesn't open automatically, copy the OAuth URL shown in the terminal and paste it into your browser manually.

If you're running Claude Code in a headless environment (SSH, remote container) or the OAuth flow still fails, authenticate with an API token instead. Copy your token from [Apify Console > Settings > Integrations](https://console.apify.com/settings/integrations) and set it before starting Claude Code:


```bash
export APIFY_TOKEN=<YOUR_API_TOKEN>
```


## Limitations

* Long-running Actors may exceed the time a single tool call waits for completion. Reduce the scope or split the work across multiple prompts.
* Each Actor run consumes Apify platform usage from your plan in addition to any Claude usage. See [Billing](https://docs.apify.com/account/billing.md) for details.
* Skills that edit files in your project (Actor development, actorization, SDK integration) make local changes - review them before deploying or committing.

## Related integrations

* [MCP server integration](https://docs.apify.com/integrations/mcp.md) - Use the Apify MCP server with other clients
* [ChatGPT integration](https://docs.apify.com/integrations/chatgpt.md) - Connect the Apify MCP server to ChatGPT

## Resources

* [Apify plugin for Claude Code](https://github.com/apify/apify-claude-code-plugin) - Source repository and full README with advanced setup notes (Apify CLI install, all auth paths, available MCP tools)
* [Claude Code documentation](https://code.claude.com/docs/en/overview) - Official Claude Code docs
* [Apify Store](https://apify.com/store) - Browse Actors you can run from Claude Code
