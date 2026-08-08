# Overview

Copy for LLM

The Apify API client for Python is the official library to access the [Apify REST API](https://docs.apify.com/api/v2) from your Python applications. It provides useful features like automatic retries and convenience functions that improve the experience of using the Apify API.

The client simplifies interaction with the Apify platform by providing:

* Intuitive methods for working with [Actors](https://docs.apify.com/platform/actors), [Datasets](https://docs.apify.com/platform/storage/dataset), [Key-value stores](https://docs.apify.com/platform/storage/key-value-store), and other Apify resources
* Both synchronous and asynchronous interfaces for flexible integration
* Built-in [retries with exponential backoff](https://docs.apify.com/api/client/python/api/client/python/docs/concepts/retries.md) for failed requests
* Comprehensive API coverage with JSON encoding (UTF-8) for all requests and responses

## Prerequisites[](#prerequisites)

`apify-client` requires Python 3.11 or higher. Python is available for download on the [official website](https://www.python.org/downloads/). Check your current Python version by running:

```
python --version
```

## Installation[](#installation)

The Apify client is available as the `apify-client` package [on PyPI](https://pypi.org/project/apify-client/) or [on conda-forge](https://anaconda.org/conda-forge/apify-client).

* PyPI
* conda-forge

```
pip install apify-client
```

```
conda install conda-forge::apify-client
```

For better request-body compression, opt in to `brotli`, which compresses better than `gzip`, especially for large payloads. Install the optional extra:

* PyPI
* conda-forge

```
pip install "apify-client[brotli]"
```

```
conda install conda-forge::apify-client
```

For details, see [HTTP compression](https://docs.apify.com/api/client/python/api/client/python/docs/concepts/http-compression.md).

## Quick example[](#quick-example)

The following example shows how to run an Actor and retrieve its results:

* Async client
* Sync client

```
from apify_client import ApifyClientAsync



# You can find your API token at https://console.apify.com/settings/integrations.

TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(TOKEN)



    # Start an Actor and wait for it to finish.

    actor_client = apify_client.actor('john-doe/my-cool-actor')

    call_result = await actor_client.call()



    if call_result is None:

        print('Actor run failed.')

        return



    # Fetch results from the Actor run's default dataset.

    dataset_client = apify_client.dataset(call_result.default_dataset_id)

    list_items_result = await dataset_client.list_items()

    print(f'Dataset: {list_items_result}')
```

```
from apify_client import ApifyClient



# You can find your API token at https://console.apify.com/settings/integrations.

TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    apify_client = ApifyClient(TOKEN)



    # Start an Actor and wait for it to finish.

    actor_client = apify_client.actor('john-doe/my-cool-actor')

    call_result = actor_client.call()



    if call_result is None:

        print('Actor run failed.')

        return



    # Fetch results from the Actor run's default dataset.

    dataset_client = apify_client.dataset(call_result.default_dataset_id)

    list_items_result = dataset_client.list_items()

    print(f'Dataset: {list_items_result}')
```

> You can find your API token in the [Integrations section](https://console.apify.com/account/integrations) of Apify Console. See the [Quick start guide](https://docs.apify.com/api/client/python/api/client/python/docs/introduction/quick-start.md) for more details on authentication.
