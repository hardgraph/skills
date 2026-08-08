---
title: 🦜🔘➡️ LangGraph integration
url: https://docs.apify.com/integrations/langgraph.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
previous: [Langflow](https://docs.apify.com/integrations/langflow.md)
next: [Lindy](https://docs.apify.com/integrations/lindy.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# 🦜🔘➡️ LangGraph integration

[LangGraph](https://www.langchain.com/langgraph) is a framework for constructing stateful, multi-agent applications with large language models (LLMs). It allows developers to build complex AI agent workflows that can leverage tools, APIs, and databases. For more details, check out the [LangGraph documentation](https://langchain-ai.github.io/langgraph/).

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## How to use Apify with LangGraph

This guide will demonstrate how to use Apify Actors with LangGraph by building a ReAct agent that will use the [RAG Web Browser](https://apify.com/apify/rag-web-browser) Actor to search Google for TikTok profiles and [TikTok Data Extractor](https://apify.com/clockworks/free-tiktok-scraper) Actor to extract data from the TikTok profiles to analyze the profiles.

### Prerequisites

* **Apify API token**: To use Apify Actors in LangGraph, you need an Apify API token. If you don't have one, you can learn how to obtain it in the [Apify documentation](https://docs.apify.com/integrations/api).

* **OpenAI API key**: In order to work with agents in LangGraph, you need an OpenAI API key. If you don't have one, you can get it from the [OpenAI platform](https://.openai.com/account/api-keys).

* **Python packages**: You need to install the following Python packages:


  ```bash
  pip install langgraph langchain-apify langchain-openai
  ```


### Build the TikTok profile search and analysis agent

First, import all required packages:


```python
import os



from langchain_apify import ApifyActorsTool

from langchain_core.messages import HumanMessage

from langchain_openai import ChatOpenAI

from langgraph.prebuilt import create_react_agent
```


Next, set the environment variables for the Apify API token and OpenAI API key:


```python
os.environ["OPENAI_API_KEY"] = "Your OpenAI API key"

os.environ["APIFY_API_TOKEN"] = "Your Apify API token"
```


Instantiate LLM and Apify Actors tools:


```python
llm = ChatOpenAI(model="gpt-4o-mini")



browser = ApifyActorsTool("apify/rag-web-browser")

tiktok = ApifyActorsTool("clockworks/free-tiktok-scraper")
```


Create the ReAct agent with the LLM and Apify Actors tools:


```python
tools = [browser, tiktok]

agent_executor = create_react_agent(llm, tools)
```


Finally, run the agent and stream the messages:


```python
for state in agent_executor.stream(

    stream_mode="values",

    input={

        "messages": [

            HumanMessage(content="Search the web for OpenAI TikTok profile and analyze their profile.")

        ]

    }):

    state["messages"][-1].pretty_print()
```


Search and analysis may take some time

The agent tool call may take some time as it searches the web for OpenAI TikTok profiles and analyzes them.

You will see the agent's messages in the console, which will show each step of the agent's workflow.


```text
================================ Human Message =================================



Search the web for OpenAI TikTok profile and analyze their profile.

================================== AI Message ==================================

Tool Calls:

  apify_actor_apify_rag-web-browser (call_y2rbmQ6gYJYC2lHzWJAoKDaq)

 Call ID: call_y2rbmQ6gYJYC2lHzWJAoKDaq

  Args:

    run_input: {"query":"OpenAI TikTok profile","maxResults":1}



...



================================== AI Message ==================================



The OpenAI TikTok profile is titled "OpenAI (@openai) Official." Here are some key details about the profile:



- **Followers**: 592.3K

- **Likes**: 3.3M

- **Description**: The profile features "low key research previews" and includes videos that showcase their various projects and research developments.



### Profile Overview:

- **Profile URL**: [OpenAI TikTok Profile](https://www.tiktok.com/@openai?lang=en)

- **Content Focus**: The posts primarily involve previews of OpenAI's research and various AI-related innovations.



...
```


If you want to test the whole example, you can simply create a new file, `langgraph_integration.py`, and copy the whole code into it.


```python
import os



from langchain_apify import ApifyActorsTool

from langchain_core.messages import HumanMessage

from langchain_openai import ChatOpenAI

from langgraph.prebuilt import create_react_agent



os.environ["OPENAI_API_KEY"] = "Your OpenAI API key"

os.environ["APIFY_API_TOKEN"] = "Your Apify API token"



llm = ChatOpenAI(model="gpt-4o-mini")



browser = ApifyActorsTool("apify/rag-web-browser")

tiktok = ApifyActorsTool("clockworks/free-tiktok-scraper")



tools = [browser, tiktok]

agent_executor = create_react_agent(llm, tools)



for state in agent_executor.stream(

    stream_mode="values",

    input={

        "messages": [

            HumanMessage(content="Search the web for OpenAI TikTok profile and analyze their profile.")

        ]

    }):

    state["messages"][-1].pretty_print()
```


## Resources

* [Apify Actors](https://docs.apify.com/actors)
* [LangGraph - How to Create a ReAct Agent](https://langchain-ai.github.io/langgraph/how-tos/create-react-agent/)
