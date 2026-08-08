---
title: Codex in the ChatGPT desktop app
url: https://docs.apify.com/integrations/codex-app.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
  - [OpenAI](https://docs.apify.com/integrations/openai.md)
previous: [ChatGPT](https://docs.apify.com/integrations/chatgpt.md)
next: [Codex CLI](https://docs.apify.com/integrations/codex-cli.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Codex in the ChatGPT desktop app

[Codex](https://developers.openai.com/codex/) is OpenAI's agentic coding tool. It reads and edits your codebase, runs commands, and completes multi-step development tasks.

The [Apify plugin for Codex](https://github.com/apify/apify-codex-plugin) connects Codex to Apify's library of [Actors](https://apify.com/store) and bundles:

* The [Apify MCP server](https://docs.apify.com/integrations/mcp.md) for searching Apify Store, running Actors, and retrieving datasets through the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/docs/getting-started/intro).
* Five built-in skills for common workflows (see Bundled skills below).

This guide covers installation in the ChatGPT desktop app, where you select **Codex** from the top-left menu. To use Codex in your terminal instead, see the [Codex CLI](https://docs.apify.com/integrations/codex-cli.md) guide.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Prerequisites

* [An Apify account](https://console.apify.com/sign-up) - sign up for free if you don't have one.
* [Codex](https://developers.openai.com/codex/) - installed.

## Install the plugin

1. In Codex, open the sidebar and select **Plugins**.

   ![Codex sidebar with the Plugins entry selected](data:image/webp;base64,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)

2. On the **Plugins** screen, select the dropdown next to **+** and choose **Add marketplace**.

   ![Plugins screen with the add menu open and Add marketplace available](/assets/images/01-add-marketplace-5c429ec3584d03931cb6c3c4698ff307.webp)

3. In the **Add plugin marketplace** dialog, enter the Apify plugin repository in the **Source** field:


   ```text
   apify/apify-codex-plugin
   ```


   ![Add plugin marketplace dialog with the Apify repository in the Source field](/assets/images/02-marketplace-form-b233a8ecbe609adad0b718c7f5977695.webp)

4. Select **Add marketplace**.

5. On the **Plugins** screen, open the **Personal** tab. The **Apify** plugin appears under **Apify Plugin**.

   ![Plugins screen Personal tab showing the Apify plugin card](/assets/images/03-add-plugin-6accbfd7f7035d8e0e59dab3c4be138c.webp)

6. Select **Add** next to **Apify**.

   ![Apify plugin card with the Add button and Install plugin tooltip](data:image/webp;base64,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)

7. In the dialog, select **Add to Codex**.

8. Select **Install Apify** to start the Apify MCP server setup.

## Authenticate to Apify

The plugin bundles the Apify MCP server. Read-only tools like searching Apify Store and fetching Actor details work without signing in, but you need to authenticate to run Actors and access your account data.

1. After you select **Install Apify**, Codex starts the Apify MCP server setup and opens a browser tab for the Apify OAuth flow.

2. Review the permissions and select **Allow access**.

3. Back in Codex, the `apify` MCP server connects and is ready to use.

Session persistence

The connection stays authenticated for future sessions. You can revoke access at any time in [Apify Console > Settings > Integrations](https://console.apify.com/settings/integrations).

## Run your first prompt

Describe what you want in natural language. Because this bundle exposes the MCP tools and skills directly, be explicit about the workflow you want.

> Use Apify to find a good Actor for scraping Google Maps places. Show me the best option, its input requirements, pricing model, and what kind of dataset output it returns. Do not run the Actor yet.

Codex searches Apify Store, fetches the top Actor's details through the `apify` MCP server, and summarizes its inputs, pricing, and output - all without running the Actor.

![Codex session calling the Apify MCP server and returning Google Maps Actor details](/assets/images/05-test-plugin-9f3285525a9e2d687ff734e850c7337d.webp)

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

### The Apify plugin does not appear in the list

Open the **Plugins** screen, switch to the **Personal** tab, and confirm the Apify marketplace was added. If the **Apify** plugin still doesn't appear, re-add the marketplace using the repository `apify/apify-codex-plugin`.

### The Plugins screen does not appear

Plugins require a local installation of Codex with plugin support enabled. Install or update Codex, then reopen the **Plugins** screen.

### Browser doesn't open, or OAuth fails

If the browser doesn't open automatically, copy the OAuth URL shown by Codex and paste it into your browser manually.

If you're running Codex in a headless environment (SSH, remote container) or the OAuth flow still fails, authenticate with an API token instead. Copy your token from [Apify Console > Settings > Integrations](https://console.apify.com/settings/integrations) and set it before starting Codex:


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
