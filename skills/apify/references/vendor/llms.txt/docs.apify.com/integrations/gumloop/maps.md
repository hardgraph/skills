---
title: Gumloop - Google maps Actor integration
url: https://docs.apify.com/integrations/gumloop/maps.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Workflows and notifications](https://docs.apify.com/integrations/workflows-and-notifications.md)
  - [Gumloop](https://docs.apify.com/integrations/gumloop.md)
previous: [Instagram](https://docs.apify.com/integrations/gumloop/instagram.md)
next: [TikTok](https://docs.apify.com/integrations/gumloop/tiktok.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Gumloop - Google maps Actor integration

Search, extract, and enrich business data from Google Maps in Gumloop.

***

The Gumloop Google Maps integration provides a native interface for running Apify’s Google Maps scrapers directly in your workflows. No API keys or manual polling required. All you need is a Gumloop account.

Using the Gumloop Google Maps MCP node, you can simply prompt the location data you need and Gumloop will retrieve it from relevant Apify Actors. From there, you can connect it to your favorite tools and AI agents to process the information.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Available actions

You can pull the following types of place data from Google Maps using Gumloop’s Google Maps node (via Apify). Each action has a credit cost.

| Tool/Action         | Description                                                                      | Credit Cost        |
| ------------------- | -------------------------------------------------------------------------------- | ------------------ |
| Search places       | Search for places on Google Maps using location and search terms.                | 3 credits per item |
| Get place details   | Retrieve detailed information about a specific place using its URL or place ID.  | 5 credits per item |
| Search by category  | Search for places by a specific category (e.g. cafes, gyms) on Google Maps.      | 3 credits per item |
| Get place reviews   | Fetch reviews for specific locations, including text, rating, and reviewer info. | 3 credits per item |
| Find places in area | Return all visible places within a defined map area or bounding box.             | 3 credits per item |

## Retrieve Google Maps data in Gumloop

1. *Add the Gumloop Google Maps MCP node.*

   First, add the Google Maps node from the node library to your workflow canvas.

   ![Google Maps MCP node in Node Library](/assets/images/mcp-node-image-9dc9a99e9b218630598dbcd763074818.jpeg)

2. *Prompt the data you need*

   In the node's configuration panel, write a clear, specific prompt that describes the data you want to retrieve.

   ![Google Maps node prompt field](/assets/images/prompt-ad1e5c2290335244420e248f1cc09551.jpeg)

   Prompting tips

   * MCP nodes only have access to the tools listed so your prompt should be scoped to Google Maps.
   * You can mix and match different tools (e.g., search for gyms in Vancouver → get place details → pull reviews).

3. *Define inputs/outputs*

   Once you’ve entered your prompt, you will go through the AI assisted node creation process starting with defining inputs, parameters and outputs of your Google Maps node.

   Gummie (Gumloop’s AI assistant) will suggest inputs (information coming from previous steps, ie location, keyword, category), parameters (user defined choices, ie number of results or radius) and outputs (information used in future steps, ie business name, review count, ratings).

   ![Suggested inputs, parameters, outputs](/assets/images/input-outputs-52e633dc3b986bef4878f771dc66e580.jpeg)

4. *Generate and test the node*

   With inputs, parameters and outputs defined, Gummie will generate the code to retrieve the data from the Apify Google Maps node. There is no requirement for you to review or understand the code, Gummie takes care of it.

   After the code is written, enter test values to confirm the outputs of the node and save.

   Once saved, you can access this node in any of your flows.

   ![Google Maps node with test values and outputs](/assets/images/node-5166eee64175fe0c8d797176867dea76.jpeg)

## Other integrations

* [TikTok](https://docs.apify.com/integrations/gumloop/tiktok.md)
* [Instagram](https://docs.apify.com/integrations/gumloop/instagram.md)
* [YouTube](https://docs.apify.com/integrations/gumloop/youtube.md)
