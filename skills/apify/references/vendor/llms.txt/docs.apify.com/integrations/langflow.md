---
title: Langflow integration
url: https://docs.apify.com/integrations/langflow.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
previous: [LangChain](https://docs.apify.com/integrations/langchain.md)
next: [LangGraph](https://docs.apify.com/integrations/langgraph.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Langflow integration

[Langflow](https://www.langflow.org/) is a low-code, visual tool that enables developers to build powerful AI agents and workflows that can use any API, models, or databases. For more information, visit the [Langflow documentation](https://docs.langflow.org/).

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## How to use Apify with Langflow

This guide will demonstrate two different ways to use Apify Actors with Langflow:

* **Calling Apify Actors in Langflow**: We will use the [RAG Web Browser](https://apify.com/apify/rag-web-browser) Actor to search Google for a query and extract the search results.
* **Building a flow to search for a company's social media profiles**: We will use the [Google Search Results Scraper](https://apify.com/apify/google-search-scraper) Actor to search the web for social media profiles of a given company. Then, we will use the [TikTok Data Extractor](https://apify.com/clockworks/free-tiktok-scraper) Actor to extract data from the TikTok profiles.

### Prerequisites

* **Apify API token**: To use Apify Actors in Langflow, you need an Apify API token. If you don't have one, you can learn how to get it in the [Apify documentation](https://docs.apify.com/integrations/api).

* **OpenAI API key**: To work with agents in Langflow, you need an OpenAI API key. If you don't have one, you can get it from the [OpenAI platform](https://.openai.com/account/api-keys).

#### Langflow

Cloud vs local setup

Langflow can either be installed locally or used in the cloud. The cloud version is available on the [Langflow](https://www.langflow.org/) website. If you are using the cloud version, you can skip the installation step, and go straight to Creating a new flow

First, install the Langflow platform using Python package and project manager [uv](https://docs.astral.sh/uv/):


```bash
uv pip install langflow
```


After installing Langflow, you can start the platform:


```bash
uv run langflow run
```


When the platform is started, open the Langflow UI using `http://127.0.0.1:7860` in your browser.

> Other installation methods can be found in the [Langflow documentation](https://docs.langflow.org/get-started-installation).

### Create a new flow

On the Langflow welcome screen, click the **New Flow** button and then create **Blank Flow**: ![New Flow screen - Blank Flow](/assets/images/new_blank_flow-8c5272acc3b2bf2b7779caff60c4726b.png)

Now, you can start building your flow.

### Call Apify Actors in Langflow

To call Apify Actors in Langflow, you need to add the **Apify Actors** component to the flow. From the bundle menu, add **Apify Actors** component: ![Flow - Add Apify Actors](/assets/images/bundles_apify-b72b75511bcd261c86b0b998951b77f4.png)

Next, configure the Apify Actors components. First, input your API token (learn how to get it at [Integrations](https://docs.apify.com/integrations/api)). Then, set the Actor ID of the component to `apify/rag-web-browser` to use the [RAG Web Browser](https://apify.com/apify/rag-web-browser). Set the **Run input** field to pass arguments to the Actor run, allowing it to search Google with the query `"what is monero?"` (full Actor input schema can be found in the [RAG Web Browser input schema](https://apify.com/apify/rag-web-browser/input-schema)):


```json
{"query": "what is monero?", "maxResults": 3}
```


Click **Run**. ![Flow - Apify Actors Run](/assets/images/apify_actors_run-27164252e3d8b180c516959224db031f.png)

After the run finishes, click **Output** to view the results. ![Flow - Apify Actors Output](/assets/images/apify_actors_output-66dc9d6763d338a42b6a17db1043e526.png)

The output should look similar to this: ![Flow - Apify Actors Output Data](/assets/images/apify_actors_output_data-beaa74c6c9f876eca0034296d667f489.png)

To filter only the `metadata` and `markdown` fields, set **Output fields** to `metadata,markdown`. Additionally, enable **Flatten output** by setting it to `true`. This will output only the metadata and text content from the search results.

> Flattening is necessary when you need to access nested dictionary fields in the output data object; they cannot be accessed directly otherwise in the Data object.

![Flow - Apify Actors Output Filter](/assets/images/apify_actors_output_filter-670268c7cd24482d2f8f62ae7de8e5ea.png)

When you run the component again, the output contains only the `markdown` and flattened `metadata` fields:

![Flow - Apify Actors Output Filtered](/assets/images/apify_actors_output_data_filtered-09d37d0f2511870d96745f8d772ff8d1.png)

Now that you understand how to call Apify Actors, let's build a practical example where you search for a company's social media profiles and extract data from them.

### Build a flow to search for a company's social media profiles

Create a new flow and add two **Apify Actors** components from the menu.

Input your API token (learn how to get it in the [Integrations documentation](https://docs.apify.com/integrations/api)) and set the Actor ID of the first component to `apify/google-search-scraper` and the second one to `clockworks/free-tiktok-scraper`: ![Flow - Actors configuration](/assets/images/apify_actors_configuration-9024e10771b3242b24afb7a9bfc57687.png)

Add the **Agent** component from the menu and set your OpenAI API key (get it from the [OpenAI API keys page](https://.openai.com/account/api-keys)):

Optimize Agent results

For better results, switch the model to `gpt-4o` instead of `gpt-4o-mini` in the Agent configuration

![Flow - Agent configuration](/assets/images/agent_configuration-56902bb56e84143deff4c3b733823ae4.png)

To be able to interact with the agent, add **Chat Input** and **Chat Output** components from the menu and connect them to the Agent component **Input** and **Response**. Then connect both Apify Actor components **Tool** outputs to the Agent component **Tools** input so that the agent can call the Apify Actors. The final flow that can search the web for a company's social media profiles and extract data from them should look like this: ![Flow - Final](/assets/images/flow-c1424ee0b06e6cc8178ce6aa33a550ed.png)

Click the **Playground** button and chat with the agent to test the flow: ![Flow - Playground](/assets/images/playground-e56729ea661b2a8a089eb3f5084bc0c1.png)

Here is an example agent output for the following query:


```text
find tiktok profile of company openai using google search and then show me the profile bio and their latest video
```


![Flow - agent output](/assets/images/agent_output-5a20a0f099edca9bafadcbda946ea412.png)
