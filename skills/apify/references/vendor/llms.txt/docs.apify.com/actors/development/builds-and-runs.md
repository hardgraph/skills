---
title: Builds and runs
url: https://docs.apify.com/actors/development/builds-and-runs.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Development](https://docs.apify.com/actors/development.md)
children:
  - [Builds](https://docs.apify.com/actors/development/builds-and-runs/builds.md)
  - [Runs](https://docs.apify.com/actors/development/builds-and-runs/runs.md)
  - [State persistence](https://docs.apify.com/actors/development/builds-and-runs/state-persistence.md)
previous: [Continuous integration](https://docs.apify.com/actors/development/deployment/continuous-integration.md)
next: [Builds](https://docs.apify.com/actors/development/builds-and-runs/builds.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Builds and runs

Actor **builds** and **runs** are fundamental concepts within the Apify platform. Understanding them is crucial for effective use of the platform.

## Build an Actor

When you start the build process for your Actor, you create a *build*. A build is a Docker image containing your source code and the required dependencies needed to run the Actor:

<!-- -->

## Run an Actor

To create a *run*, you take your *build* and start it with some input:

<!-- -->

## Lifecycle

Actor builds and runs share a common lifecycle. Each build and run begins with the initial status **READY** and progress through one or more transitional statuses to reach a terminal status.

<!-- -->

***

| Status     | Type         | Description                                 |
| ---------- | ------------ | ------------------------------------------- |
| READY      | initial      | Started but not allocated to any worker yet |
| RUNNING    | transitional | Executing on a worker machine               |
| SUCCEEDED  | terminal     | Finished successfully                       |
| FAILED     | terminal     | Run failed                                  |
| TIMING-OUT | transitional | Timing out now                              |
| TIMED-OUT  | terminal     | Timed out                                   |
| ABORTING   | transitional | Run is being aborted                        |
| ABORTED    | terminal     | Run aborted                                 |
