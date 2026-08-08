---
title: Lindy integration
url: https://docs.apify.com/integrations/lindy.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
previous: [LangGraph](https://docs.apify.com/integrations/langgraph.md)
next: [LlamaIndex](https://docs.apify.com/integrations/llama-index.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Lindy integration

[Lindy](https://www.lindy.ai/) is an AI-powered automation platform that lets you create intelligent workflows and automate complex tasks. By integrating Apify with Lindy, you can leverage Apify's web scraping capabilities within Lindy's AI-driven automation workflows to extract data, monitor websites, and trigger actions based on scraped information.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Prerequisites

To use the Apify integration with Lindy, you need:

* A Lindy account with access to premium actions (required for certain integrations or higher usage limits).

## How to Run an Apify Actor from Lindy

This section demonstrates how to integrate Apify's data extraction capabilities into Lindy's AI automation.

1. Start a new Lindy workflow by clicking the **+ New Lindy** button.

   ![Lindy dashboard with new Lindy button highlighted](/assets/images/lindy-new-button-455860dd5a4537f85d9ffcfd59434492.png)

   Select **Start from scratch** to build a custom workflow.

   ![Lindy workflow creation options, with \&quot;Start from scratch\&quot; selected](/assets/images/lindy-scratch-c8420dba2bf3586f860ddc538c612815.png)

2. Choose a trigger that will initiate your automation. For this demonstration, we will select **Chat with Lindy/Message received**. This allows you to trigger the Apify Actor simply by sending a message to Lindy.

   ![Lindy workflow editor, showing trigger selection, with \&quot;Select Trigger\&quot; highlighted](/assets/images/lindy-trigger-7c76b60f84ca086a502e157bc3b65a50.png) ![Lindy workflow editor, showing trigger selection, with \&quot;Chat with Lindy/Message received\&quot; chosen](/assets/images/lindy-received-d7214e022c2e00d51664bce3c804bb17.png)

3. After setting the trigger, select **Perform an Action**.

   ![Lindy workflow editor, showing the option to \&quot;Perform an Action\&quot;](/assets/images/lindy-action-33de046c5cd6e51ae9644373a9dd44a9.png)

   In the action search box, search for "Apify" or navigate to the **Scrapers** category and choose **Run Actor**.

   ![Lindy action search box with \&quot;Apify\&quot; typed, showing \&quot;Run Actor\&quot; option, or \&quot;Scrapers\&quot; category with \&quot;Run Actor\&quot; highlighted.](/assets/images/lindy-run-actor-72b07884bc23e4e98cfc0adbe98f5a66.png)

4. Configure the Apify "Run Actor" Module. In the Apify "Run Actor" configuration, choose the Actor you want to execute. For example, select the **Instagram profile scraper**.

   ![Apify \&quot;Run Actor\&quot; module configuration in Lindy, showing a dropdown or search for Actors, with \&quot;Instagram profile scraper\&quot; selected.](/assets/images/lindy-instagram-actor-3bdd1e3110314bf1d8923e38c049cc07.png)

Actor availability

You have access to thousands of Actors available on the [Apify Store](https://apify.com/store). Please note that Actors using the *rental pricing model* are not available for use with this integration. The rental pricing model is being retired during 2026 and migrated to pay-per-usage. For details, refer to our [rental pricing model documentation](https://docs.apify.com/actors/publishing/monetize/rental.md).

This establishes the fundamental workflow:\
*Chatting with Lindy can now trigger the Apify Instagram Profile Scraper.*

### Extend your workflow

Lindy offers different triggers (e.g., *email received*, *Slack message received*, etc.) and actions beyond running an Actor.

After the Apify Actor run is initiated, you can define what happens next, depending on your needs:

* **When Actor Run Starts:**

  * You might want to send a notification.
  * Log the start time.
  * Run a pre-processing step.

* **After Results Are Available:** Once the Apify Actor completes and its results are ready, you can:

  <!-- -->

  * Retrieve the Actor's output data from its dataset.
  * Pass the extracted data to Lindy's AI for summarization, analysis, content generation, or other AI-driven tasks.
  * Route the data to other services (e.g., Google Sheets, databases, email notifications) using Lindy's action modules.

## Available Actions in Lindy for Apify

While Lindy's specific module names may evolve, the core Apify functionalities typically exposed are:

* **Run Actor:** Initiates a specific Apify Actor and can optionally wait for its completion.
