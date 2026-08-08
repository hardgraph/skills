---
title: 🔺 Vercel AI SDK integration
url: https://docs.apify.com/integrations/vercel-ai-sdk.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
previous: [Upsonic](https://docs.apify.com/integrations/upsonic.md)
next: [x402](https://docs.apify.com/integrations/x402.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# 🔺 Vercel AI SDK integration

[Vercel AI SDK](https://ai-sdk.dev/) is a TypeScript toolkit designed to help developers build AI-powered applications and agents with React, Next.js, Vue, Svelte, Node.js, and more. For more details, check out the [Vercel AI SDK documentation](https://ai-sdk.dev/docs/introduction).

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## How to use Apify with Vercel AI SDK

Apify is a marketplace of ready-to-use web scraping and automation tools, AI agents, and MCP servers that you can equip your own AI with. This guide demonstrates how to use Apify tools with a simple AI agent built with Vercel AI SDK.

### Prerequisites

* *Apify API token*: You need an Apify API token set as the `APIFY_TOKEN` environment variable. To obtain your token check [Apify documentation](https://docs.apify.com/integrations/api).

* *Node.js packages*: Install the following Node.js packages:


  ```bash
  npm install @modelcontextprotocol/sdk @openrouter/ai-sdk-provider ai
  ```


### Build a simple pub search AI agent using Apify Google Maps scraper

First, import all required packages:


```typescript
import { experimental_createMCPClient as createMCPClient, generateText, stepCountIs } from 'ai';

import { StreamableHTTPClientTransport } from '@modelcontextprotocol/sdk/client/streamableHttp.js';

import { createOpenRouter } from '@openrouter/ai-sdk-provider';
```


Connect to the Apify MCP server and get all available tools for the AI agent. You can use the [UI configurator](https://mcp.apify.com/) to select your tools visually and generate the configuration code below:


```typescript
// Connect to the Apify MCP server and get the available tools

const url = new URL('https://mcp.apify.com');

const mcpClient = await createMCPClient({

  transport: new StreamableHTTPClientTransport(url, {

      requestInit: {

        headers: {

            "Authorization": `Bearer ${process.env.APIFY_TOKEN}`

        }

      }

  }),

});

const tools = await mcpClient.tools();

console.log('Tools available:', Object.keys(tools).join(', '));
```


Create Apify OpenRouter LLM provider so we can run the AI agent:

Single token

By using the [Apify OpenRouter](https://apify.com/apify/openrouter) you don't need to provide a separate API key for OpenRouter or any other LLM provider. Only your Apify token is needed. All token costs go to your Apify account.


```typescript
// Configure the Apify OpenRouter LLM provider

const openrouter = createOpenRouter({

    baseURL: 'https://openrouter.apify.actor/api/v1',

    apiKey: 'api-key-not-required',

    headers: {

        "Authorization": `Bearer ${process.env.APIFY_TOKEN}`

    }

});
```


Run the AI agent with the Apify Google Maps scraper tool to find a pub near the Ferry Building in San Francisco:


```typescript
// Run the AI agent and generate a response

const response = await generateText({

    model: openrouter('x-ai/grok-4-fast'),

    tools,

    stopWhen: stepCountIs(5),

    messages: [

        {

            role: 'user',

            content: [{ type: 'text', text: 'Find a pub near the Ferry Building in San Francisco using the Google Maps scraper.' }],

        },

    ],

});

console.log('Response:', response.text);

console.log('\nDone!');

await mcpClient.close();
```


## Resources

* [Apify Actors](https://docs.apify.com/actors)
* [Vercel AI SDK documentation](https://ai-sdk.dev/docs/introduction)
* [What are AI agents?](https://blog.apify.com/what-are-ai-agents/)
* [Apify MCP server](https://mcp.apify.com)
* [Apify MCP server documentation](https://docs.apify.com/integrations/mcp)
* [Apify OpenRouter proxy](https://apify.com/apify/openrouter)
