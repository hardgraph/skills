---
title: Telegram integration through Zapier
url: https://docs.apify.com/integrations/telegram.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Workflows and notifications](https://docs.apify.com/integrations/workflows-and-notifications.md)
previous: [Slack](https://docs.apify.com/integrations/slack.md)
next: [viaSocket](https://docs.apify.com/integrations/viasocket.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Telegram integration through Zapier

With [Apify integration for Zapier](https://zapier.com/apps/apify/integrations), you can connect your Apify Actors to Slack, Trello, Google Sheets, Dropbox, Salesforce, and loads more.

Your Zapier workflows can start Apify Actors or tasks, fetch items from a dataset, set and get records from key-value stores, or find Actor or task runs.

You can use the Zapier integration to trigger a workflow whenever an Actor or a task finishes.

Complementary to the following guide we've created a detailed video, that will guide you through the process of setting up your Telegram integration through Zapier.

[YouTube video player](https://www.youtube.com/embed/XldEuQleq3c?si=86qbdrzWpVLoY_fr)

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Connect Apify with Zapier

To use the Apify integration on Zapier, you will need to:

* Have an [Apify account](https://console.apify.com/).
* Have a [Zapier account](https://zapier.com/).

### Step 1: Create Zap and find Apify on Zapier

Once you have your Zapier account ready and you are successfully logged in, you can create your first Zap.

Go to the Zaps section and find the **Create Zap** button.

In the create Zap form, you can choose whether you want to use Apify as the trigger or action for the Zap.

Click on Trigger and find Apify using the search box.

![Zapier choosing source of trigger](/assets/images/zapier-trigger-b545a74749bca7180546178fa17e9d08.png)

You have two possible triggers that you can choose while setting up your Telegram integration

* Finished Actor Run - triggers upon the completion of a selected Actor run.

* Finished Task Run - triggers upon the completion of a selected Actor task run.

![Zapier choosing trigger event](/assets/images/zapier-zap-choose-event-256ebd8df51a8482927c35e55376c2e0.png)

Available Actors & Tasks

Please note that only Actors or Actor tasks that you previously run will be available to choose from.

### Step 2: Create a connection to your Apify account

The next step is to connect your Apify account.

Click on the "Sign in" button next to the Connect to Apify title. Or you can select an account if you already have one connected.

The connection configuration options open in a new tab in your browser or in a modal window.

![Apify Zapier connection](/assets/images/zapier-new-connection-84a5dda50ec779c44f543918580c98f0.png)

In connection configuration, you need to provide your Apify API Token. You can find the token in Apify Console by navigating to **Settings > API & Integrations**.

![Integrations token](/assets/images/apify-integrations-token-a480c4034e9658f9989b7c661ee0fad5.png)

Copy the token and paste it into the configuration form and continue with the "Yes, Continue to Apify" button.

The connection is now created and the configuration form closed.

## Connect Telegram bot with Zapier

### Step 1: Create and connect new bot on Telegram

After setting up Apify as your trigger within Zapier, it's time to set up Telegram as the action that will occur based on the trigger.

![Choosing action source within Zapier](/assets/images/zapier-zap-choose-action-2cab03ba13fee2ae5e70cb4f06350e04.png)

You have two possible actions that you can choose while setting up your Telegram integration

* Send Message - sends a message from your bot when trigger activates.

* Send Poll - sends a poll from your bot when trigger activates.

![Zapier choosing action event](/assets/images/zapier-zap-choose-event-2-4f8cffd5c5cee82720b2b11ca91ff168.png)

After you choose your event that will be happening after trigger, you need to connect your Telegram bot that will be responsible for sending the message or sending polls.

![Telegram bot HTTP token](/assets/images/zapier-zap-telegram-access-f8700284981895e9d44f6d416c16beb1.png)

The best way to do it's to:

1. Start conversation with Telegrams BotFather, a bot that manages bots on Telegram.

   ![BotFather conversation](/assets/images/botfather-start-f6dbf707c00fcdd5bc3cd664e1125338.png)

2. Issue the `/newbot` command in conversation with it and follow the instructions, until you get your HTTP API token.

   ![BotFather conversation with new HTTP API token](/assets/images/botfather-HTTP-API-blurred-d6dbee971c3b8bfa2b734a07baa68db3.png)

### Step 2: Create action for your new Telegram bot

Once you've setup your new bot within Zapier, it's time to setup an action.

Start new conversation with your bot and copy the **Chat-Id** and input it within Zapier.

Select **Chat-Id**, **Text Format**, and a **Message Text** that suits your need for example

![Zapier choosing Action event](/assets/images/zapier-zap-action-39f63d9f8090d28c3a9526a64f270e91.png)

Once you fill all required fields, you can test your integration and if everything works hit **Publish** and you are done!
