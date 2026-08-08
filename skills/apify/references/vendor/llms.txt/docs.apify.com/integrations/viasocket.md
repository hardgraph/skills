---
title: viaSocket integration
url: https://docs.apify.com/integrations/viasocket.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Workflows and notifications](https://docs.apify.com/integrations/workflows-and-notifications.md)
previous: [Telegram](https://docs.apify.com/integrations/telegram.md)
next: [Windmill](https://docs.apify.com/integrations/windmill.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# viaSocket integration

[viaSocket](https://viasocket.com/) is a workflow automation platform that lets you connect apps and automate tasks without writing code. With the Apify integration, you can trigger Actor runs, retrieve results, and build automation pipelines that respond to events across your connected apps.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Step 1: Generate an Apify API token

1. Log in to your [Apify account](https://console.apify.com/).
2. Go to **Settings > API & Integrations > API Tokens**.
3. Copy your personal API token.

![Apify API token settings](/assets/images/viasocket-apify-token-eb365361921c116e2c601b93a30483ac.png)

Token security

Never share your API token publicly or commit it to version control.

## Step 2: Create a new flow in viaSocket

1. Log in to your [viaSocket account](https://viasocket.com/) and click **Create New Flow**.
2. In the **Trigger** section, search for and select **Apify**. ![Search for Apify in viaSocket](/assets/images/viasocket-search-apify-76fe94635c911a97024fb7ab0b118d7e.png)
3. Choose **Finished Task Run** or **Finished Actor Run** as the trigger event. ![Select Apify trigger in viaSocket](/assets/images/viasocket-select-trigger-c4ec6411fbae8389aef8eb7f4b7ac44c.png)

## Step 3: Connect your Apify account and configure the trigger

1. Click **Connect to Apify**.
2. Paste your Apify API token into the connection dialog.
3. Click **Save** to establish the connection.
4. Confirm the connection is successfully added before continuing.
5. Provide the Actor or task ID manually or map it dynamically from a previous step.
6. Set **Status** to `Succeeded`.
7. Click **Test** to fetch sample data and verify the trigger works.
8. Save the trigger configuration.

![Add Apify trigger in viaSocket](/assets/images/viasocket-add-trigger-5d320ffcaab41685f1a59c1ec3ff8d9d.png)

## Step 4: Add an action

You can add any supported app as the next step in your flow. The example below uses Gmail to send an email when the trigger fires.

1. Click **Add Step**.

2. Select **Gmail** and choose the **Send Email** action.

3. Connect or select your Gmail account.

4. Map the following fields:

   <!-- -->

   * **To**: recipient email address
   * **Subject**: email subject line
   * **Message Body**: use the trigger `body` object as dynamic input

5. Click **Test** to run the action and confirm a `200` response status.

![Gmail action configuration in viaSocket](/assets/images/viasocket-gmail-action-2107c4e6491b75f7b902fec8833ed2ec.png)

## Step 5: Go live and monitor

1. Click **Go Live** and confirm activation.
2. Use **Flow View** to inspect the flow structure and **Log View** to monitor individual executions.
3. Re-run a specific execution from **Run History** if needed.

![viaSocket flow monitoring](/assets/images/viasocket-monitor-a61e4b63d9d7bb565337416ac574df10.png)

For questions or help, join the [Apify Discord community](https://discord.com/invite/jyEM2PRvMU).
