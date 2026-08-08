---
title: Gumloop - TikTok Actor integration
url: https://docs.apify.com/integrations/gumloop/tiktok.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Workflows and notifications](https://docs.apify.com/integrations/workflows-and-notifications.md)
  - [Gumloop](https://docs.apify.com/integrations/gumloop.md)
previous: [Google Maps](https://docs.apify.com/integrations/gumloop/maps.md)
next: [YouTube](https://docs.apify.com/integrations/gumloop/youtube.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Gumloop - TikTok Actor integration

Get TikTok hashtag videos, profile videos, followers, video details, and search results in Gumloop.

***

The Gumloop TikTok integration provides a native interface for running Apify’s TikTok scrapers directly in your workflows. No API tokens or manual polling required. All you need is a Gumloop account. Using the Gumloop TikTok MCP node, you can simply prompt the TikTok data you need and Gumloop will retrieve it from relevant Apify Actors. From there, you can connect it to your favorite tools and AI agents to process the information.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Available actions

You can pull the following types of data from TikTok using Gumloop’s TikTok node (via Apify). Each action has a credits cost.

| Tool/Action           | Description                                                                                                    | Credit Cost        |
| --------------------- | -------------------------------------------------------------------------------------------------------------- | ------------------ |
| Get hashtag videos    | Fetch videos from TikTok hashtags with captions, engagement metrics, play counts, and author information.      | 3 credits per item |
| Get profile videos    | Get videos from TikTok user profiles with video metadata, engagement stats, music info, and timestamps.        | 3 credits per item |
| Get profile followers | Retrieve followers or following lists from TikTok profiles, including usernames, follower counts, and bios.    | 3 credits per item |
| Get video details     | Get comprehensive data on a specific TikTok video using its URL - includes engagement and video-level metrics. | 5 credits per item |
| Search videos         | Search TikTok for videos and users using queries. Returns video details and user profile info.                 | 3 credits per item |

## Retrieve TikTok data in Gumloop

1. *Add the Gumloop TikTok MCP node*

   First, add the TikTok node from the node library to your workflow canvas.

   ![TikTok MCP node on canvas](/assets/images/mcp-node-image-0cefb39aa4fac930d2638b28f40955cf.jpeg)

2. *Prompt the data you need*

   In the node's configuration panel, write a clear, specific prompt that describes the data you want to retrieve.

   ![TikTok node prompt field](/assets/images/prompt-a2729c35b8e1b9762882812182ebae7f.jpeg)

   Prompting tips

   * MCP nodes only have access to the tools listed so your prompt should be scoped to TikTok.
   * You can mix and match different tools (e.g., search a hashtag → get profile videos → retrieve engagement data).

3. *Define inputs/outputs*

   Once you’ve entered your prompt, you will go through the AI assisted node creation process starting with defining inputs, parameters and outputs of your TikTok node.

   Gummie (Gumloop’s AI assistant) will suggest inputs (information coming from previous steps, ie search keyword, video URL), parameters (user defined choices, number of videos to retrieve) and outputs (information used in future steps, ie follower count, video engagement, music info).

   ![Suggested inputs, parameters, outputs](/assets/images/input-output-ef52e25e5ba2d757b84d44e828cbe6d5.jpeg)

4. *Generate and test the node*

   With inputs, parameters and outputs defined, Gummie will generate the code to retrieve the data from the Apify TikTok node. There is no requirement for you to review or understand the code, Gummie takes care of it.

   After the code is written, enter test values to confirm the outputs of the node and save.

   Once saved, you can access this node in any of your flows.

   ![TikTok node with test values and outputs](/assets/images/node-843d8146ce1351eb43d5824faa27b528.jpeg)

## Other integrations

* [Instagram](https://docs.apify.com/integrations/gumloop/instagram.md)
* [YouTube](https://docs.apify.com/integrations/gumloop/youtube.md)
* [Google Maps](https://docs.apify.com/integrations/gumloop/maps.md)
