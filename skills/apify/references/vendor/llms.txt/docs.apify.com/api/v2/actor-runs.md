---
title: Manage Actor runs
url: https://docs.apify.com/api/v2/actor-runs.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify API documentation](https://docs.apify.com/api.md)
  - [Apify API](https://docs.apify.com/api/v2.md)
children:
  - [Get user runs list](https://docs.apify.com/api/v2/actor-runs-get.md)
  - [Get run](https://docs.apify.com/api/v2/actor-run-get.md)
  - [Update run](https://docs.apify.com/api/v2/actor-run-put.md)
  - [Delete run](https://docs.apify.com/api/v2/actor-run-delete.md)
  - [Abort run](https://docs.apify.com/api/v2/actor-run-abort-post.md)
  - [Metamorph run](https://docs.apify.com/api/v2/actor-run-metamorph-post.md)
  - [Reboot run](https://docs.apify.com/api/v2/actor-run-reboot-post.md)
  - [Resurrect run](https://docs.apify.com/api/v2/post-resurrect-run.md)
  - [Charge events in run](https://docs.apify.com/api/v2/post-charge-run.md)
  - [Get run's log](https://docs.apify.com/api/v2/actor-run-log-get.md)
previous: [Get OpenAPI definition](https://docs.apify.com/api/v2/actor-build-openapi-json-get.md)
next: [Get user runs list](https://docs.apify.com/api/v2/actor-runs-get.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Manage Actor runs

The API endpoints described in this section enable you to manage, and delete Apify Actor runs.

If any returned run object contains usage in dollars, your effective unit pricing at the time of query has been used for computation of this dollar equivalent, and hence it should be used only for informative purposes.

For completed runs, aggregated fields such as `stats` or dollar usage totals are eventually consistent and update within a few seconds. For values that must match finalized totals, wait about 10 seconds after the run completed, then fetch the run again.

You can learn more about platform usage in the [documentation](https://docs.apify.com/platform/actors/running/usage-and-resources#usage).

<!-- -->

## [Get user runs list](https://docs.apify.com/api/v2/actor-runs-get.md)

[/actor-runs](https://docs.apify.com/api/v2/actor-runs-get.md)

## [Get run](https://docs.apify.com/api/v2/actor-run-get.md)

[/actor-runs/{runId}](https://docs.apify.com/api/v2/actor-run-get.md)

## [Update run](https://docs.apify.com/api/v2/actor-run-put.md)

[/actor-runs/{runId}](https://docs.apify.com/api/v2/actor-run-put.md)

## [Delete run](https://docs.apify.com/api/v2/actor-run-delete.md)

[/actor-runs/{runId}](https://docs.apify.com/api/v2/actor-run-delete.md)

## [Abort run](https://docs.apify.com/api/v2/actor-run-abort-post.md)

[/actor-runs/{runId}/abort](https://docs.apify.com/api/v2/actor-run-abort-post.md)

## [Metamorph run](https://docs.apify.com/api/v2/actor-run-metamorph-post.md)

[/actor-runs/{runId}/metamorph](https://docs.apify.com/api/v2/actor-run-metamorph-post.md)

## [Reboot run](https://docs.apify.com/api/v2/actor-run-reboot-post.md)

[/actor-runs/{runId}/reboot](https://docs.apify.com/api/v2/actor-run-reboot-post.md)

## [Resurrect run](https://docs.apify.com/api/v2/post-resurrect-run.md)

[/actor-runs/{runId}/resurrect](https://docs.apify.com/api/v2/post-resurrect-run.md)

## [Charge events in run](https://docs.apify.com/api/v2/post-charge-run.md)

[/actor-runs/{runId}/charge](https://docs.apify.com/api/v2/post-charge-run.md)

## [Get run's log](https://docs.apify.com/api/v2/actor-run-log-get.md)

[/actor-runs/{runId}/log](https://docs.apify.com/api/v2/actor-run-log-get.md)
