---
title: Manage schedules
url: https://docs.apify.com/api/v2/schedules.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify API documentation](https://docs.apify.com/api.md)
  - [Apify API](https://docs.apify.com/api/v2.md)
children:
  - [Get list of schedules](https://docs.apify.com/api/v2/schedules-get.md)
  - [Create schedule](https://docs.apify.com/api/v2/schedules-post.md)
  - [Get schedule](https://docs.apify.com/api/v2/schedule-get.md)
  - [Update schedule](https://docs.apify.com/api/v2/schedule-put.md)
  - [Delete schedule](https://docs.apify.com/api/v2/schedule-delete.md)
  - [Get schedule log](https://docs.apify.com/api/v2/schedule-log-get.md)
previous: [Get webhook dispatch](https://docs.apify.com/api/v2/webhook-dispatch-get.md)
next: [Get list of schedules](https://docs.apify.com/api/v2/schedules-get.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Manage schedules

This section describes API endpoints for managing schedules.

Schedules are used to automatically start your Actors at certain times. Each schedule can be associated with a number of Actors and Actor tasks. It is also possible to override the settings of each Actor (task) similarly to when invoking the Actor (task) using the API. For more information, see [Schedules documentation](https://docs.apify.com/platform/schedules).

Each schedule is assigned actions for it to perform. Actions can be of two types

* `RUN_ACTOR` and `RUN_ACTOR_TASK`.

For details, see the documentation of the Get schedule endpoint.

<!-- -->

## [Get list of schedules](https://docs.apify.com/api/v2/schedules-get.md)

[/schedules](https://docs.apify.com/api/v2/schedules-get.md)

## [Create schedule](https://docs.apify.com/api/v2/schedules-post.md)

[/schedules](https://docs.apify.com/api/v2/schedules-post.md)

## [Get schedule](https://docs.apify.com/api/v2/schedule-get.md)

[/schedules/{scheduleId}](https://docs.apify.com/api/v2/schedule-get.md)

## [Update schedule](https://docs.apify.com/api/v2/schedule-put.md)

[/schedules/{scheduleId}](https://docs.apify.com/api/v2/schedule-put.md)

## [Delete schedule](https://docs.apify.com/api/v2/schedule-delete.md)

[/schedules/{scheduleId}](https://docs.apify.com/api/v2/schedule-delete.md)

## [Get schedule log](https://docs.apify.com/api/v2/schedule-log-get.md)

[/schedules/{scheduleId}/log](https://docs.apify.com/api/v2/schedule-log-get.md)
