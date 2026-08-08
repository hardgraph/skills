---
title: LlamaIndex integration
url: https://docs.apify.com/integrations/llama-index.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [AI](https://docs.apify.com/integrations/ai.md)
previous: [Lindy](https://docs.apify.com/integrations/lindy.md)
next: [Manus](https://docs.apify.com/integrations/manus.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# LlamaIndex integration

> For more information on LlamaIndex, visit its [documentation](https://developers.llamaindex.ai/python/framework/).

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## What is LlamaIndex?

LlamaIndex is a platform that allows you to create and manage vector databases and LLMs.

## How to integrate Apify with LlamaIndex?

You can integrate Apify dataset or Apify Actor with LlamaIndex.

Before we start with the integration, we need to install all dependencies:

`pip install apify-client llama-index-core llama-index-readers-apify`

After successfully installing all dependencies, we can start writing Python code.

### Apify Actor

To use the Apify Actor, import `ApifyActor` and `Document`, and set your [Apify API token](https://docs.apify.com/integrations/api#api-token) in the code. The following example uses the [Website Content Crawler](https://apify.com/apify/website-content-crawler) Actor to crawl an entire website, which will extract text content from the web pages. The extracted text is formatted as a llama\_index `Document` and can be fed to a vector store or language model like GPT.


```python
from llama_index.core import Document

from llama_index.readers.apify import ApifyActor



reader = ApifyActor("<My Apify API token>")



documents = reader.load_data(

    actor_id="apify/website-content-crawler",

    run_input={

        "startUrls": [{"url": "https://docs.llamaindex.ai/en/latest/"}]

    },

    dataset_mapping_function=lambda item: Document(

        text=item.get("text"),

        metadata={

            "url": item.get("url"),

        },

    ),

)
```


### Apify Dataset

To download Apify Dataset, import `ApifyDataset` and `Document` and load the dataset using a dataset ID.


```python
from llama_index.core import Document

from llama_index.readers.apify import ApifyDataset



reader = ApifyDataset("<My Apify API token>")

documents = reader.load_data(

    dataset_id="my_dataset_id",

    dataset_mapping_function=lambda item: Document(

        text=item.get("text"),

        metadata={

            "url": item.get("url"),

        },

    ),

)
```


## Resources

* [Apify loaders](https://llamahub.ai/l/readers/llama-index-readers-apify)
* [LlamaIndex documentation](https://developers.llamaindex.ai/python/framework/)
