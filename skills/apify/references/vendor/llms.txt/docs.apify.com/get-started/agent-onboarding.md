---
title: Apify for AI agents
url: https://docs.apify.com/get-started/agent-onboarding.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Get started](https://docs.apify.com/get-started.md)
previous: [Build Actors](https://docs.apify.com/get-started/build-actors.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Apify for AI agents

Connect your AI agent or application to Apify - the platform for web scraping, data extraction, and browser automation. The typical agent workflow: find an Actor, run it, get structured data back.

## Core concepts

* *Actors* - Serverless cloud programs that perform scraping, crawling, or automation tasks. Thousands of ready-made Actors are available in [Apify Store](https://apify.com/store).
* *Datasets* - Append-only storage for structured results. Every Actor run creates a default dataset. Export as JSON, CSV, Excel, XML, or RSS.
* *API* - RESTful API at `https://api.apify.com/v2` for all platform operations. Also accessible via [MCP](https://docs.apify.com/integrations/mcp.md), [CLI](https://docs.apify.com/cli), and client libraries.
* *MCP connectors* - When you build an Actor that needs to act on a user's third-party accounts (Notion, Slack, GitHub, and others), use [MCP connectors](https://docs.apify.com/integrations/mcp-connectors.md) to receive connector IDs as input instead of asking users for raw credentials.

## Prerequisites

Sign up to [Apify Console](https://console.apify.com/sign-up). The free plan includes monthly platform usage credits with no credit card required. Get your API token from **[Console > Settings > Integrations](https://console.apify.com/settings/integrations)**.

Allow Apify in Claude Code on the web

If you're using Claude Code on the web, your cloud sessions run inside a network-restricted sandbox. By default, the sandbox blocks outbound requests to `*.apify.com`, making documentation, the API, and the MCP server unreachable.

To grant access, add `*.apify.com` to a cloud environment's allowed domains:

1. Open the environment selector and select **Add environment**.
2. Set **Network access** to **Custom**.
3. Check **Also include default list of common package managers** so existing functionality stays intact.
4. Add `*.apify.com` to the **Allowed domains** list.

Alternatively, set **Network access** to **Full** to allow any domain.

For more details, see [Claude Code on the web: Network access](https://code.claude.com/docs/en/claude-code-on-the-web#network-access).

Codex Cloud uses a similar default-deny sandbox. See [Codex Cloud internet access](https://developers.openai.com/codex/cloud/internet-access) for the equivalent configuration.

## Run your first Actor

Every Apify Actor follows the same pattern: send input as JSON, get structured data back. The shortest path through each of the main integration methods, using the agent-optimized [RAG Web Browser](https://apify.com/apify/rag-web-browser) Actor:

**MCP**

After connecting the MCP server to your AI assistant, ask:


```text
Use Apify's RAG Web Browser to find the top 3 pages about Apify documentation, then summarize.
```


Your agent calls [search-actors](https://docs.apify.com/integrations/mcp.md#available-tools), [call-actor](https://docs.apify.com/integrations/mcp.md#available-tools), and reads the resulting dataset items - all through MCP, no code required.

**JavaScript**


```typescript
import { ApifyClient } from 'apify-client';



const client = new ApifyClient({ token: process.env.APIFY_TOKEN });

const run = await client.actor('apify/rag-web-browser').call({

    query: 'Apify documentation',

    maxResults: 3,

});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
```


**Python**


```python
import os

from apify_client import ApifyClient



client = ApifyClient(token=os.environ['APIFY_TOKEN'])

run = client.actor('apify/rag-web-browser').call(

    run_input={'query': 'Apify documentation', 'maxResults': 3},

)

items = client.dataset(run['defaultDatasetId']).list_items().items
```


**CLI**


```bash
apify login                                       # one-time

apify call apify/rag-web-browser \

    -i '{"query": "Apify documentation", "maxResults": 3}' \

    --output-dataset
```


The pattern is the same across every integration method: pick an Actor, send input, receive structured data. Choose the connection method below that fits your stack.

Cost controls

When an agent calls Actors automatically, set run limits to prevent surprise bills. Pass these as query parameters on the [run Actor endpoint](https://docs.apify.com/api/v2/actors-runs-post.md):

* `memory` (MB) - power of 2, minimum 128. Lower memory means lower cost per second.
* `timeout` (seconds) - cap how long a single run can last.
* `maxTotalChargeUsd` - cap total run cost for pay-per-event Actors.

See [Usage and resources](https://docs.apify.com/actors/running/usage-and-resources.md) and [Billing](https://docs.apify.com/account/billing.md) for details.

## Choose your integration method

| Method     | Best for                                       | Auth               |
| ---------- | ---------------------------------------------- | ------------------ |
| MCP server | AI agents and coding assistants                | OAuth or API token |
| API client | Backend apps (JavaScript/Python)               | API token          |
| CLI        | Building and deploying custom Actors           | API token          |
| REST API   | Any language, HTTP integrations, no-code tools | API token          |

### MCP server

The [Apify MCP server](https://docs.apify.com/integrations/mcp.md) connects your agent to the full Apify platform via the [Model Context Protocol](https://modelcontextprotocol.io/). No local installation needed for remote-capable clients.

Free exploration

The MCP server's `search-actors`, `fetch-actor-details`, and docs tools work without authentication. You can browse Actors and documentation without an account.

#### Remote (recommended)

Works with Claude Code, Cursor, VS Code, GitHub Copilot, and other remote-capable clients.

1. Add the following to your MCP client's configuration:


   ```json
   {

     "mcpServers": {

       "apify": {

         "url": "https://mcp.apify.com"

       }

     }

   }
   ```


2. Restart your client and sign in when prompted. OAuth handles authentication automatically.

#### Local/stdio

For clients that only support local MCP servers, for example Claude Desktop.

1. Add the following to your MCP client's configuration:


   ```json
   {

     "mcpServers": {

       "apify": {

         "command": "npx",

         "args": ["-y", "@apify/actors-mcp-server"],

         "env": { "APIFY_TOKEN": "YOUR_TOKEN" }

       }

     }

   }
   ```


2. Replace `YOUR_TOKEN` with your API token and restart the client.

For client-specific setup instructions, use the [MCP Configurator](https://mcp.apify.com) which generates ready-to-paste configs. For details, see the [MCP server documentation](https://docs.apify.com/integrations/mcp.md).

### API client

For integrating Apify into your application code.

Package naming

`apify-client` is the API client for *calling* Actors. The `apify` package is the SDK for *building* Actors. For backend integration, install `apify-client`.

**JavaScript / TypeScript**


```bash
npm install apify-client
```



```typescript
import { ApifyClient } from 'apify-client';



const client = new ApifyClient({ token: process.env.APIFY_TOKEN });

const run = await client.actor('apify/web-scraper').call({

    startUrls: [{ url: 'https://example.com' }],

});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
```


Full reference: [JavaScript API client docs](https://docs.apify.com/api/client/js)

**Python**


```bash
pip install apify-client
```



```python
import os

from apify_client import ApifyClient



client = ApifyClient(token=os.environ['APIFY_TOKEN'])

run = client.actor('apify/web-scraper').call(

    run_input={'startUrls': [{'url': 'https://example.com'}]}

)

items = client.dataset(run['defaultDatasetId']).list_items().items
```


Full reference: [Python API client docs](https://docs.apify.com/api/client/python)

### CLI

For running Actors and building custom ones from the command line.

Install on macOS or Linux (Windows and Homebrew alternatives in the [CLI install docs](https://docs.apify.com/cli/docs/installation)):


```bash
curl -fsSL https://apify.com/install-cli.sh | bash

apify login                                      # authenticate with your API token
```


Discover and inspect Actors:


```bash
apify actors search scraping                     # search Apify Store

apify actors info apify/web-scraper --readme     # get Actor README

apify actors info apify/web-scraper --input      # get input schema
```


Run an Actor and get its output:


```bash
apify actors call apify/web-scraper \

    -i '{"startUrls": [{"url": "https://example.com"}]}' \

    --output-dataset
```


Build and deploy custom Actors:


```bash
apify create my-actor                            # scaffold (JS/TS/Python)

apify run                                        # test locally

apify push                                       # deploy to Apify cloud
```


Full reference: [Apify CLI documentation](https://docs.apify.com/cli).

### REST API

For HTTP-native integrations or languages without a dedicated client. Base URL: `https://api.apify.com/v2`. Authenticate with the `Authorization: Bearer YOUR_TOKEN` header.

#### Quick reference

| Action                                                                                                  | Method | Endpoint                                          |
| ------------------------------------------------------------------------------------------------------- | ------ | ------------------------------------------------- |
| [Search Actors in Store](https://docs.apify.com/api/v2/store-get.md)                                    | `GET`  | `/v2/store`                                       |
| [Get Actor details](https://docs.apify.com/api/v2/actor-get.md)                                         | `GET`  | `/v2/actors/{actorId}`                            |
| [Run an Actor](https://docs.apify.com/api/v2/actors-runs-post.md)                                       | `POST` | `/v2/actors/{actorId}/runs`                       |
| [Run Actor (sync, get results)](https://docs.apify.com/api/v2/actor-run-sync-get-dataset-items-post.md) | `POST` | `/v2/actors/{actorId}/run-sync-get-dataset-items` |
| [Get run status](https://docs.apify.com/api/v2/actor-run-get.md)                                        | `GET`  | `/v2/actor-runs/{runId}`                          |
| [Get dataset items](https://docs.apify.com/api/v2/dataset-items-get.md)                                 | `GET`  | `/v2/datasets/{datasetId}/items`                  |

The sync endpoint ([run-sync-get-dataset-items](https://docs.apify.com/api/v2/actor-run-sync-get-dataset-items-post.md)) runs an Actor and returns results in a single request (waits up to 5 minutes). Use [async endpoints](https://docs.apify.com/api/v2/actors-runs-post.md) for longer runs.

For runs that take longer than the sync timeout, prefer [webhooks](https://docs.apify.com/integrations/webhooks.md) over polling - Apify will POST a notification to your URL when the run finishes, avoiding wasted requests.

Full reference: [Apify API v2](https://docs.apify.com/api/v2.md).

## Agent Skills

Once you connect an agent via MCP or a coding assistant, [Apify Agent Skills](https://skills.sh/apify/agent-skills) add pre-built workflows on top - guiding the agent through multi-step scraping pipelines and Actor development tasks. Skills are not a separate integration method; they layer over your existing connection.

Install into Claude Code, Cursor, Gemini CLI, or OpenAI Codex:


```bash
npx skills add apify/agent-skills
```


| Skill                          | What it does                                                                  |
| ------------------------------ | ----------------------------------------------------------------------------- |
| `apify-ultimate-scraper`       | Routes web scraping requests to the right Actor for multi-step data pipelines |
| `apify-actor-development`      | Guided workflow for building and deploying custom Actors                      |
| `apify-actorization`           | Converts an existing project into an Apify Actor                              |
| `apify-generate-output-schema` | Auto-generates output schemas from Actor source code                          |
| `apify-sdk-integration`        | Integrates Actor execution into applications using the `apify-client` package |

For the full list and details, see the [skills registry](https://skills.sh/apify/agent-skills).

## Documentation access for agents

Apify documentation is available in formats optimized for programmatic consumption.

| Resource                | How to access                                                          |
| ----------------------- | ---------------------------------------------------------------------- |
| Specific doc page       | Append `.md` to any docs URL (for example, `docs.apify.com/actors.md`) |
| Specific doc page (alt) | Request with `Accept: text/markdown` header                            |
| Docs index              | [docs.apify.com/llms.txt](https://docs.apify.com/llms.txt)             |
| Full docs (large)       | [docs.apify.com/llms-full.txt](https://docs.apify.com/llms-full.txt)   |
| Actor Store pages       | Append `.md` to any Apify Store URL                                    |
| MCP docs tools          | `search-apify-docs`, `fetch-apify-docs`                                |

For targeted lookups, prefer `.md` URLs for specific pages or the MCP docs tools over the full `llms-full.txt` file. Agents with limited context windows may not load `llms-full.txt` fully.

## Useful resources

* [MCP server integration](https://docs.apify.com/integrations/mcp.md) - Tool customization, dynamic Actor discovery, and advanced configuration
* [CLI documentation](https://docs.apify.com/cli) - Complete command reference
* [API reference](https://docs.apify.com/api/v2.md) - All REST API endpoints
* [API client for JavaScript](https://docs.apify.com/api/client/js) | [for Python](https://docs.apify.com/api/client/python) - Client libraries
* [Storage documentation](https://docs.apify.com/storage.md) - Datasets, key-value stores, and request queues
* [Build with AI](https://docs.apify.com/actors/development.md) - Build and deploy your first Actor
* [Framework integrations](https://docs.apify.com/integrations/crewai.md) - CrewAI, LangChain, LlamaIndex, and more
