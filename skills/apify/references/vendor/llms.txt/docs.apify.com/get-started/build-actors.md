---
title: Build Actors
url: https://docs.apify.com/get-started/build-actors.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Get started](https://docs.apify.com/get-started.md)
previous: [Run Actors](https://docs.apify.com/get-started/run-actors.md)
next: [Agent onboarding](https://docs.apify.com/get-started/agent-onboarding.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Build Actors

An Actor is ordinary code - JavaScript, Python, or anything that runs in a Docker container - packaged with a definition that tells the Apify platform how to run it. The definition specifies what input it accepts, what output it produces, and what environment it needs. That packaging is what makes an Actor more than a script. The platform handles the servers, scaling, storage, and scheduling, and every Actor you build gets an API, a user interface, and integrations for free.

The journey from idea to published Actor has three stages: develop it, deploy it to the platform, and publish it in [Apify Store](https://apify.com/store) if you want others to use it. This page walks you through those stages and points you to the right material at each one.

## Understand the Actor model

What turns your code into an Actor is a small set of files: an `actor.json` manifest, a Dockerfile, and schemas that describe the input and output. The input schema is what generates the input form users see in Apify Console, and the Apify SDK gives your code access to input, storage, and platform events at runtime. Understand these pieces and the rest of Actor development falls into place.

* [Actor definition](https://docs.apify.com/actors/development/actor-definition.md) - the `actor.json`, Dockerfile, and schemas:

  <!-- -->

  * [Input schema](https://docs.apify.com/actors/development/actor-definition/input-schema.md)
  * [Dataset schema](https://docs.apify.com/storage/dataset-schema.md)
  * [Key-value store schema](https://docs.apify.com/storage/key-value-store-schema.md)

* [Programming interface](https://docs.apify.com/actors/development/programming-interface.md) - the Apify SDK, environment variables, and state persistence.

* [Builds and runs](https://docs.apify.com/actors/development/builds-and-runs.md) - the build and run lifecycle and versioning.

* [Storage](https://docs.apify.com/storage.md) - how to read input and write results from code.

## Create your first Actor

The fastest way to a working Actor is a template - a runnable project you modify rather than a blank file. You can develop locally with your own tools, in the web IDE without any setup, or with an AI coding assistant.

* [Actor development quick start](https://docs.apify.com/actors/development/quick-start.md) - pick your entry point:

  <!-- -->

  * [Local development](https://docs.apify.com/actors/development/quick-start/locally.md) with the Apify CLI.
  * [Web IDE](https://docs.apify.com/actors/development/quick-start/web-ide.md) - build in the browser, no local setup.
  * [Build with AI](https://docs.apify.com/actors/development/quick-start/build-with-ai.md) - use AI coding assistants with the Apify MCP server.

* [Build an Actor from a template](https://docs.apify.com/academy/getting-started/creating-actors.md) - a guided Apify Academy lesson using the web IDE.

* [Inputs and outputs from scratch](https://docs.apify.com/academy/getting-started/inputs-outputs.md) - build an Actor that takes input and stores a result.

* [Develop AI agents](https://docs.apify.com/actors/development/quick-start/develop-ai-agents.md) - templates, sandboxes, LLM access, and pay-per-event monetization.

## Deploy and iterate

Deployment turns your source code into a build - a Docker image the platform can run. From there, development becomes a loop: push a change, build, run, check the results. You can drive the loop by hand with the CLI or wire it to a Git repository so every push deploys automatically.

* [Deploy your Actor](https://docs.apify.com/actors/development/deployment.md) to the platform.
* [Set up continuous integration](https://docs.apify.com/actors/development/deployment/continuous-integration.md) for automatic builds.
* [Optimize performance](https://docs.apify.com/actors/development/performance.md) to reduce cost and increase throughput.
* [Configure permissions](https://docs.apify.com/actors/development/permissions.md) to control what your Actor can access.

## Publish to Store

Publishing puts your Actor in front of everyone browsing Apify Store, and lets you charge for it. A good public Actor is more than working code. It needs a clear input schema, a README that explains what it does and how to use it, and reliable runs that strangers can depend on. The publishing guide covers all of it, including the monetization models you can choose from.

* [Publish and monetize](https://docs.apify.com/actors/publishing.md) your Actor in Apify Store.
* Call your Actors from your own code with the [API](https://docs.apify.com/api/v2.md) or the [API clients](https://docs.apify.com/academy/getting-started/apify-client.md).

Just want to run Actors, not build them? See [Run Actors](https://docs.apify.com/get-started/run-actors.md).
