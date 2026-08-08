---
title: Windmill integration
url: https://docs.apify.com/integrations/windmill.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Workflows and notifications](https://docs.apify.com/integrations/workflows-and-notifications.md)
previous: [viaSocket](https://docs.apify.com/integrations/viasocket.md)
next: [Workato](https://docs.apify.com/integrations/workato.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Windmill integration

[Windmill](https://www.windmill.dev/) is an open-source automation platform for building scripts and flows that connect your tools and data. With the Apify integration for Windmill, you can run Actors and tasks, scrape websites, extract data from storage, and trigger workflows based on Apify events.

This guide shows you how to install the Apify package, set up authentication, and create automated workflows that integrate with Apify.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Prerequisites

Before you begin, make sure you have:

* An [Apify account](https://console.apify.com/)
* A [Windmill instance](https://www.windmill.dev/docs/getting_started/how_to_use_windmill) (self-hosted or cloud)

## Install the Apify integration

The Apify integration provides scripts, flows, and resources that will be available through the Windmill Hub.

![Apify Hub](/assets/images/windmill-install-hub-8d5c17df17314e3157adb1bc462a9dc9.png)

### Step 1: Import Apify scripts from Windmill Hub

You can import Apify integration scripts into your flows from the Windmill Hub, regardless of whether you're using Windmill Cloud or a self-hosted instance. The following components will be available:

**Scripts (Actions):**

* Run Actor
* Run Task
* Scrape Single URL
* Get Dataset Items
* Get Key-Value Store

**Scripts (Triggers):**

* Polling-based Actor Run Trigger
* Polling-based Task Trigger

**Flows (Triggers):**

* Webhook-based Actor Trigger
* Webhook-based Task Trigger

**Resources:**

* Apify API Key Resource (for storing your API token)
* Webhook Config Resource (optional, for managing webhook-based triggers)
* Apify OAuth Resource (available only on Windmill Cloud)

### Step 2: Authentication

You can provide the token to scripts via a **Windmill Resource**. Create it either in the **Resources** tab or directly from a script.

#### Option A - Create in the Resources tab

1. Open **Resources** → **New Resource**.
2. Select `apify_api_key` resource type.
3. Name it (e.g., `apify_token`) and paste your Apify API token.
4. Save, then reference this resource in your scripts/flows.

![Apify Auth](/assets/images/windmill-install-auth-resource-tab-347333f728a8339f9ea1207891669c2d.png)

#### Option B - Create/bind from a script

1. Open the script in Windmill UI.
2. Add a secret input parameter (e.g., `apify_token`) .
3. Bind it to the resource you created (or create a new one inline).
4. Use the bound token inside the script to call Apify.

![Apify Auth](/assets/images/windmill-install-auth-script-663a0e5478da1be8c8d9b1d2550d23a6.png)

#### Option C - OAuth authentication

Cloud-only feature

OAuth authentication is only available on Windmill Cloud, not on self-hosted instances.

1. Open **Resources** → **New Resource**.
2. Select `apify` resource type.
3. Click the **Connect** button.
4. Complete the OAuth flow to authorize Windmill to access your Apify account.
5. Save the resource, then reference it in your scripts/flows just like the API key resource.

![Apify Auth](/assets/images/windmill-install-oauth-resource-e76f7607cb8be90b0213aaa573bc93cd.png)

## Create your first workflow

Let's create a simple workflow that runs an Actor and fetches its results.

### Step 1: Create a new flow

1. In the Windmill UI, click **New Flow**.
2. Give your flow a descriptive name (e.g., "Run Actor and Get Results").

### Step 2: Add the Run Actor script

1. Click **Add Step** and search for "Run Actor".

2. Select the **Run Actor** script.

3. Configure the inputs:

   * **Apify Auth**: Select either your *API Key* or *OAuth Token* resource
   * **Actor ID or Slug**: Enter the Actor you want to run
   * **Input**: JSON input for the Actor (optional)
   * **Wait for Finish**: Set to `true` to wait for completion
   * **Memory**: Memory allocation in MB (optional)
   * **Timeout**: Timeout in seconds (optional)

![Apify Flow](/assets/images/windmill-flow-run-actor-2d5b709f4d928bc37fe37c66d4930edf.png)

### Step 3: Add the Get Dataset Items script

1. Add another step and search for "Get Dataset Items".

2. Configure the inputs:

   * **Apify Auth**: Select either your *API Key* or *OAuth Token* resource
   * **Dataset ID**: Use the `defaultDatasetId` from the previous step
   * **Limit**: Number of items to retrieve (optional)

![Apify Flow](/assets/images/windmill-flow-get-dataset-847210612b28fa4dbbf730eb62ce452e.png)

### Step 4: Test and run

With the flow created and scripts linked we can test and run it.

![Apify Flow](/assets/images/windmill-flow-linked-79604147b41aaaaa553dac8e7d2e0538.png)

1. Click **Test** to run the flow with sample data.
2. Review the results and ensure both steps completed successfully.
3. Save and activate your flow.

## Use webhooks to trigger workflows

Windmill provides webhook-based triggers that can automatically start workflows when Apify events occur.

### Step 1: Fork the example flow

1. Search for **Example Actor Webhook Flow** in Windmill hub. ![Apify Webhook](/assets/images/windmill-webhook-hub-5a8ba5a4b8b22ee28909596b94773377.png)
2. Fork the **Apify example flow with webhook trigger** from Windmill’s templates. ![Apify Webhook](/assets/images/windmill-webhook-fork-4f3d91d1b53efa00f00a61aee4be17fe.png)
3. Add a trigger of type **Webhook**. ![Apify Webhook](/assets/images/windmill-webhook-set-trigger-8c5ab6475c81273218a29ee3bb06c19f.png)
4. Create a **Webhook-specific Token**.
5. Copy both the **token** and the **webhook URL** to your clipboard. These will be used for creation of the **Webhook Config** resource in the next step.

### Step 2: Configure the webhook creation script

1. In the flow, open the script that creates the Apify webhook.

2. Create a new **Webhook Config** resource using the saved **token** and **webhook URL** values from previous step. ![Apify Webhook](/assets/images/windmill-webhook-config-resource-ed7403e0952e1501dd3db87f338e372b.png)

3. Configure the inputs:

   * **Apify Auth**: Select either your *API Key* or *OAuth Token* resource
   * **Actor ID**: the Apify Actor you want to monitor
   * **Event Types**: the events that should trigger the flow ![Apify Webhook](/assets/images/windmill-webhook-config-1766c58e83ccdfe976f3c44a3b36e426.png)

4. **IMPORTANT**: Test-run this script to **create the webhook in Apify** for the Actor. ![Apify Webhook](/assets/images/windmill-webhook-create-result-a667a94b201ccddcb7eed8adc426bd4a.png)

### Step 3: Adjust the flow logic

1. Replace the default logic in the flow with your desired actions:

   * Example: persist results into a Google Sheet.

2. If needed, use the provided **helper script** to “mold” the webhook payload into the correct shape for the Google Sheets operation.

![Apify Webhook](/assets/images/windmill-webhook-process-dataset-feb1c1bef632d551b9ebd10fe73655b5.png)

### Step 4: Test the flow

1. Deploy the flow so it can be triggered by the webhook.
2. Run the Actor in Apify that the webhook is registered for.
3. Check the **flow runs** in Windmill to verify that the test run was registered and the logic executed.

![Apify Webhook](/assets/images/windmill-webhook-test-runs-1462a31667e7b01b82270a5a510e5983.png)

## Delete the webhook

1. Fork the **Apify's Delete Webhook** script from the Windmill Hub.
2. Set either your *API Key* or *OAuth Token* resource
3. Set the **Webhook Config Resource** to the webhook you want to delete.
4. Run the script to delete the webhook in Apify.

![Apify Webhook](/assets/images/windmill-webhook-delete-8660601d8b2c1e08163f05c71f305fd3.png)

## Available operations

The Apify integration provides several operations you can use in your Windmill workflows.

### Actions (Scripts)

* **Run Actor**: Starts an Actor with optional input and configuration, with the option to wait for completion or run asynchronously. Returns run metadata including dataset ID.
* **Run Task**: Executes a predefined Actor task. Similar to Run Actor but uses task configuration, making this ideal for recurring operations.
* **Scrape Single URL**: Runs a lightweight scraper for a single webpage and returns content as text, markdown, and HTML, making this perfect for quick content extraction.
* **Get Dataset Items**: Retrieves items from a dataset with options to filter by dataset ID or last run. Supports pagination and item limits.
* **Get Key-Value Store**: Reads values from key-value stores with the ability to retrieve specific keys or list all keys, making this useful for configuration and state management.

### Triggers

#### Webhook-based Triggers

* **Actor Webhook Trigger**: Responds to Actor run events
* **Task Webhook Trigger**: Responds to task run events
* Real-time event processing with full payload data

#### Polling-based Triggers

* **Actor Run Polling Trigger**: Periodically checks for new Actor runs
* **Task Run Polling Trigger**: Periodically checks for new task runs
* Fallback option when webhooks aren't available

## Resources

* [Windmill Documentation](https://www.windmill.dev/docs/)
* [Windmill Local Development](https://www.windmill.dev/docs/advanced/local_development)
* [Apify API Documentation](https://docs.apify.com)
* [Apify Webhooks](https://docs.apify.com/integrations/webhooks.md)
* [Apify Actors & Tasks](https://docs.apify.com/actors.md)

## Troubleshooting

If you have any questions or need help, feel free to reach out to us on our [developer community on Discord](https://discord.com/invite/jyEM2PRvMU).
