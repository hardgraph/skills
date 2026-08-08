---
title: Gumloop - YouTube Actor integration
url: https://docs.apify.com/integrations/gumloop/youtube.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Workflows and notifications](https://docs.apify.com/integrations/workflows-and-notifications.md)
  - [Gumloop](https://docs.apify.com/integrations/gumloop.md)
previous: [TikTok](https://docs.apify.com/integrations/gumloop/tiktok.md)
next: [IFTTT](https://docs.apify.com/integrations/ifttt.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Gumloop - YouTube Actor integration

Get YouTube search results, video details, channel videos, playlists, and channel metadata in Gumloop.

***

The Gumloop YouTube integration provides a native interface for running Apify’s YouTube scrapers directly in your workflows. No API keys or manual polling required. All you need is a Gumloop account.

Using the Gumloop YouTube MCP node, you can simply prompt the YouTube data you need and Gumloop will retrieve it from relevant Apify Actors. From there, you can connect it to your favorite tools and AI agents to process the information.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Available actions

You can pull the following types of data from YouTube using Gumloop’s YouTube node (via Apify). Each action has a credit cost:

| Tool/Action         | Description                                                                                  | Credit Cost       |
| ------------------- | -------------------------------------------------------------------------------------------- | ----------------- |
| Search videos       | Search YouTube by keywords and get video results with filtering, metadata, and content info. | 3 credit per item |
| Get video details   | Retrieve detailed stats and content info for specific videos via URL or ID.                  | 4 credit per item |
| Get channel videos  | Get videos from a specific YouTube channel with full metadata and context.                   | 3 credit per item |
| Get playlist videos | Fetch videos from a YouTube playlist with metadata and playlist details.                     | 3 credit per item |
| Get channel details | Get channel metadata including subscriber count, total videos, description, and more.        | 5 credit per item |

## Retrieve YouTube data in Gumloop

1. *Add the Gumloop YouTube MCP node*

   First, add the YouTube node from the node library to your workflow canvas.

   ![YouTube MCP node in Node Library](/assets/images/mcp-node-image-2c97f2448741970135d0d30b160e3e14.jpeg)

2. *Prompt the data you need*

   In the node's configuration panel, write a clear, specific prompt that describes the data you want to retrieve.

   ![YouTube node prompt field](/assets/images/prompt-426be030e8571386f2a06747ce2ddbc3.jpeg)

   Prompting tips

   * MCP nodes only have access to the tools listed so your prompt should be scoped to YouTube.
   * You can mix and match different tools (e.g., search for videos → get video details → extract channel info).

3. *Define the inputs and outputs*

   Once you’ve entered your prompt, you will go through the AI assisted node creation process starting with defining inputs, parameters and outputs of your YouTube node.

   Gummie (Gumloop’s AI assistant) will suggest inputs (information coming from previous steps, ie search terms, video URLs), parameters (user defined choices, ie number of videos to retrieve) and outputs (information used in future steps, title, view count, channel name).

   ![Suggested inputs, parameters, outputs](/assets/images/input-outputs-cd88353d7de9088ea944ba2d5481ce1b.jpeg)

4. *Generate and test the node*

   With inputs, parameters and outputs defined, Gummie will generate the code to retrieve the data from the Apify YouTube node. There is no requirement for you to review or understand the code, Gummie takes care of it.

   After the code is written, enter test values to confirm the outputs of the node and save.

   Once saved, you can access this node in any of your flows.

   ![YouTube node with test values and outputs](/assets/images/node-8ed508133441d57ca960e7c890ea9e16.jpeg)

## Other integrations

* [TikTok](https://docs.apify.com/integrations/gumloop/tiktok.md)
* [Instagram](https://docs.apify.com/integrations/gumloop/instagram.md)
* [Google Maps](https://docs.apify.com/integrations/gumloop/maps.md)
