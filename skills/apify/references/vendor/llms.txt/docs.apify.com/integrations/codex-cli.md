---
title: Codex CLI integration
url: https://docs.apify.com/integrations/codex-cli.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
  - [OpenAI](https://docs.apify.com/integrations/openai.md)
previous: [Codex (desktop app)](https://docs.apify.com/integrations/codex-app.md)
next: [OpenAI Agents SDK](https://docs.apify.com/integrations/openai-agents.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Codex CLI integration

[Codex](https://developers.openai.com/codex/) is OpenAI's agentic coding tool. The Codex CLI runs in your terminal, reads and edits your codebase, runs commands, and completes multi-step development tasks.

The [Apify plugin for Codex](https://github.com/apify/apify-codex-plugin) connects Codex to Apify's library of [Actors](https://apify.com/store) and bundles:

* The [Apify MCP server](https://docs.apify.com/integrations/mcp.md) for searching Apify Store, running Actors, and retrieving datasets through the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/docs/getting-started/intro).
* Five built-in skills for common workflows (see Bundled skills below).

This guide covers installation in the Codex CLI. To use Codex in the ChatGPT desktop app instead, see the [Codex in the ChatGPT desktop app](https://docs.apify.com/integrations/codex-app.md) guide.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Prerequisites

* [An Apify account](https://console.apify.com/sign-up) - sign up for free if you don't have one.
* [Codex CLI](https://developers.openai.com/codex/) - installed and authenticated locally.

## Install the plugin

1. In the Codex CLI, run the `/plugins` command.

   ![Codex CLI prompt with the /plugins command typed in](/assets/images/00-plugins-command-30dc5b703c5a858060c728348f1f83a9.webp)

2. Use the arrow keys to select the **Add Marketplace** tab, then press Enter.

   ![Codex CLI plugins menu with the Add Marketplace tab selected](/assets/images/01-add-marketplace-tab-91e9487d11adde946043d10f38e4d416.webp)

3. Type the Apify plugin repository and press Enter to confirm:


   ```text
   apify/apify-codex-plugin
   ```


   ![Codex CLI add marketplace prompt with the Apify repository entered](data:image/webp;base64,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)

4. Open the **Apify Plugin** tab, select **Apify**, and press Enter to view the plugin details.

   ![Codex CLI plugins menu showing the Apify plugin as available](/assets/images/03-plugin-detail-f0c85bf1aab97f9e010d3e6edf7d504b.webp)

5. Select **Install plugin** and press Enter to install.

   ![Codex CLI Apify plugin detail with the Install plugin option selected](/assets/images/04-plugin-install-a637f8f02f49be4a938a922aeea6697e.webp)

## Authenticate to Apify

The plugin bundles the Apify MCP server. Read-only tools like searching Apify Store and fetching Actor details work without signing in, but you need to authenticate to run Actors and access your account data.

1. The first time Codex calls a tool that requires authentication, such as running an Actor, it opens a browser tab for the Apify OAuth flow.

2. Review the permissions and select **Allow access**.

3. Back in the terminal, the `apify` MCP server is connected and ready to use in any new chat.

Session persistence

The connection stays authenticated for future sessions. You can revoke access at any time in [Apify Console > Settings > Integrations](https://console.apify.com/settings/integrations).

## Run your first prompt

In any new Codex CLI chat, describe what you want in natural language. Because this bundle exposes the MCP tools and skills directly, be explicit about the workflow you want.

> Use Apify to find a good Actor for scraping Google Maps places. Show me the best option, its input requirements, pricing model, and what kind of dataset output it returns. Do not run the Actor yet.

Codex searches Apify Store and fetches the top Actor's details through the `apify` MCP server.

![Codex CLI calling the Apify MCP server search-actors tool](/assets/images/05-test-prompt-input-813f403edb6b51ab187c7f1330ace9f5.webp)

It then summarizes the Actor's inputs, pricing, and output - all without running the Actor.

![Codex CLI response summarizing the Google Maps Actor inputs and pricing](/assets/images/06-test-prompt-output-f387e65ead30f71ee76a02d5498c3f95.webp)

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

## Troubleshooting

### The `/plugins` command does not appear

Plugins require a local installation of the Codex CLI with plugin support enabled. Install or update the Codex CLI locally, then run `/plugins` again.

### The Apify plugin does not appear in the list

Reopen `/plugins`, move to the **Apify Plugin** tab, and confirm the marketplace was added. If the **Apify** plugin still doesn't appear, re-add the marketplace using the repository `apify/apify-codex-plugin`.

### Browser doesn't open, or OAuth fails

If the browser doesn't open automatically, copy the OAuth URL shown in the terminal and paste it into your browser manually.

If you're running the Codex CLI in a headless environment (SSH, remote container) or the OAuth flow still fails, authenticate with an API token instead. Copy your token from [Apify Console > Settings > Integrations](https://console.apify.com/settings/integrations) and set it before starting Codex:


```bash
export APIFY_TOKEN=<YOUR_API_TOKEN>
```


## Limitations

* Long-running Actors may exceed the time a single tool call waits for completion. Reduce the scope or split the work across multiple prompts.
* Each Actor run consumes Apify platform usage from your plan in addition to any Codex usage. See [Billing](https://docs.apify.com/account/billing.md) for details.
* Skills that edit files in your project (Actor development, actorization, SDK integration) make local changes - review them before deploying or committing.

## Related integrations

* [MCP server integration](https://docs.apify.com/integrations/mcp.md) - Use the Apify MCP server with other clients
* [ChatGPT integration](https://docs.apify.com/integrations/chatgpt.md) - Connect the Apify MCP server to ChatGPT

## Resources

* [Apify plugin for Codex](https://github.com/apify/apify-codex-plugin) - Source repository and full README with advanced setup notes (Apify CLI install, all auth paths, available MCP tools)
* [Codex documentation](https://developers.openai.com/codex/) - Official Codex docs
* [Apify Store](https://apify.com/store) - Browse Actors you can run from Codex
