---
title: Agno Integration
url: https://docs.apify.com/integrations/agno.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
previous: [AI](https://docs.apify.com/integrations/ai.md)
next: [Amazon Bedrock](https://docs.apify.com/integrations/aws_bedrock.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Agno Integration

[Agno](https://docs.agno.com/) is an open-source framework for building intelligent AI agents. It provides a flexible architecture to create agents with custom tools, enabling seamless integration with external services like Apify for tasks such as web scraping, data extraction, and automation. Check out the [Agno documentation](https://docs.agno.com/introduction) for more details on building AI agents.

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## How to use Apify with Agno

This guide shows how to integrate Apify Actors with Agno to empower your AI agents with real-time web data. We'll use the [RAG Web Browser](https://apify.com/apify/rag-web-browser) Actor to fetch web content and the [Google Places Crawler](https://apify.com/compass/crawler-google-places) Actor to extract location-based data. It is very easy to use with any other Actor by just passing the name of the Actor. See and choose from thousands of Actors in the [Apify Store](https://apify.com/store).

### Prerequisites

* *Apify API token*: Obtain your API token from the [Apify Console](https://console.apify.com/settings/integrations).
* *OpenAI API key*: Get your API key from the [OpenAI platform](https://.openai.com/account/api-keys).

Alternative LLM providers

While our examples use OpenAI, Agno supports other LLM providers as well. You'll need to adjust the environment variables and configuration according to your chosen provider. Check out the [Agno models documentation](https://docs.agno.com/models/introduction) for details on supported providers and configuration.

* *Python environment*: Ensure Python is installed (version 3.8+ recommended).
* *Required packages*: Install the following dependencies in your terminal:


```bash
pip install agno apify-client
```


## Basic integration example

Start by setting up an Agno agent with Apify tools. This example uses the RAG Web Browser Actor to extract content from a specific URL.


```python
import os



from agno.agent import Agent

from agno.tools.apify import ApifyTools



os.environ["APIFY_API_TOKEN"] = "YOUR_APIFY_API_TOKEN"  # Replace with your Apify API token

os.environ["OPENAI_API_KEY"] = "YOUR_OPENAI_API_KEY"  # Replace with your OpenAI API key



# Initialize the agent with Apify tools

agent = Agent(

    tools=[ApifyTools( actors=["apify/rag-web-browser"])],

    show_tool_calls=True,

    markdown=True

)



# Fetch and display web content

agent.print_response("Extract key details from https://docs.agno.com/introduction", markdown=True)
```


Running this code will scrape the specified URL and return formatted content your agent can use.

### Advanced scenario: Travel planning agent

Combine multiple Apify Actors to create a powerful travel planning agent. This example uses the RAG Web Browser and Google Places Crawler to gather travel insights and local business data.


```python
import os



from agno.agent import Agent

from agno.tools.apify import ApifyTools



os.environ["APIFY_API_TOKEN"] = "YOUR_APIFY_API_TOKEN"  # Replace with your Apify API token

os.environ["OPENAI_API_KEY"] = "YOUR_OPENAI_API_KEY"  # Replace with your OpenAI API key



# Create a travel planning agent

agent = Agent(

    name="Travel Planner",

    instructions=[

        "You are a travel planning assistant. Use web data and location insights to provide detailed travel recommendations."

    ],

    tools=[

        ApifyTools(

            actors=[

                "apify/rag-web-browser",           # For general web research

                "compass/crawler-google-places"    # For location-based data

            ]

        )

    ],

    show_tool_calls=True,

    markdown=True

)



# Plan a trip to Tokyo

agent.print_response(

    """

    I'm traveling to Tokyo next month.

    1. Research the best time to visit and top attractions.

    2. Find a highly rated sushi restaurant near Shinjuku.

    Compile a travel guide with this information.

    """,

    markdown=True

)
```


This agent will fetch travel-related data and restaurant recommendations, providing a comprehensive travel guide:

1. Use the RAG Web Browser to research Tokyo travel details.
2. Use the Google Places Crawler to find a top sushi restaurant.
3. Combine the results into a comprehensive guide.

Apify Store

Browse the [Apify Store](https://apify.com/store) to find additional Actors for tasks like social media scraping, e-commerce data extraction, or news aggregation.

### Available Apify tools

Agno supports any Apify Actor via the ApifyTools class. You can specify a single Actor ID or a list of Actor IDs to register multiple tools for your agent at once.

## Configuration options

`apify_api_token` (string, default: `None`) : Apify API token (or set via APIFY\_API\_TOKEN environment variable)

`actors` (string or List\[string], default: `None`) : Single Actor ID or list of Actor IDs to register

## Resources

* [How to build an AI agent](https://blog.apify.com/how-to-build-an-ai-agent/)
* [Agno framework documentation](https://docs.agno.com)
* [Apify Actor documentation](https://docs.apify.com/actors.md)
* [Apify Store - browse available Actors](https://apify.com/store)
* [Agno Apify toolkit documentation](https://docs.agno.com/tools/toolkits/others/apify#apify)
