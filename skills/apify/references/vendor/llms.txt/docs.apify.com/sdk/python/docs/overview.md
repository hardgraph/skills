# Overview

Copy for LLM

The Apify SDK for Python is the official library for creating [Apify Actors](https://docs.apify.com/platform/actors) in Python. It provides everything you need to build an Actor and run it both locally and on the [Apify platform](https://docs.apify.com/platform). It handles the Actor lifecycle, [storage](https://docs.apify.com/platform/storage) access, platform events, [Apify Proxy](https://docs.apify.com/platform/proxy), pay-per-event charging, and more.

```
import asyncio



import httpx

from bs4 import BeautifulSoup



from apify import Actor





async def main() -> None:

    async with Actor:

        actor_input = await Actor.get_input() or {}

        url = actor_input.get('url', 'https://apify.com')

        async with httpx.AsyncClient() as client:

            response = await client.get(url)

        soup = BeautifulSoup(response.content, 'html.parser')

        data = {

            'url': url,

            'title': soup.title.string if soup.title else None,

        }

        await Actor.push_data(data)





if __name__ == '__main__':

    asyncio.run(main())
```

## What are Actors?[](#what-are-actors)

Actors are serverless programs that can do almost anything. From simple scripts and web scrapers to complex automation workflows, AI agents, or even always-on services that expose HTTP endpoints.

They can run either locally or on the Apify platform, where you can scale their execution, monitor runs, schedule tasks, integrate them with other services, or even publish and monetize them. If you're new to Apify, learn more about the platform in the [Apify documentation](https://docs.apify.com/platform/about).

For more context, read the [Actor whitepaper](https://whitepaper.actor/).

## Features[](#features)

* Run the full Actor lifecycle inside `async with Actor:`, covering init, exit, failures, status messages, rebooting, and metamorphing ([Actor lifecycle](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/actor-lifecycle.md)).
* Read Actor input validated against your input schema with `Actor.get_input()`, including automatic decryption of secret fields ([Actor input](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/actor-input.md)).
* Read and write datasets, key-value stores, and request queues, locally or on the platform ([Working with storages](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/storages.md)).
* React to platform events such as system info, migration, and abort, and persist state across migrations and restarts ([Actor events](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/actor-events.md)).
* Route requests through Apify Proxy with group selection, country targeting, and rotation, with session and tiered-proxy support ([Proxy management](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/proxy-management.md)).
* Start, call, and abort other Actors and tasks, and attach webhooks to run events ([Interacting with other Actors](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/interacting-with-other-actors.md), [Webhooks](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/webhooks.md)).
* Monetize your Actor with pay-per-event charging ([Pay-per-event](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/pay-per-event.md)).
* Reach the full [Apify API](https://docs.apify.com/api/v2) through a preconfigured `ApifyClient` ([Accessing the Apify API](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/access-apify-api.md)).

## What you can build[](#what-you-can-build)

Almost any Python project can become an Actor, including projects for:

* **Web scraping and crawling** - The SDK is fully compatible with [Crawlee](https://crawlee.dev/python), which makes Apify a natural place to deploy and scale your crawlers (see the [Crawlee guide](https://docs.apify.com/sdk/python/sdk/python/docs/guides/crawlee.md)). It also works with other popular scraping libraries, such as [Scrapy](https://docs.apify.com/sdk/python/sdk/python/docs/guides/scrapy.md), [Scrapling](https://docs.apify.com/sdk/python/sdk/python/docs/guides/scrapling.md), or [Crawl4AI](https://docs.apify.com/sdk/python/sdk/python/docs/guides/crawl4ai.md).
* **Browser automation** - Drive a real browser with [Playwright](https://docs.apify.com/sdk/python/sdk/python/docs/guides/playwright.md) or [Selenium](https://docs.apify.com/sdk/python/sdk/python/docs/guides/selenium.md), or with higher-level tools such as [Browser Use](https://docs.apify.com/sdk/python/sdk/python/docs/guides/browser-use.md).
* **Web servers and APIs** - Run a [web server](https://docs.apify.com/sdk/python/sdk/python/docs/guides/running-webserver.md) inside an Actor to serve HTTP requests, for example to expose your scraper as a live API.
* **AI agents** - Host agents built with your framework of choice (see the [AI agents guide](https://docs.apify.com/sdk/python/sdk/python/docs/guides/ai-agents.md)). Ready-made Actor templates cover [LangGraph](https://apify.com/templates/python-langgraph), [CrewAI](https://apify.com/templates/python-crewai), [PydanticAI](https://apify.com/templates/python-pydanticai), [LlamaIndex](https://apify.com/templates/python-llamaindex-agent), and [Smolagents](https://apify.com/templates/python-smolagents).
* **MCP servers** - Deploy a Python MCP server as an Actor and make its tools available to any MCP client (see the [MCP servers guide](https://docs.apify.com/sdk/python/sdk/python/docs/guides/mcp-servers.md)). Ready-made Actor templates cover the [MCP server](https://apify.com/templates/python-mcp-empty) and [MCP proxy](https://apify.com/templates/python-mcp-proxy).

Whatever you build, the Apify SDK doesn't lock you into a particular framework. Bring the libraries you already use, and let Apify run your project in the cloud.

## Quick start[](#quick-start)

To create and run Actors using Apify Console, check out [Apify Console documentation](https://docs.apify.com/platform/console). For creating and running Python Actors locally, refer to the [quick start guide](https://docs.apify.com/sdk/python/sdk/python/docs/quick-start.md).

Explore the Guides section in the sidebar for a deeper understanding of the SDK's features and best practices.

## Installation[](#installation)

The Apify SDK for Python requires Python version 3.11 or above. It is typically installed when you create a new Actor project using the [Apify CLI](https://docs.apify.com/cli). To install it manually in an existing project, use:

```
pip install apify
```

API client alternative

If you need to interact with the Apify API programmatically without creating Actors, use the [Apify API client for Python](https://docs.apify.com/api/client/python) instead.
