---
title: Bubble integration
url: https://docs.apify.com/integrations/bubble.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Workflows and notifications](https://docs.apify.com/integrations/workflows-and-notifications.md)
previous: [Activepieces](https://docs.apify.com/integrations/activepieces.md)
next: [Dify](https://docs.apify.com/integrations/dify.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Bubble integration

[Bubble](https://bubble.io/) is a no-code platform that allows you to build web applications without writing code. With the [Apify integration for Bubble](https://bubble.io/plugin/apify-1749639212621x698168698147962900), you can easily connect your Apify Actors to your Bubble applications to automate workflows and display scraped data.

Explore the live demo

Open the demo Bubble app to check out the integration end-to-end before building your own: [apify-28595.bubbleapps.io](https://apify-28595.bubbleapps.io)

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Get started

To use the Apify integration for Bubble, you will need:

* An [Apify account](https://console.apify.com/)
* A [Bubble account](https://bubble.io/)
* A Bubble application where you want to use the integration

## Install the Apify plugin for Bubble

To integrate Apify with your Bubble application, you first need to install the Apify plugin from the Bubble plugin marketplace.

![Apify Plugin Download](/assets/images/plugin_install_preview-cf91187a70dc083a37333cefb62fe678.png)

1. Go to your Bubble application dashboard and navigate to the **Plugins** tab.
2. Click the **Add plugins** button.
3. Search for **Apify** in the plugin marketplace.
4. And then click **Install**.

## Configure the Apify plugin

After installing the plugin, you'll need to provide your API token when setting up Apify actions.

### Get your Apify API token

In Apify Console, go to **Settings → API & Integrations** and copy your API token. ![Apify API token](/assets/images/apify_api_token-2b80e6a9870851b2376e3a0475fe3a5e.png)

### Store the token securely in Bubble

For security, avoid hardcoding the token in action settings. Store it on the `User` data type with Privacy rules so only the current user can access their own token.

1. In Bubble, go to **Data → Data types**, open `User`.
2. Add a new field, for example `apify_api_token` (type: text).
   <!-- -->
   * ![Bubble data type](/assets/images/data_type_api_key.png)
3. Go to **Data → Privacy** and check if only the **Current User** is allowed to view their own `apify_api_token`.
   <!-- -->
   * ![Bubble data type](/assets/images/data_privacy-1a1bde669350b771d8601064c00e62de.png)

### Point Apify actions to the saved token

When configuring Apify actions in a workflow (check out screenshot below), set the token field dynamically to:

* `Current User's apify_api_token`
  * ![Current User\&#39;s API token](/assets/images/data_select_user_api_key.png)

## Use the integration

Once the plugin is configured, you can start building automated workflows.

### Actions vs data calls

Apify's Bubble plugin exposes two ways to interact with Apify:

* **Actions (workflow steps)**: Executed inside a Bubble workflow (both page workflows and backend workflows). Use these to trigger side effects like running an Actor or Task, or creating a webhook. They run during the workflow execution and can optionally wait for the result (if timeout is greater than 0).

  * Examples: **Run Actor**, **Run Actor Task**, **Create Webhook**, **Delete Webhook**.
  * Location in Bubble: **Workflow editor → Add an action → Plugins → Apify**
  * ![Apify Plugin\&#39;s actions](/assets/images/plugin_actions-4acb01aea79f54042521fb4263f3935d.png)

* **Data calls (data sources)**: Used as data sources in element properties and expressions. They fetch data from Apify and return it as lists/objects that you can bind to UI (for example, a repeating group) or use inside expressions.

  * Examples: **Fetch Data From Dataset JSON As Data**, **List Actor Runs**, **Get Record As Text/Image/File** from key-value store, **List User Datasets/Actors/Tasks**.
  * Location in Bubble: In any property input where a data source is expected click **Insert dynamic data**, under **Data sources** select **Get Data from an External API**, and choose the desired Apify data call.
  * ![Apify Plugin\&#39;s data calls](/assets/images/data_calls_preview-54f00c0451c9219fddb32654ed113d61.png)

Inline documentation

Each Apify plugin action and data call input in Bubble includes inline documentation describing what the parameter is for and the expected format. If you're unsure, check the field's help text in the Bubble editor.

### Dynamic values in inputs and data calls

Dynamic values are available across Apify plugin fields. Use Bubble's **Insert dynamic data** to bind values from your app.

* For instance you can source values from:

  <!-- -->

  * **Page/UI elements**: inputs, dropdowns, multi-selects, radio buttons, checkboxes
  * **Database Things and fields**
  * **Current User**
  * **Previous workflow steps** (e.g., Step 2's Run Actor result's `defaultDatasetId` or `runId`)
  * **Get Data from an External API**: data calls

#### Examples

##### Use a page input in an Action's JSON field (Input overrides)


```json
{

  "url": "Input URL's value"

}
```


Inserting dynamic data

When inserting dynamic data, Bubble replaces the selected text. Place your cursor exactly where you want the expression in the JSON; avoid selecting the entire field.

## Run Apify plugin actions from Bubble events

Create workflows that run Apify plugin actions in response to events in your Bubble app, such as button clicks or form submissions.

1. Open the **Workflow** tab and create a new workflow (for example, **When Run button is clicked**).

   <!-- -->

   * You can also click `Add workflow` button:
   * ![Adding workflow to button](/assets/images/button_adding_workflow-c53bed872e00cdd6f2b5295b64304655.png)
   * Or you can create it manually: `Workflows` → `+ New` → `An element is clicked`
   * ![Create workflow](/assets/images/create_workflow-7382207ef7663f35076cc04fbaddbe5e.png)
   * Then select the correct UI button.
   * ![Adding workflow to button](/assets/images/button_creating_workflow-158f7201546a2365cc573b18c6646e44.png)

2. Click `Add an action` → `Plugins` → choose one of the Apify actions:

   <!-- -->

   * For example `Run Actor` (run a specific Actor by ID)
   * ![Add action to workflow](/assets/images/add_action_to_workflow-5b8ab7a719fd6a7b09b7403c73af6c28.png)

3. Configure the action:

   <!-- -->

   * **API token**: set to `Current User's apify_api_token` (Point Apify actions to the saved token)
   * **Actor or Task**: paste an Actor ID
   * **Input overrides**: provide JSON and use dynamic expressions from page elements or things
   * **Timeout**: set in seconds (0 means no limit). Due to Bubble workflow time limits, set this explicitly. If you do not want to restrict the call duration, set it to 0.

### Where to find your IDs

Find IDs directly in Apify Console. Each resource page shows the ID in the API panel and in the page URL.

* **Actor ID**: Actor detail page → API panel or URL.

  <!-- -->

  * Example URL: `https://console.apify.com/actors/<actorId>`
  * Actor name format: owner/name (e.g., `apify/website-scraper`)

* **Task ID**: Task detail page → API panel or URL.
  <!-- -->
  * Example URL: `https://console.apify.com/actors/tasks/<taskId>`

* **Dataset ID**: Storage → Datasets → Dataset detail → API panel or URL.

  <!-- -->

  * Example URL: `https://console.apify.com/storage/datasets/<datasetId>`
  * Also available in the table in `Storage → Datasets` page

* **Key-value store ID**: Storage → Key-value stores → Store detail → API panel or URL.

  <!-- -->

  * Example URL: `https://console.apify.com/storage/key-value-stores/<storeId>`
  * Also available in the table in `Storage → Key-value stores` page

* **Webhook ID**: Actors → Actor → Integrations.
  <!-- -->
  * Example URL: `https://console.apify.com/actors/<actor_id>/integrations/<webhook_id>`

You can also discover IDs via the plugin responses and data calls (e.g., **List User Datasets**, **List Actor Runs**), which return objects with `id` fields you can pass into other actions/data calls.

## Display Apify data in your application

Populate elements in your Bubble application with information from your Apify account or Actor run data.

There are two common approaches:

### Display data

* This example appends the text result of an Actor run; it's a basic bind to the element’s text.
* Create / select the UI visual element - in this example, `Text`.
* In the Appearance tab, click the input area, select Insert dynamic data, and, according to your case, find the source - in this example, it's the `key_value_storages's recordContentText` custom state, where I set the result of the API call
* ![Display text data](/assets/images/text_dynamic_content-049036322c6ca388a75f4c29c97937b1.png)

### Display list of data

* This example lists the current user's datasets and displays them in a repeating group.

* Add a **Repeating group** to the page.

  <!-- -->

  1. Add data to a variable: create a custom state (for example, on the page) that will hold the list of datasets, and set it to the plugin's **List User Datasets** data call.
     <!-- -->
     * ![Step 1 - Set variable with user\&#39;s datasets](/assets/images/user_dataset_repeating_group_set-946b636162411634bb07743be1a80b03.png)
  2. Set the type: in the repeating group's settings, set **Type of content** to match the dataset object your variable returns.
     <!-- -->
     * ![Step 2 - Repeating group type of content](/assets/images/user_dataset_repeating_group-41aa54845047ec23a03a79099a7cff74.png)
  3. Bind the variable: set the repeating group's **Data source** to the variable from Step 1.
     <!-- -->
     * ![Step 3 - Repeating group data source](/assets/images/user_dataset_repeating_group_source-3cf20959b52881f8f8d309683b86af3b.png)

* Inside the repeating group cell, bind dataset fields (for example, `Current cell's item name`, `id`, `createdAt`).

* ![Step 4 - Repeating group data cell](/assets/images/user_dataset_repeating_group_cell-bef0311e8cec626e0bc3419c139256a6.png)

## Long‑running scrapes and Bubble time limits (async pattern)

Bubble workflows have execution time limits. Long scrapes (for example, **Scrape Single URL**) may time out if you wait for them. Use this asynchronous pattern.

### Prerequisite: Enable Bubble API and construct the webhook URL

To receive webhooks from Apify, enable Bubble's public API workflows and copy your API root URL:

1. In Bubble, go to **Settings → API** and enable **This app exposes a Workflow API**.
2. Copy the **Workflow API root URL**. It looks like `https://your-app.bubbleapps.io/version-test/api/1.1/wf`.
3. Create a backend workflow named `webhook`. Its full URL will be `https://your-app.bubbleapps.io/version-test/api/1.1/wf/webhook`.

Use this URL as the Apify webhook target. Configure the webhook's authentication as needed (e.g., a shared secret or query string token) and verify it inside your Bubble workflow before processing.

1. Trigger the scrape without waiting

   <!-- -->

   * In a workflow, add **Run Actor** (or **Run Actor Task**) and set **timeout** to 0.
   * Actor ID: `aYG0l9s7dbB7j3gbS` (`apify/website-content-crawler`).
   * Input: copy the Actor's input from the Actor's Input page, and map `crawlerType` and `url` to values from your UI.
   * ![Run scraping actor](/assets/images/step1_scraping-41ccba46a0ea3d216259106de1cebe92.png)

2. Notify Bubble when the run finishes

   <!-- -->

   * Create an Apify **Webhook** with event `ACTOR.RUN.SUCCEEDED`.
   * Set `actorId` from the Step 1 result.
   * Set `databaseId` from the Step 1 result, where Actor will store the result.
   * Set `idempotencyKey` to random value.
   * Set `requestUrl` to your Bubble backend workflow URL, for example: `https://your-app.bubbleapps.io/version-test/api/1.1/wf/webhook`.
   * ![Create a webhook](/assets/images/step2_scraping-bff76f237fa66dbe0521b78e3e13bb53.png)

3. Receive the webhook in Bubble and store the dataset ID

   <!-- -->

   * Create a public data type, for example, `ScrapingResults`.
   * Add a text field, for example, `result`, to store the dataset ID from the webhook.
   * ![Create a datatype with new field](/assets/images/step3_scraping-6a10330945aeb586eaafdecb3193de79.png)
   * Create the backend workflow (`webhook`) that Bubble exposes at `/api/1.1/wf/webhook`. The workflow name defines the API route.
   * ![Create a backend webhook](/assets/images/step4_scraping-46a0b4102826631f224a90c0ca9f9d0b.png)
   * In that workflow, for each received webhook call, create a new thing in `ScrapingResults` and set `result` to the dataset ID from the request body. This stores one `datasetId` per call for later processing.
   * ![Add new result](/assets/images/step5_scraping-27d487511588ee6fc6a261c8d44628ba.png)

4. Pick up the results asynchronously

   <!-- -->

   * In a (periodic) backend workflow, search `ScrapingResults` for pending entries (or for the expected `datasetId`).
   * If found, read its `result` (the `datasetId`), fetch items via the appropriate action (for example, **Fetch Data From Dataset JSON As Action**), update the UI or save to your DB, and then delete that `ScrapingResults` entry to avoid reprocessing.
   * If not found yet, do nothing and check again later.
   * ![Do every time](/assets/images/step6_scraping-b9b4629c95166135bd4a55d00c1b8501.png)

This approach avoids Bubble timeouts, keeps the UI responsive, and scales to larger scrapes.

## Example use cases

* *E-commerce price monitoring* - Schedule a daily workflow to run a price-scraping Actor on competitor sites. Store the results in your Bubble database, display them in a dashboard, and set up alerts for significant price changes.
* *Lead generation automation* - Trigger a workflow on form submission to run an Actor that enriches lead data, such as pulling company details from a domain. Save the enriched information to your database and automate follow-up actions like email campaigns.
* *Content aggregation* - Configure regular Actor runs to gather articles or posts from multiple sources.

## Available Apify actions and data sources

::: tip Check out the documentation

Each API call links to the Apify documentation. To learn more about any plugin action or data call, go to the **Plugins** page in your app, select the Apify plugin, and use the documentation links in the field descriptions.

:::

The Apify plugin provides two main types of operations:

**Data calls** (data sources):

* [Fetch Data From Dataset Text As Data](https://docs.apify.com/api/v2/dataset-items-get)
* [Fetch Data From Dataset File As Data](https://docs.apify.com/api/v2/dataset-items-get)
* [Fetch Data From Dataset Json As Data](https://docs.apify.com/api/v2/dataset-items-get)
* [List User Datasets](https://docs.apify.com/api/v2/datasets-get)
* [List Key-Value Stores](https://docs.apify.com/api/v2/key-value-stores-get)
* [List Store Keys](https://docs.apify.com/api/v2/key-value-store-keys-get)
* [Get Record As File](https://docs.apify.com/api/v2/key-value-store-record-get)
* [Get Record As Image File](https://docs.apify.com/api/v2/key-value-store-record-get)
* [List Store Actors](https://docs.apify.com/api/v2/store-get)
* [List User Actors](https://docs.apify.com/api/v2/actors-get)
* [List User Tasks](https://docs.apify.com/api/v2/actor-tasks-get)
* [Scrape Single URL As File As Data](https://docs.apify.com/api/v2/actor-run-sync-get-dataset-items-post)
* [Scrape Single URL As Json As Data](https://docs.apify.com/api/v2/actor-run-sync-get-dataset-items-post)
* [List Actor Runs](https://docs.apify.com/api/v2/actors-runs-get)
* [List Specific Actor Runs](https://docs.apify.com/api/v2/actors-runs-get)
* [List Webhooks](https://docs.apify.com/api/v2/webhooks-get)

**Actions** (workflow steps):

* [Fetch Data From Dataset Text As Action](https://docs.apify.com/api/v2/dataset-items-get)
* [Fetch Data From Dataset File As Action](https://docs.apify.com/api/v2/dataset-items-get)
* [Get Record As Text](https://docs.apify.com/api/v2/key-value-store-record-get)
* [Scrape Single URL As File As Action](https://docs.apify.com/api/v2/actor-run-sync-get-dataset-items-post)
* [Scrape Single URL As Json As Action](https://docs.apify.com/api/v2/actor-run-sync-get-dataset-items-post)
* [Run Actor](https://docs.apify.com/api/v2/actors-runs-post)
* [Run Actor Task](https://docs.apify.com/api/v2/actor-task-runs-post)
* [Create Webhook](https://docs.apify.com/api/v2/webhooks-post)
* [Delete Webhook](https://docs.apify.com/api/v2/webhook-delete)

## Use the latest version of the plugin

To stay up to date with new features, make sure you're using the latest version of the plugin. You can check this on the **Plugins** page by selecting the Apify plugin and choosing the latest version from the drop-down menu. You'll also see a brief note describing what's changed in that version.

## Troubleshooting

### Authentication errors

Ensure your API token is correctly set in the action (preferably as `Current User's apify_api_token`) and that it has the necessary permissions.

### Missing Actors or Tasks

If your Actor or Task doesn't appear in list responses, run it at least once in Apify Console so it becomes discoverable.

### Timeout errors

Bubble workflows have execution time limits. For long‑running Actors, set the **timeout** to 0 and process results asynchronously via a webhook and a backend workflow or scheduled event. See the Long‑running scrapes and Bubble time limits (async pattern) section for a step‑by‑step guide.

### Data format issues

Check that your JSON input is valid when providing **Input overrides** and that dynamic expressions resolve to valid JSON values. Verify the structure of the dataset output when displaying it in your app.

If you have any questions or need help, feel free to reach out on the [Apify developer community on Discord](https://discord.com/invite/jyEM2PRvMU).
