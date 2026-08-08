---
title: Pinecone integration
url: https://docs.apify.com/integrations/pinecone.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
previous: [OpenClaw](https://docs.apify.com/integrations/openclaw.md)
next: [Qdrant](https://docs.apify.com/integrations/qdrant.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Pinecone integration

[Pinecone](https://www.pinecone.io) is a managed vector database that allows users to store and query dense vectors for AI applications such as recommendation systems, semantic search, and retrieval augmented generation (RAG).

The Apify integration for Pinecone enables you to export results from Apify Actors and Dataset items into a specific Pinecone vector index.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Prerequisites

Before you begin, ensure that you have the following:

* A [Pinecone database](https://www.pinecone.io/) and index set up.
* A Pinecone index created & Pinecone API token obtained.
* An [OpenAI API key](https://openai.com/index/openai-api/) to compute text embeddings.
* An [Apify API token](https://docs.apify.com/integrations/api#api-token) to access [Apify Actors](https://apify.com/store).

### How to setup Pinecone database and create an index

1. Sign up or log in to your Pinecone account and click on **Create Index**.

2. Specify the following details: index name, vector dimension, vector distance metric, deployment type (serverless or pod), and cloud provider.

   ![Pinecone index configuration](/assets/images/pinecone-create-index-dc7488389754a28dbec882c2847fd6f4.png)

Once the index is created and ready, you can proceed with integrating Apify.

### Integration Methods

You can integrate Apify with Pinecone using either Apify Console or the Apify Python SDK.

Website Content Crawler usage

The examples utilize the Website Content Crawler Actor, which deeply crawls websites, cleans HTML by removing modals and navigation elements, and converts HTML to Markdown for training AI models or providing web content to LLMs and generative AI applications.

#### Apify Console

1. Set up the [Website Content Crawler](https://apify.com/apify/website-content-crawler) Actor in the [Apify Console](https://console.apify.com). Refer to this guide on how to set up [website content crawl for your project](https://blog.apify.com/talk-to-your-website-with-large-language-models/).

2. Once you have the crawler ready, navigate to the integration section and add Apify’s Pinecone integration.

   ![Website Content Crawler with Pinecone integration](/assets/images/pinecone-wcc-integration-89c3c509adf40b11a797d92c71a4e799.png)

3. Select when to trigger this integration (typically when a run succeeds) and fill in all the required fields for the Pinecone integration. You can learn more about the input parameters at the [Pinecone integration input schema](https://apify.com/apify/pinecone-integration/input-schema).

   ![Pinecone integration configuration](/assets/images/pinecone-integration-setup-f054e98ba2a9bf2f31afa32ee5151d51.png)

Pinecone index configuration

You need to ensure that your embedding model in the Pinecone index configuration matches the Actor settings. For example, the `text-embedding-3-small` model from OpenAI generates vectors of size `1536`, so your Pinecone index should be configured for vectors of the same size.

* For a detailed explanation of the input parameters, including dataset settings, incremental updates, and examples, see the [Pinecone integration description](https://apify.com/apify/pinecone-integration).

* For an explanation on how to combine Actors to accomplish more complex tasks, refer to the guide on [Actor-to-Actor](https://blog.apify.com/connecting-scrapers-apify-integration/) integrations.

#### Python

Another way to interact with Pinecone is through the [Apify Python SDK](https://docs.apify.com/sdk/python/).

1. Install the Apify Python SDK by running the following command:

   `pip install apify-client`

2. Create a Python script and import all the necessary modules:


   ```python
   from apify_client import ApifyClient



   APIFY_API_TOKEN = "YOUR-APIFY-TOKEN"

   OPENAI_API_KEY = "YOUR-OPENAI-API-KEY"

   PINECONE_API_KEY = "YOUR-PINECONE-API-KEY"

   PINECONE_INDEX_NAME = "YOUR-PINECONE-INDEX-NAME"



   client = ApifyClient(APIFY_API_TOKEN)
   ```


3. Call the [Website Content Crawler](https://apify.com/apify/website-content-crawler) Actor to crawl the Pinecone documentation and extract text content from the web pages:


   ```python
   actor_call = client.actor("apify/website-content-crawler").call(

       run_input={"startUrls": [{"url": "https://docs.pinecone.io/home"}]}

   )



   print("Website Content Crawler Actor has finished")

   print(actor_call)
   ```


4. Use Apify's [Pinecone integration](https://apify.com/apify/pinecone-integration) to store all the selected data from the dataset (provided by `datasetId` from the Actor call) into the Pinecone vector database.


   ```python
   pinecone_integration_inputs = {

       "pineconeApiKey": PINECONE_API_KEY,

       "pineconeIndexName": PINECONE_INDEX_NAME,

       "datasetFields": ["text"],

       "datasetId": actor_call["defaultDatasetId"],

       "enableDeltaUpdates": True,

       "deltaUpdatesPrimaryDatasetFields": ["url"],

       "deleteExpiredObjects": True,

       "expiredObjectDeletionPeriodDays": 30,

       "embeddingsApiKey": OPENAI_API_KEY,

       "embeddingsProvider": "OpenAI",

       "performChunking": True,

       "chunkSize": 1000,

       "chunkOverlap": 0,

   }



   actor_call = client.actor("apify/pinecone-integration").call(run_input=pinecone_integration_inputs)

   print("Apify's Pinecone Integration has finished")

   print(actor_call)
   ```


You have successfully integrated Apify with Pinecone and the data is now stored in the Pinecone vector database.

## Additional Resources

* [Apify Pinecone integration](https://apify.com/apify/pinecone-integration)
* [What is Pinecone and why use it with your LLMs?](https://blog.apify.com/what-is-pinecone-why-use-it-with-llms/)
* [Pinecone documentation](https://docs.pinecone.io/)
