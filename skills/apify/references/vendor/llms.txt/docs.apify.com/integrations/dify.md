---
title: Dify integration
url: https://docs.apify.com/integrations/dify.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Workflows and notifications](https://docs.apify.com/integrations/workflows-and-notifications.md)
previous: [Bubble](https://docs.apify.com/integrations/bubble.md)
next: [Gmail](https://docs.apify.com/integrations/gmail.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Dify integration

[Dify](https://dify.ai/) is an intuitive LLM application development platform for creating and operating AI-native apps. With the Apify integration for Dify, you can connect Apify Actors and storage to your Dify Workflows and Chatflows. You can run scrapers, extract data, and trigger workflows based on Apify events.

This guide explains how to set up authentication and incorporate the Apify plugin into your Dify applications as either a tool (action) or a trigger.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Prerequisites

Before you begin, make sure you have:

* An [Apify account](https://console.apify.com/)
* A [Dify account](https://dify.ai/) (self‑hosted or cloud)

## Authentication

To connect your Apify account, configure the plugin using your Apify API token or OAuth.

1. In Dify, navigate to **Plugins** from the top menu.
2. Select **Install plugin → Marketplace**.
3. Find the **apify-integration** plugin and install it.
4. Return to the **Plugins** page.

### API key

1. Open the plugin window and select **Add API Key**.
2. In the pop-up menu, paste your Apify API token. You can find it in the [Apify Console](https://console.apify.com/settings/integrations).
3. Select **Save**.

![Apify Auth in Dify](/assets/images/dify-connection-popup-09dd755a5505557766837645088b35a6.png)

A green indicator confirms that your credentials are correct.

![Apify Auth in Dify](/assets/images/dify-connection-success-5fac2413b87f1b0ac48f4ca419ef215f.png)

### OAuth

1. Open the plugin window and select **Add OAuth**.
2. Follow the OAuth flow to authorize Dify to access your Apify account.
3. Select **Save** to complete the connection.

With authentication set up, you can now add Apify tools to your applications.

## Use Apify tools in a Workflow

Create a new **Workflow** application in Dify Studio and choose **Create from Blank**. Select the **+** icon on the **User Input** or **Trigger** node and choose an Apify tool to add it to your board.

![Adding an Apify Tool in Dify](/assets/images/dify-workflow-create-5849270a834cfc199a0a449a32cae5e5.png) ![Adding an Apify Tool in Dify](/assets/images/dify-workflow-start-1d0ea850caa1717664e4a69733226a1a.png)

### Use Apify as an action

Actions let you perform operations like running an Actor within a workflow. For example, trigger a scraper and use its results in later steps.

#### Run an Actor

Add the **Run Actor** tool to your workflow and configure it with the following parameters:

* **Actor ID**: The unique ID of the Actor to run (for example, `2Mdma1N6Fd0y3QEjR` for Google Maps Scraper)
* **JSON body for input**: The JSON input for the Actor run
* **Wait for finish**: Select `True` to make the workflow wait for the run to complete

Example of JSON input


```json
{

  "language": "en",

  "locationQuery": "New York, USA",

  "maxCrawledPlacesPerSearch": 50,

  "searchStringsArray": [ "restaurant" ],

  "skipClosedPlaces": false

}
```


![Adding an Apify Tool in Dify](/assets/images/dify-configure-actor-run-c0d130e30df07d26ac2544e5c03eb932.png)

Add an **Output** node and configure its output variable to use the result from the **Run Actor** node.

![Run Actor Node Configuration](/assets/images/dify-configure-end-node-7466476cb94cfbe2b4a610f10369ee11.png)

You can now start the workflow and view the results.

![Run Actor Node Configuration](/assets/images/dify-ready-actor-flow-b409c5d44e8eb36cf07044eac3eb0ef5.png)

#### Run an Actor task

Add the **Run Actor task** tool to your workflow and enter the **Task ID** you want to execute. Add an **Output** node and map its output variable to the result from this tool.

![Run Actor Task Node](/assets/images/dify-task-input-e557c498f4b3a356c9cb5baae8d85077.png)

#### Scrape Single URL

Add the **Scrape Single URL** tool and enter the **URL** to scrape (for example, `https://docs.apify.com/`). Select the **Crawler type**, use **Raw HTTP** for faster execution within Dify's timeout limits. Add an **Output** node and map its output variable to the result.

![Scrape Single URL Node](/assets/images/dify-scrape-input-ee545bcb95e73268d9f5ed2a265382d7.png)

#### Get Dataset Items

Add the **Get Dataset Items** tool and enter the **Dataset ID** to retrieve items from. You can use a variable picker to select the **Dataset ID** from a previous **Run Actor** or **Run Actor task** node. Add an **Output** node and map its output to the tool's result.

![Get Dataset Items Node](/assets/images/dify-dataset-input-979d42abc0e21bd67b161471a72d47c6.png) ![Get Dataset Items Node](/assets/images/dify-dataset-flow-6928a095f56b9e015ac95ba79fcf667f.png)

#### Get Key-Value Store

Add the **Get Key-Value Store** tool and enter the **Store ID** and **Key** for the record you want to retrieve. Add an **Output** node and map its output to the tool's result. You can also download files from the store.

![Get Key-Value Store Node](/assets/images/dify-key-value-input-1a6d4205ad2e866b0bf491eeca147dfd.png) ![Get Key-Value Store Node](/assets/images/dify-key-value-files-d5af73394e0d2b942efd61af6a4a6258.png)

## Use Apify as a trigger

Triggers enable your Dify application to respond automatically to Apify events, such as completed Actor runs. This approach is ideal for creating event-driven automations.

### Set up a webhook endpoint

In Dify, navigate to **Plugins** from the top menu. Click on your plugin and look for **Endpoints** section. Select the **+** icon to create a new endpoint. Choose the **Workflow** or **Chatflow** application you want to trigger and give it a name. Select **Save**. Dify will generate two URLs.

![Create Dify Endpoint](/assets/images/dify-workflow-endpoints-7fd67f0bd268924ec0ef56b7a58b7105.png) ![Create Dify Endpoint](/assets/images/dify-workflow-input-bd71bf6931181b3b154dfe77b113f2eb.png)\
![Create Dify Endpoint](/assets/images/dify-trigger-url-0409e080731af51d506ff3ebbde26ed9.png)

Copy the URL that corresponds to your application type (Workflow or Chatflow).

### Configure the webhook in Apify

In the Apify Console, open the Actor you want to use as a trigger and go to the **Integrations** tab. Select the **HTTP webhook** option and paste the Dify endpoint URL. Under **Events**, select **Run succeeded** and save the integration.

![Create Dify Endpoint](/assets/images/dify-trigger-webhook-134ac96adbf52603511bcb8303298086.png)\
![Create Dify Endpoint](/assets/images/dify-trigger-apify-console-328ca4856a611aa87d4fefbc89fd42c0.png)

Publishing required

A Dify Workflow or Chatflow must be published for the webhook trigger to work. If the application is not published, the webhook will fail.

![Create Dify Endpoint](/assets/images/dify-workflow-publish-b17cf775870c611ea31e6c5eac4c25ae.png)

Start the Actor in the Apify Console to trigger your workflow.

### Trigger a Chatflow

The setup for a Chatflow is similar to a Workflow. Select your Chatflow app instead of a Workflow and copy the Chatflow endpoint URL.

When triggering a Chatflow, you must provide a **Payload template** in the Apify webhook settings. This template needs a `query` field to serve as the first message to the chatflow.

![Chatflow Payload Template](/assets/images/dify-trigger-query-26749d20fb6091d4b13a548cd7a5c9b9.png)

### Access Apify Data in Dify

When an Apify Actor run completes and triggers your Dify workflow, Apify sends a [JSON response object](https://docs.apify.com/api/v2/actors-runs-post#responses) containing information about the completed run. This includes details like the run ID, Actor ID, dataset ID, and status.

#### Understand nested data access

Dify's variable system cannot directly access nested JSON properties using dot notation like `resource.id`. Instead, you must use a flattened format where each level of nesting is represented by double underscores (`__`).

For example, if Apify sends:


```json
{

  "resource": {

    "id": "abc123",

    "defaultDatasetId": "xyz789"

  }

}
```


In Dify, you would access these values as:

* `resource__id` for the run ID
* `resource__defaultDatasetId` for the dataset ID

#### Define input variables

To use nested properties from the webhook payload in your workflow:

1. In your workflow's **Input Node** node, select **+** to add a new input variable.
2. Enter the flattened property name (e.g., `resource__defaultDatasetId`).
3. The variable is now available throughout your workflow.

![Chatflow Payload Template](/assets/images/dify-define-input-variable-060b6cd80fc6408c560ee4c3037fb073.png)

#### Example: Download data from a completed Actor run

A common use case is automatically fetching data from a dataset after an Actor run completes. Here's how to set it up:

1. *Define the dataset ID variable*: In your workflow's Input node, add an input variable named `resource__defaultDatasetId` to capture the dataset ID from the webhook payload.
2. *Add the Get Dataset Items tool*: Add the **Get Dataset Items** Apify tool to your workflow.
3. *Use the variable*: In the **Dataset ID** field of the Get Dataset Items tool, reference your input variable (e.g., `{{resource__defaultDatasetId}}`).
4. *Process the data*: The tool retrieves all items from the dataset, which you can then process, transform, or send to other services in your workflow.

![Chatflow Payload Template](/assets/images/dify-input-variable-webhook-baec4ff8f7decacddb3b867a5a67b78c.png)

This workflow automatically downloads and processes data whenever your Actor completes, without manual intervention.
