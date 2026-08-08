---
title: Cursor integration
url: https://docs.apify.com/integrations/cursor.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
previous: [CrewAI](https://docs.apify.com/integrations/crewai.md)
next: [Flowise](https://docs.apify.com/integrations/flowise.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Cursor integration

[Cursor](https://cursor.com) is an AI-powered code editor that understands your codebase, edits files, runs commands, and completes multi-step development tasks from natural-language prompts.

The [Apify plugin for Cursor](https://github.com/apify/apify-cursor-plugin) connects Cursor to Apify's library of [Actors](https://apify.com/store) and bundles:

* The [Apify MCP server](https://docs.apify.com/integrations/mcp.md) for searching Apify Store, running Actors, and retrieving datasets through the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/docs/getting-started/intro).
* An `apify` routing agent that picks the right tool or skill from a natural-language request.
* Five built-in skills for common workflows (see Bundled skills below).

This guide covers installation from the Cursor plugin marketplace.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Prerequisites

* [An Apify account](https://console.apify.com/sign-up) - sign up for free if you don't have one.
* [Cursor](https://cursor.com) - installed and signed in locally.

## Install the plugin

1. Open **Cursor** > **Preferences** > **Cursor Settings**.

2. Select **Plugins**.

   ![Cursor Settings with the Plugins section selected](/assets/images/01-plugins-589f7c81605678e03fec196a6a8087c2.webp)

3. Search for **Apify**.

4. Select the **Apify Cursor plugin** from the results.

   ![Cursor plugin marketplace search results with the Apify Cursor plugin card](data:image/webp;base64,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)

5. Click **Get**.

6. Choose an install scope. Select **This project** to enable the plugin only in the current project, or **All projects** to enable it for every project under your account.

## Authenticate to Apify

The plugin bundles the Apify MCP server. Read-only tools like searching Apify Store and fetching Actor details work without signing in, but you need to authenticate to run Actors and access your account data.

1. Open **Cursor** > **Preferences** > **Cursor Settings** and select **Tools & MCPs**.

2. Scroll to the bottom of the page. The **Apify MCP** server appears in the list.

   ![Tools \&amp; MCPs page showing the Apify MCP server at the bottom of the list](/assets/images/03-tools-and-mcps-614afc79f3e755175bc6a8d54ac912ad.webp)

3. Click **Connect**. Cursor opens a browser tab for the Apify OAuth flow.

4. Review the permissions and click **Allow access**.

5. Back in Cursor, the **Apify MCP** server shows as connected.

   ![Tools \&amp; MCPs page showing the Apify MCP server connected](/assets/images/04-mcp-connected-ab316b105d9d0451f4d7edc353c20e2e.webp)

Session persistence

The connection stays authenticated for future sessions. You can revoke access at any time in [Apify Console > Settings > Integrations](https://console.apify.com/settings/integrations).

## Run your first prompt

Describe what you want in natural language. The `apify` agent routes the request to the right tool or skill, so you don't need to name tools yourself.

> Use Apify to find a good Actor for scraping Google Maps places. Show me the best option, its input requirements, pricing model, and what kind of dataset output it returns. Do not run the Actor yet.

The agent searches Apify Store, fetches the top Actor's details through the Apify MCP server, and summarizes its inputs, pricing, and output - all without running the Actor.

![Cursor session calling the Apify MCP server and returning Google Maps Actor details](/assets/images/05-example-prompt-f3beca6eaf7d130dfb98d0e1b97fe50c.webp)

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

### The Apify MCP server stays disconnected

Open **Cursor** > **Preferences** > **Cursor Settings**, select **Tools & MCPs**, and toggle the **Apify MCP** server off and on. If it still doesn't connect, re-trigger the OAuth flow with **Connect**; see Authenticate to Apify.

### Cursor picks the wrong skill

Start your request with `@apify` so the routing agent handles it. The agent owns the guardrails that pick the right skill and avoid common traps, such as confusing the `apify` and `apify-client` packages.

### Browser doesn't open, or OAuth fails

If the browser doesn't open automatically, copy the OAuth URL shown by Cursor and paste it into your browser manually.

If you're running Cursor in a remote session, devcontainer, or over SSH where no browser is available, authenticate with an API token instead. Copy your token from [Apify Console > Settings > Integrations](https://console.apify.com/settings/integrations) and set it in your environment before starting Cursor:


```bash
export APIFY_TOKEN=<YOUR_API_TOKEN>
```


## Limitations

* Long-running Actors may exceed the time a single tool call waits for completion. Reduce the scope or split the work across multiple prompts.
* Each Actor run consumes Apify platform usage from your plan in addition to any Cursor usage. See [Billing](https://docs.apify.com/account/billing.md) for details.
* Skills that edit files in your project (Actor development, actorization, SDK integration) make local changes - review them before deploying or committing.

## Related integrations

* [MCP server integration](https://docs.apify.com/integrations/mcp.md) - Use the Apify MCP server with other clients
* [ChatGPT integration](https://docs.apify.com/integrations/chatgpt.md) - Connect the Apify MCP server to ChatGPT

## Resources

* [Apify plugin for Cursor](https://github.com/apify/apify-cursor-plugin) - Source repository and full README with advanced setup notes (Apify CLI install, all auth paths, available MCP tools)
* [Cursor documentation](https://docs.cursor.com) - Official Cursor docs
* [Apify Store](https://apify.com/store) - Browse Actors you can run from Cursor
