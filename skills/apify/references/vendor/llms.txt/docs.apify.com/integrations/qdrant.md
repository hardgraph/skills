---
title: Qdrant integration
url: https://docs.apify.com/integrations/qdrant.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
previous: [Pinecone](https://docs.apify.com/integrations/pinecone.md)
next: [Skyfire](https://docs.apify.com/integrations/skyfire.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Qdrant integration

[Qdrant](https://qdrant.tech) is a high performance managed vector database that allows users to store and query dense vectors for next generation AI applications such as recommendation systems, semantic search, and retrieval augmented generation (RAG).

The Apify integration for Qdrant enables you to export results from Apify Actors and Dataset items into a specific Qdrant collection.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Prerequisites

Before you begin, ensure that you have the following:

* A [Qdrant cluster](https://qdrant.tech) set up.
* A Qdrant URL to the database and Qdrant API token.
* An [OpenAI API key](https://openai.com/index/openai-api/) to compute text embeddings.
* An [Apify API token](https://docs.apify.com/integrations/api#api-token) to access [Apify Actors](https://apify.com/store).

### How to setup Qdrant database and create a cluster

1. Sign up or log in to your Qdrant account and create a new cluster.

2. Specify the following details: provider, region, and name.

3. Set up an API key for the cluster once it is created and its status is healthy.

With the cluster ready and its URL and API key in hand, you can proceed with integrating Apify.

### Integration Methods

You can integrate Apify with Qdrant using either Apify Console or the Apify Python SDK.

Website Content Crawler usage

The examples utilize the Website Content Crawler Actor, which deeply crawls websites, cleans HTML by removing modals and navigation elements, and converts HTML to Markdown for training AI models or providing web content to LLMs and generative AI applications.

#### Apify Console

1. Set up the [Website Content Crawler](https://apify.com/apify/website-content-crawler) Actor in the [Apify Console](https://console.apify.com). Refer to this guide on how to set up [website content crawl for your project](https://blog.apify.com/talk-to-your-website-with-large-language-models/).

2. Once you have the crawler ready, navigate to the integration section and add Apify's Qdrant integration.

   ![Website Content Crawler with Qdrant integration](/assets/images/qdrant-wcc-integration-ddbf59326d334b6c38da23b8d1de4d42.png)

3. Select when to trigger this integration (typically when a run succeeds) and fill in all the required fields for the Qdrant integration. If you haven't created a collection, it can be created automatically with the specified model. You can learn more about the input parameters at the [Qdrant integration input schema](https://apify.com/apify/qdrant-integration).

   ![Qdrant integration configuration](/assets/images/qdrant-integration-setup-15d4bade6cb47bc451d6968e66668a88.png)

* For a detailed explanation of the input parameters, including dataset settings, incremental updates, and examples, see the [Qdrant integration description](https://apify.com/apify/qdrant-integration).

* For an explanation on how to combine Actors to accomplish more complex tasks, refer to the guide on [Actor-to-Actor](https://blog.apify.com/connecting-scrapers-apify-integration/) integrations.

#### Python

Another way to interact with Qdrant is through the [Apify Python SDK](https://docs.apify.com/sdk/python/).

1. Install the Apify Python SDK by running the following command:


   ```py
   pip install apify-client
   ```


2. Create a Python script and import all the necessary modules:


   ```python
   from apify_client import ApifyClient



   APIFY_API_TOKEN = "YOUR-APIFY-TOKEN"

   OPENAI_API_KEY = "YOUR-OPENAI-API-KEY"



   QDRANT_URL = "YOUR-QDRANT-URL"

   QDRANT_API_KEY = "YOUR-QDRANT-API-KEY"

   QDRANT_COLLECTION_NAME = "YOUR-QDRANT-COLLECTION-NAME"



   client = ApifyClient(APIFY_API_TOKEN)
   ```


3. Call the [Website Content Crawler](https://apify.com/apify/website-content-crawler) Actor to crawl the Qdrant documentation and extract text content from the web pages:


   ```python
   actor_call = client.actor("apify/website-content-crawler").call(

       run_input={"startUrls": [{"url": "https://qdrant.tech/documentation/"}]}

   )
   ```


4. Call Apify's Qdrant integration and store all data in the Qdrant Vector Database:


   ```python
   qdrant_integration_inputs = {

       "qdrantUrl": QDRANT_URL,

       "qdrantApiKey": QDRANT_API_KEY,

       "qdrantCollectionName": QDRANT_COLLECTION_NAME,

       "qdrantAutoCreateCollection": True,

       "datasetId": actor_call["defaultDatasetId"],

       "datasetFields": ["text"],

       "enableDeltaUpdates": True,

       "deltaUpdatesPrimaryDatasetFields": ["url"],

       "deleteExpiredObjects": True,

       "expiredObjectDeletionPeriodDays": 30,

       "embeddingsProvider": "OpenAI",

       "embeddingsApiKey": OPENAI_API_KEY,

       "performChunking": True,

       "chunkSize": 1000,

       "chunkOverlap": 0,

   }

   actor_call = client.actor("apify/qdrant-integration").call(run_input=qdrant_integration_inputs)
   ```


You have successfully integrated Apify with Qdrant and the data is now stored in the Qdrant vector database.

## Additional Resources

* [Apify Qdrant integration](https://apify.com/apify/qdrant-integration)
* [Qdrant documentation](https://qdrant.tech/documentation/)
