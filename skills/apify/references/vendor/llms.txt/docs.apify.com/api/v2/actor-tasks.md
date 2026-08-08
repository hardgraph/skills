---
title: Manage Actor tasks
url: https://docs.apify.com/api/v2/actor-tasks.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify API documentation](https://docs.apify.com/api.md)
  - [Apify API](https://docs.apify.com/api/v2.md)
children:
  - [Get list of tasks](https://docs.apify.com/api/v2/actor-tasks-get.md)
  - [Create task](https://docs.apify.com/api/v2/actor-tasks-post.md)
  - [Get task](https://docs.apify.com/api/v2/actor-task-get.md)
  - [Update task](https://docs.apify.com/api/v2/actor-task-put.md)
  - [Delete task](https://docs.apify.com/api/v2/actor-task-delete.md)
  - [Get task input](https://docs.apify.com/api/v2/actor-task-input-get.md)
  - [Update task input](https://docs.apify.com/api/v2/actor-task-input-put.md)
  - [Get list of webhooks](https://docs.apify.com/api/v2/actor-task-webhooks-get.md)
  - [Get list of task runs](https://docs.apify.com/api/v2/actor-task-runs-get.md)
  - [Run task](https://docs.apify.com/api/v2/actor-task-runs-post.md)
  - [Run task synchronously](https://docs.apify.com/api/v2/actor-task-run-sync-get.md)
  - [Run task synchronously](https://docs.apify.com/api/v2/actor-task-run-sync-post.md)
  - [Run task synchronously and get dataset items](https://docs.apify.com/api/v2/actor-task-run-sync-get-dataset-items-get.md)
  - [Run task synchronously and get dataset items](https://docs.apify.com/api/v2/actor-task-run-sync-get-dataset-items-post.md)
  - [Get last run](https://docs.apify.com/api/v2/actor-task-runs-last-get.md)
previous: [Get run's log](https://docs.apify.com/api/v2/actor-run-log-get.md)
next: [Get list of tasks](https://docs.apify.com/api/v2/actor-tasks-get.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Manage Actor tasks

The API endpoints described in this section enable you to create, manage, delete, and run Apify Actor tasks. For more information, see the [Actor tasts documentation](https://docs.apify.com/platform/actors/running/tasks).

note

For all the API endpoints that accept the `actorTaskId` parameter to specify a task, you can pass either the task ID (e.g. `HG7ML7M8z78YcAPEB`) or a tilde-separated username of the task's owner and the task's name (e.g. `janedoe~my-task`).

Some of the API endpoints return run objects. If any such run object contains usage in dollars, your effective unit pricing at the time of query has been used for computation of this dollar equivalent, and hence it should be used only for informative purposes.

You can learn more about platform usage in the [documentation](https://docs.apify.com/platform/actors/running/usage-and-resources#usage).

<!-- -->

## [Get list of tasks](https://docs.apify.com/api/v2/actor-tasks-get.md)

[/actor-tasks](https://docs.apify.com/api/v2/actor-tasks-get.md)

## [Create task](https://docs.apify.com/api/v2/actor-tasks-post.md)

[/actor-tasks](https://docs.apify.com/api/v2/actor-tasks-post.md)

## [Get task](https://docs.apify.com/api/v2/actor-task-get.md)

[/actor-tasks/{actorTaskId}](https://docs.apify.com/api/v2/actor-task-get.md)

## [Update task](https://docs.apify.com/api/v2/actor-task-put.md)

[/actor-tasks/{actorTaskId}](https://docs.apify.com/api/v2/actor-task-put.md)

## [Delete task](https://docs.apify.com/api/v2/actor-task-delete.md)

[/actor-tasks/{actorTaskId}](https://docs.apify.com/api/v2/actor-task-delete.md)

## [Get task input](https://docs.apify.com/api/v2/actor-task-input-get.md)

[/actor-tasks/{actorTaskId}/input](https://docs.apify.com/api/v2/actor-task-input-get.md)

## [Update task input](https://docs.apify.com/api/v2/actor-task-input-put.md)

[/actor-tasks/{actorTaskId}/input](https://docs.apify.com/api/v2/actor-task-input-put.md)

## [Get list of webhooks](https://docs.apify.com/api/v2/actor-task-webhooks-get.md)

[/actor-tasks/{actorTaskId}/webhooks](https://docs.apify.com/api/v2/actor-task-webhooks-get.md)

## [Get list of task runs](https://docs.apify.com/api/v2/actor-task-runs-get.md)

[/actor-tasks/{actorTaskId}/runs](https://docs.apify.com/api/v2/actor-task-runs-get.md)

## [Run task](https://docs.apify.com/api/v2/actor-task-runs-post.md)

[/actor-tasks/{actorTaskId}/runs](https://docs.apify.com/api/v2/actor-task-runs-post.md)

## [Run task synchronously](https://docs.apify.com/api/v2/actor-task-run-sync-get.md)

[/actor-tasks/{actorTaskId}/run-sync](https://docs.apify.com/api/v2/actor-task-run-sync-get.md)

## [Run task synchronously](https://docs.apify.com/api/v2/actor-task-run-sync-post.md)

[/actor-tasks/{actorTaskId}/run-sync](https://docs.apify.com/api/v2/actor-task-run-sync-post.md)

## [Run task synchronously and get dataset items](https://docs.apify.com/api/v2/actor-task-run-sync-get-dataset-items-get.md)

[/actor-tasks/{actorTaskId}/run-sync-get-dataset-items](https://docs.apify.com/api/v2/actor-task-run-sync-get-dataset-items-get.md)

## [Run task synchronously and get dataset items](https://docs.apify.com/api/v2/actor-task-run-sync-get-dataset-items-post.md)

[/actor-tasks/{actorTaskId}/run-sync-get-dataset-items](https://docs.apify.com/api/v2/actor-task-run-sync-get-dataset-items-post.md)

## [Get last run](https://docs.apify.com/api/v2/actor-task-runs-last-get.md)

[/actor-tasks/{actorTaskId}/runs/last](https://docs.apify.com/api/v2/actor-task-runs-last-get.md)
