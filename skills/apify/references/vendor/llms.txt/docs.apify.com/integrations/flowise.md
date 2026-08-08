---
title: Flowise integration
url: https://docs.apify.com/integrations/flowise.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
previous: [Cursor](https://docs.apify.com/integrations/cursor.md)
next: [GitHub Copilot](https://docs.apify.com/integrations/github-copilot.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Flowise integration

[Flowise](https://flowiseai.com/) is an open-source UI visual tool to build customized LLM flows using LangChain.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## How to use Apify with Flowise

### Installation

To use Flowise you have to download and run it locally. The quickest way to do so is to use the following commands:

1. To install Flowise globally on your device:


   ```bash
   npm install -g flowise
   ```


2. To start Flowise locally:


   ```bash
   npx flowise start
   ```


It will be available on `https://localhost:3000`

Other methods of using Flowise can be found in their [documentation](https://docs.flowiseai.com/getting-started#quick-start)

### Build your flow

After running Flowise, you can start building your flow with Apify.

The first step is to create a new flow in the web UI.

In the left menu, you need to find Apify Website Content Crawler under Document Loaders.

![Flowise add Apify Crawler](/assets/images/flowise-apify-be24e3ad72927eabe8324296606fbc9e.png)

Now you need to configure the crawler. You can find more information about at [Website Content Crawler page](https://apify.com/apify/website-content-crawler).

![Flowise and Apify](/assets/images/flowise-6aaa0f5e5f9f12324d65667d091b43ea.png)

In the configuration, provide your Apify API token, which you can find in your [Apify account](https://console.apify.com/settings/integrations).

![Apify API token screen](/assets/images/flowise-apify-api-f22034c2739a7ec01b6459b0f630b4a6.png)

You can add more loaders, or you can add some processors to process the data. In our case, we create the flow that loads data from the Apify docs using Website Content Crawler and save them into the in-memory vector database. Connect the ChatOpenAI and the OpenAI embeddings and QA retrieval into the chatbot.

The final flow can answer questions about Apify docs.

![Flowise and Apify](/assets/images/flowise-2-8a54cc439fcc38ba74a1551c6e45bf29.png)

For more information visit the Flowise [documentation](https://flowiseai.com/).

## Resources

* [Flowise](https://flowiseai.com/)
* [Flowise documentation](https://github.com/FlowiseAI/Flowise#quick-start)
