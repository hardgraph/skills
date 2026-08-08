---
title: Actor runs
url: https://docs.apify.com/api/v2/actors-actor-runs.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify API documentation](https://docs.apify.com/api.md)
  - [Apify API](https://docs.apify.com/api/v2.md)
children:
  - [Get list of runs](https://docs.apify.com/api/v2/actors-runs-get.md)
  - [Run Actor](https://docs.apify.com/api/v2/actors-runs-post.md)
  - [Run Actor synchronously and return key-value store record](https://docs.apify.com/api/v2/actor-run-sync-post.md)
  - [Run Actor synchronously without input](https://docs.apify.com/api/v2/actor-run-sync-get.md)
  - [Run Actor synchronously and get dataset items](https://docs.apify.com/api/v2/actor-run-sync-get-dataset-items-post.md)
  - [Run Actor synchronously without input and get dataset items](https://docs.apify.com/api/v2/actor-run-sync-get-dataset-items-get.md)
  - [Resurrect run](https://docs.apify.com/api/v2/actor-run-resurrect-post.md)
  - [Get last run](https://docs.apify.com/api/v2/actor-runs-last-get.md)
  - [Get run](https://docs.apify.com/api/v2/actors-run-get.md)
  - [Abort run](https://docs.apify.com/api/v2/actors-run-abort-post.md)
  - [Metamorph run](https://docs.apify.com/api/v2/actors-run-metamorph-post.md)
previous: [Abort build](https://docs.apify.com/api/v2/actors-build-abort-post.md)
next: [Get list of runs](https://docs.apify.com/api/v2/actors-runs-get.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Actor runs

The API endpoints in this section allow you to manage your Apify Actors runs.

Some API endpoints return run objects. If a run object includes usage costs in dollars, note that these values are calculated based on your effective unit pricing at the time of the query. As a result, the dollar amounts should be treated as informational only and not as exact figures.

For completed runs, aggregated fields such as `stats` or dollar usage totals are eventually consistent and update within a few seconds. For values that must match finalized totals, wait about 10 seconds after the run completed, then fetch the run again.

For more information about platform usage and resource calculations, see the [Usage and Resources documentation](https://docs.apify.com/platform/actors/running/usage-and-resources#usage).

<!-- -->

## [Get list of runs](https://docs.apify.com/api/v2/actors-runs-get.md)

[/actors/{actorId}/runs](https://docs.apify.com/api/v2/actors-runs-get.md)

## [Run Actor](https://docs.apify.com/api/v2/actors-runs-post.md)

[/actors/{actorId}/runs](https://docs.apify.com/api/v2/actors-runs-post.md)

## [Run Actor synchronously and return key-value store record](https://docs.apify.com/api/v2/actor-run-sync-post.md)

[/actors/{actorId}/run-sync](https://docs.apify.com/api/v2/actor-run-sync-post.md)

## [Run Actor synchronously without input](https://docs.apify.com/api/v2/actor-run-sync-get.md)

[/actors/{actorId}/run-sync](https://docs.apify.com/api/v2/actor-run-sync-get.md)

## [Run Actor synchronously and get dataset items](https://docs.apify.com/api/v2/actor-run-sync-get-dataset-items-post.md)

[/actors/{actorId}/run-sync-get-dataset-items](https://docs.apify.com/api/v2/actor-run-sync-get-dataset-items-post.md)

## [Run Actor synchronously without input and get dataset items](https://docs.apify.com/api/v2/actor-run-sync-get-dataset-items-get.md)

[/actors/{actorId}/run-sync-get-dataset-items](https://docs.apify.com/api/v2/actor-run-sync-get-dataset-items-get.md)

## [Resurrect run](https://docs.apify.com/api/v2/actor-run-resurrect-post.md)

[/actors/{actorId}/runs/{runId}/resurrect](https://docs.apify.com/api/v2/actor-run-resurrect-post.md)

## [Get last run](https://docs.apify.com/api/v2/actor-runs-last-get.md)

[/actors/{actorId}/runs/last](https://docs.apify.com/api/v2/actor-runs-last-get.md)

## [Get run](https://docs.apify.com/api/v2/actors-run-get.md)

[/actors/{actorId}/runs/{runId}](https://docs.apify.com/api/v2/actors-run-get.md)

## [Abort run](https://docs.apify.com/api/v2/actors-run-abort-post.md)

[/actors/{actorId}/runs/{runId}/abort](https://docs.apify.com/api/v2/actors-run-abort-post.md)

## [Metamorph run](https://docs.apify.com/api/v2/actors-run-metamorph-post.md)

[/actors/{actorId}/runs/{runId}/metamorph](https://docs.apify.com/api/v2/actors-run-metamorph-post.md)
