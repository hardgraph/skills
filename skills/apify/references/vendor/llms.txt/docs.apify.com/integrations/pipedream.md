---
title: Pipedream integration
url: https://docs.apify.com/integrations/pipedream.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Workflows and notifications](https://docs.apify.com/integrations/workflows-and-notifications.md)
previous: [Website Content Crawler](https://docs.apify.com/integrations/n8n/website-content-crawler.md)
next: [Slack](https://docs.apify.com/integrations/slack.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Pipedream integration

[Pipedream](https://pipedream.com/) is a workflow automation platform for developers. With the [Apify integration for Pipedream](https://pipedream.com/apps/apify), you can run Actors, manage datasets and key-value stores, and trigger workflows when Actor or task runs finish.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Prerequisites

Before you begin, make sure you have:

* An [Apify account](https://console.apify.com/)
* A [Pipedream account](https://pipedream.com/)

## Connect Apify with Pipedream

1. Log into your Pipedream account and [create a new workflow](https://pipedream.com/docs/workflows).

2. [Add an Apify step](https://pipedream.com/docs/workflows/building-workflows/steps) (trigger or action) to your workflow. Select one of two Apify apps:

   ![Selecting the Apify app in Pipedream](/assets/images/pipedream-select-app-cfa9b63814fcb4eb1873d06a0356f204.png)

   * **Apify** - Authenticate with your Apify API token. Find it in [Apify Console](https://console.apify.com/settings/integrations) under **Settings > Integrations**.
   * **Apify (OAuth)** - Authorize access to your Apify account via OAuth.

3. Follow the prompts to authenticate your account.

   See Pipedream's [connected accounts documentation](https://pipedream.com/docs/apps/connected-accounts).

4. After connecting, you can use any Apify trigger or action in your workflows.

## Use Apify as a trigger

[Triggers](https://pipedream.com/docs/workflows/building-workflows/triggers) start your Pipedream workflow automatically when an event occurs in Apify.

1. [Create a new workflow](https://pipedream.com/docs/workflows) in Pipedream.

2. Select **Add Trigger** and search for **Apify**.

3. Select the trigger you want to use, e.g. **New Finished Actor Run**.

4. Configure the trigger by selecting the Actor or task to monitor.

   ![Configuring an Apify trigger in Pipedream](/assets/images/pipedream-trigger-a5d1343204d05c1a0e677572f0bd09c0.png)

5. Add subsequent steps to process the output.

## Use Apify as an action

[Actions](https://pipedream.com/docs/workflows/building-workflows/actions) let you perform Apify operations as part of a workflow. For example, you can run an Actor and then retrieve its dataset items.

1. [Create a new workflow](https://pipedream.com/docs/workflows) in Pipedream with any trigger.

2. Click **+** to add a step and search for **Apify**.

3. Select the action you want to use, e.g. **Run Actor**.

4. Configure the action parameters:

   * Select the Actor from Apify Store or your recently used Actors
   * Provide the Actor input as JSON
   * Set optional parameters such as timeout, memory, and build tag

   ![Configuring an Apify action in Pipedream](/assets/images/pipedream-action-4c8d5e0012af1a98189b73acf6e3d474.png)

5. Add another Apify step with **Get Dataset Items** to retrieve the Actor's output.

6. Add any subsequent steps to process or store the data.

## Triggers

* **New finished Actor run (instant)** - Triggers when a selected Actor run finishes.
* **New finished task run (instant)** - Triggers when a selected task run finishes.

## Actions

* **Run Actor** - Runs a selected Actor with customizable input and configuration.
* **Run task** - Runs a selected Actor task and optionally waits for it to finish.
* **Run task synchronously** - Runs a selected task and returns its dataset items when it finishes.
* **Scrape single URL** - Runs a scraper on a specified URL and returns its content as HTML. Use this for extracting content from a single page, e.g. in LLM workflows.
* **Get dataset items** - Retrieves items from a [dataset](https://docs.apify.com/storage/dataset.md).
* **Get key-value store record** - Retrieves a record from a [key-value store](https://docs.apify.com/storage/key-value-store.md).
* **Set key-value store record** - Creates or updates a record in a [key-value store](https://docs.apify.com/storage/key-value-store.md).

## Resources

* [Apify integration page on Pipedream](https://pipedream.com/apps/apify)
* [Pipedream documentation](https://pipedream.com/docs/)
* [Integration source code on GitHub](https://github.com/PipedreamHQ/pipedream/tree/master/components/apify)

If you have any questions or need help, reach out on the [Apify developer community on Discord](https://discord.com/invite/jyEM2PRvMU).
