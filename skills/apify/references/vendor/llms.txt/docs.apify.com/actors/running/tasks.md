---
title: Actor tasks
url: https://docs.apify.com/actors/running/tasks.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Running Actors](https://docs.apify.com/actors/running.md)
previous: [Permissions](https://docs.apify.com/actors/running/permissions.md)
next: [Standby mode](https://docs.apify.com/actors/running/standby.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Actor tasks

Actor tasks let you create multiple reusable configurations of a single Actor, adapted for specific use cases. For example, you can create one [Web Scraper](https://apify.com/apify/web-scraper) configuration (task) that scrapes the latest reviews from [IMDb](https://www.imdb.com/), another that scrapes nike.com for the latest sneakers, and a third that scrapes your competitor's e-shop. You can then use and reuse these configurations directly from [Apify Console](https://console.apify.com/actors/tasks), [Schedules](https://docs.apify.com/actors/running/schedules.md), or [API](https://docs.apify.com/api/v2/actor-task-runs-post.md).

You can find all your tasks in the [Apify Console](https://console.apify.com/actors/tasks).

## Create

To create a task, open any Actor from [Apify Store](https://console.apify.com/store) or your list of [Actors](https://console.apify.com/actors) in Apify Console. At the top-right section of the page, click the **Save as a new task** button.

![Create a new Apify task](/assets/images/tasks-create-task-ab91dd6c7bffbe719965658a986120c4.png)

## Configure

You can set up your task's input under the **Input** tab. A task's input configuration works just like an Actor's. After all, it's just a copy of an Actor you can pre-configure for a specific scenario. You can use either JSON or the visual input UI.

![Apify task configuration](/assets/images/tasks-create-configure-c3a0cc4d2e00baeee1d9e29fd1ac2ec1.png)

An Actors' input fields may vary depending on their purpose, but they all follow the same principle: *you provide an Actor with the information it needs so it can do what you want it to do.*

You can set run options such as timeout and [memory](https://docs.apify.com/actors/running/usage-and-resources.md) in the **Run options** tab of the task's input configuration.

### Naming

To make a task easier to identify, you can give it a name, title, and description by clicking its caption on the detail page. A task's name should be at least `3` characters long with a limit of `63` characters.

## Run

Once you've configured your task, you can run it using the **Start** button on the top-right side of the screen.

![Run an Apify task](/assets/images/tasks-start-button-10c64e3fbc13d906e0498c44c0857e12.png)

Or using the **Start** button positioned following the input configuration.

![Run an Apify task v2](/assets/images/tasks-start-after-configuration-22843067b3a7207ec59002fa909985af.png)

You can also run tasks using:

* [Schedules](https://docs.apify.com/actors/running/schedules.md).
* Directly via the [Apify API](https://docs.apify.com/api/v2/actor-task-runs-post.md).
* The [JavaScript API client](https://docs.apify.com/api/client/js/reference/class/TaskClient).
* The [Python API client](https://docs.apify.com/api/client/python/reference/class/TaskClient).

## Share

Like any other resource, you can share your Actor tasks with other Apify users via the [access rights](https://docs.apify.com/account/collaboration.md) system.
