---
title: Runs
url: https://docs.apify.com/actors/development/builds-and-runs/runs.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Development](https://docs.apify.com/actors/development.md)
  - [Builds and runs](https://docs.apify.com/actors/development/builds-and-runs.md)
previous: [Builds](https://docs.apify.com/actors/development/builds-and-runs/builds.md)
next: [State persistence](https://docs.apify.com/actors/development/builds-and-runs/state-persistence.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Runs

When you start an Actor, you create a run. A run is a single execution of your Actor with a specific input in a Docker container.

## Starting an Actor

You can start an Actor in several ways:

* Manually from the [Apify Console](https://console.apify.com/actors) UI
* Via the [Apify API](https://docs.apify.com/api/v2/actors-runs-post.md)
* Using the [Scheduler](https://docs.apify.com/actors/running/schedules.md) provided by the Apify platform
* By one of the available [integrations](https://docs.apify.com/integrations.md)

## Input and environment variables

The run receives input via the `INPUT` record of its default [key-value store](https://docs.apify.com/storage/key-value-store.md). Environment variables are also passed to the run. For more information about environment variables check the [Environment variables](https://docs.apify.com/actors/development/programming-interface/environment-variables.md) section.

## Run duration and timeout

Actor runs can be short or long-running. To prevent infinite runs, you can set a timeout. The timeout is specified in seconds, and the default timeout varies based on the template from which you create your Actor. If the run doesn't finish within the timeout, it's automatically stopped, and its status is set to `TIMED-OUT`.
