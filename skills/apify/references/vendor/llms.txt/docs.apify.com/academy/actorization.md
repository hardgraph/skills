---
title: Wrap open-source as an Actor
url: https://docs.apify.com/academy/actorization.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [How to build an Actor](https://docs.apify.com/academy/actor-marketing-playbook/store-basics/how-to-build-actors.md)
previous: [How to build an Actor](https://docs.apify.com/academy/actor-marketing-playbook/store-basics/how-to-build-actors.md)
next: [How to create a great input schema](https://docs.apify.com/academy/actor-marketing-playbook/product-optimization/how-to-create-a-great-input-schema.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Wrap open-source as an Actor

Apify is a cloud platform with a [marketplace](https://apify.com/store) of web scraping and automation tools called *Actors*. These tools are used for extracting data from social media, search engines, maps, e-commerce sites, travel portals, and general websites.

Most Actors are developed by a global creator community, and some are developed by Apify. The platform has a growing base of monthly active users and developers, and creators earn money from their Actors through Apify's monetization program.

## What are Actors

Under the hood, Actors are programs packaged as Docker images, that accept a well-defined JSON input, perform an action, and optionally produce a well-defined JSON output. This makes it easy to auto-generate user interfaces for Actors and integrate them with one another or with external systems. For example, we have user-friendly integrations with Zapier, Make, LangChain, MCP, OpenAPI, and SDKs for TypeScript/Python, CLI, etc. etc.

Actors are a new way to build reusable serverless micro-apps that are easy to develop, share, integrate, and build upon - and, importantly, monetize. While Actors are our invention, we’re in the process of making them an open standard. Learn more at [https://whitepaper.actor](https://whitepaper.actor/).

While most Actors on Apify Store are web scrapers or crawlers, there are ever more Actors for other use cases including data processing, web automation, API backend, or [AI agents](https://apify.com/store/categories/agents). In fact, any piece of software that accepts input, performs a job, and can run in Docker, can be *Actorized* simply by adding an `.actor` directory to it with a couple of JSON files.

## Why Actorize

By publishing your service or project at [Apify Store](https://apify.com/store) your project will benefit from:

1. *Expanded reach*: Your tool instantly becomes available to Apify's user community and connects with popular automation platforms like [Make](https://www.make.com), [n8n](https://n8n.io/), and [Zapier](https://zapier.com/).
2. *Multiple monetization paths*: Choose from flexible pricing models (pay-per-event or monthly subscriptions).
3. *AI integration*: Your Actor can serve as a tool for AI agents through Apify's MCP (Model Context Protocol) server, creating new use cases and opportunities while you earn 80% of all revenues.

Open-Source Benefits

For open-source developers, Actorization adds value without extra costs:

* Host your code in the cloud for easy user trials (no local installs needed).
* Avoid managing cloud infrastructure - users cover the costs.
* Earn income through [Apify’s Open Source Fair Share program](https://apify.com/partners/open-source-fair-share) via GitHub Sponsors or direct payouts.
* Publish and monetize 10x faster than building a micro-SaaS, with Apify handling infra, billing, and access to its established user base.

For example, IBM’s [Docling project](https://github.com/docling-project/docling) merged our pull request that actorized their popular open-source GitHub repo and added the [Apify Actor status badge](https://docs.apify.com/actors/publishing/status-badge.md) to the README:

![Docling Apify badge](/assets/images/docling-apify-badge-3b6ad8beefffa23d0ffcc9bc92d593bb.png)

### Example Actorized projects

You can Actorize various projects ranging from open-source libraries, throughout existing SaaS services, up to MCP server:

| Name            | Type                   | Source                                                                                     | Actor                                                                                                                  |
| --------------- | ---------------------- | ------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| Parsera         | SaaS service           | [https://parsera.org/](https://parsera.org/)                                               | [https://apify.com/parsera-labs/parsera](https://apify.com/parsera-labs/parsera)                                       |
| Monolith        | Open source library    | [https://github.com/Y2Z/monolith](https://github.com/Y2Z/monolith)                         | [https://apify.com/snshn/monolith](https://apify.com/snshn/monolith)                                                   |
| Crawl4AI        | Open source library    | [https://github.com/unclecode/crawl4ai](https://github.com/unclecode/crawl4ai)             | [https://apify.com/janbuchar/crawl4ai](https://apify.com/janbuchar/crawl4ai)                                           |
| Docling         | Open source library    | [https://github.com/docling-project/docling](https://github.com/docling-project/docling)   | [https://apify.com/vancura/docling/source-code](https://apify.com/vancura/docling/source-code)                         |
| Playwright MCP  | Open source MCP server | [https://github.com/microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) | [https://apify.com/jiri.spilka/playwright-mcp-server](https://apify.com/jiri.spilka/playwright-mcp-server)             |
| Browserbase MCP | SaaS MCP server        | [https://www.browserbase.com/](https://www.browserbase.com/)                               | [https://apify.com/agentify/browserbase-mcp-server/api/mcp](https://apify.com/agentify/browserbase-mcp-server/api/mcp) |

For more inspiration on which open-source libraries to wrap as Actors, see the [open-source libraries](https://docs.apify.com/academy/build-and-publish/actor-ideas/find-actor-ideas.md#open-source-libraries) section in the guide on finding Actor ideas.

### What projects are suitable for Actorization

Use these criteria to decide if your project is a good candidate for Actorization:

1. *Is it self-contained?* Does the project work non-interactively, with a well-defined, preferably structured input and output format? Positive examples include various data processing utilities, web scrapers and other automation scripts. Negative examples are GUI applications or applications that run indefinitely. If you want to run HTTP APIs on Apify, you can do so using [Actor Standby](https://docs.apify.com/actors/development/programming-interface/standby.md).
2. *Can the state be stored in Apify storages?* If the application has state that can be stored in a small number of files it can utilize [key-value store](https://docs.apify.com/storage/key-value-store.md), or if it processes records that can be stored in Apify’s [request queue](https://docs.apify.com/storage/request-queue.md). If the output consists of one or many similar JSON objects, it can utilize [dataset](https://docs.apify.com/storage/dataset.md).
3. *Can it be containerized?* The project needs to be able to run in a Docker container. Apify currently does not support GPU workloads. External services (e.g., databases) need to be managed by developer.
4. *Can it use Apify tooling?* Javascript/Typescript applications and Python applications can be Actorized with the help of the [Apify SDK](https://docs.apify.com/sdk.md), which makes easy for your code to interacts with the Apify platform. Applications that can be run using just the CLI can also be Actorized using the Apify CLI by writing a simple shell script that retrieves user input using [Apify CLI](https://docs.apify.com/cli), then runs your application and sends the results back to Apify (also using the CLI). If your application is implemented differently, you can still call the [Apify API](https://docs.apify.com/api/v2.md) directly - it’s just HTTP and pretty much every language has support for that but the implementation is less straightforward.

## Actorization guide

This guide outlines the steps to convert your application into an Apify [Actor](https://docs.apify.com/actors.md). Follow the documentation links for detailed information - this guide provides an overview rather than exhaustive instructions.

### 1. Add Actor metadata - the `.actor` folder

The Apify platform requires your Actor repository to have a `.actor` folder at the root level, which contains the metadata needed to build and run the Actor.

For existing projects, you can add the `.actor` folder using the [apify init CLI command](https://docs.apify.com/cli/docs/reference#apify-init-actorname).

In case you're starting a new project, we strongly advise to start with a [template](https://apify.com/templates) using the [apify create CLI command](https://docs.apify.com/cli/docs/reference#apify-create-actorname) based on your usecase

* [TypeScript template](https://apify.com/templates/ts-empty)

* [Python template](https://apify.com/templates/python-empty)

* [CLI template](https://apify.com/templates/cli-start)

* [MCP server template](https://apify.com/templates/python-mcp-server)

* … and many others, check out for comprehensive list [https://apify.com/templates](https://apify.com/templates)

  Quick Start for beginners

  For a step-by-step introduction to creating your first Actor (including tech stack choices and development paths), see [Get started](https://docs.apify.com/get-started.md).

The newly created `.actor` folder contains an `actor.json` file - a manifest of the Actor. See [documentation](https://docs.apify.com/actors/development/actor-definition/actor-json.md) for more details

You must also make sure your Actor has a Dockerfile and that it installs everything needed to successfully run your application. Check out [Dockerfile documentation](https://docs.apify.com/actors/development/actor-definition/dockerfile.md) by Apify. If you don't want to use these, you are free to use any image as the base of your Actor.

When launching the Actor, the Apify platform will simply run your Docker image. This means that a) you need to configure the `ENTRYPOINT` and `CMD` directives so that it launches your application and b) you can test your image locally using Docker.

These steps are the bare minimum you need to run your code on Apify. The rest of the guide will help you flesh it out better.

### 2. Define input and output

Most Actors accept an input and produce an output. As part of Actorization, you need to define the input and output structure of your application.

For detailed information, read the docs for [input schema](https://docs.apify.com/actors/development/actor-definition/input-schema.md), [dataset schema](https://docs.apify.com/storage/dataset-schema.md), and general [storage](https://docs.apify.com/storage.md).

#### Design guidelines

1. If your application has some arguments or options, those should be part of the input defined by input schema.
2. If there is a configuration file or if your application is configured with environment variables, those should also be part of the input. Ideally, nested structures should be “unpacked”, i.e., try not to accept deeply nested structures in your input. Start with less input options and expand later.
3. If the output is a single file, you’ll probably want your Actor to output a single dataset item that contains a public URL to the output file stored in the Apify key-value store
4. If the output has a table-like structure or a series of JSON-serializable objects, you should output each row or object as a separate dataset item
5. If the output is a single key-value record, your Actor should return a single dataset item

### 3. Handle state persistence (optional)

If your application performs a number of well-defined subtasks, the [request queue](https://docs.apify.com/storage/request-queue.md) lets you pause and resume execution on job restart. This is important for long-running jobs that might be migrated between servers at some point. In addition, this allows the Apify platform to display the progress to your users in the UI.

A lightweight alternative to the request queue is simply storing the state of your application as a JSON object in the key-value store and checking for that when your Actor is starting.

Fully-fledged Actors will often combine these two approaches for maximum reliability. More on this topic you find in the [state persistence](https://docs.apify.com/actors/development/builds-and-runs/state-persistence.md) article.

### 4. Write Actorization code

Perhaps the most important part of the Actorization process is writing the code that will be executed when the Apify platform launches your Actor.

Unless you’re writing an application targeted directly on the Apify platform, this will have the form of a script that calls your code and integrates it with the Apify Storages

Apify provides SDKs for [JavaScript](https://docs.apify.com/sdk/js) and [Python](https://docs.apify.com/sdk/python) plus the [Apify CLI](https://docs.apify.com/cli) allowing an easy interaction with the Apify platform from the command line.

Check out [programming interface](https://docs.apify.com/actors/development/programming-interface.md) documentation article for details on interacting with the Apify platform in your Actor's code.

### 5. Deploy the Actor

Deployment to the Apify platform can be done easily via `apify push` command of the [Apify CLI](https://docs.apify.com/cli) and for details see [deployment](https://docs.apify.com/actors/development/deployment.md) documentation.

### 6. Publish and monetize

For details on publishing the Actor in [Apify Store](https://apify.com/store) see the [Publishing and monetization](https://docs.apify.com/actors/publishing.md). You can also follow the guide on how to [Create an Actor README](https://docs.apify.com/actors/publishing/actor-readme.md) and [Marketing checklist](https://docs.apify.com/academy/actor-marketing-playbook/promote-your-actor/checklist.md).

To show your Actor's current status and usage in your README or documentation, add the [Actor status badge](https://docs.apify.com/actors/publishing/status-badge.md).
