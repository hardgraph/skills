# Quick start

Copy for LLM

Learn how to authenticate, run Actors, and retrieve results using the Apify API client for Python.

***

## Step 1: Authenticate the client[](#step-1-authenticate-the-client)

To use the client, you need an [API token](https://docs.apify.com/platform/integrations/api#api-token). You can find your token under the [Integrations](https://console.apify.com/account/integrations) tab in Apify Console. Copy the token and initialize the client by providing it (`MY-APIFY-TOKEN`) as a parameter to the `ApifyClient` constructor.

* Async client
* Sync client

```
from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    # Client initialization with the API token.

    apify_client = ApifyClientAsync(TOKEN)
```

```
from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    # Client initialization with the API token.

    apify_client = ApifyClient(TOKEN)
```

Secure access

The API token is used to authorize your requests to the Apify API. You can be charged for the usage of the underlying services, so do not share your API token with untrusted parties or expose it on the client side of your applications.

## Step 2: Run an Actor[](#step-2-run-an-actor)

To start an Actor, call the [`apify_client.actor()`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ActorClient.md) method with the Actor's ID (e.g., `john-doe/my-cool-actor`). The Actor's ID is a combination of the Actor owner's username and the Actor name. You can run both your own Actors and Actors from [Apify Store](https://apify.com/store).

To define the Actor's input, pass a dictionary to the [`call()`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ActorClient.md#call) method that matches the Actor's [input schema](https://docs.apify.com/platform/actors/development/actor-definition/input-schema). The input can include URLs to scrape, search terms, or other configuration data.

* Async client
* Sync client

```
from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(TOKEN)

    actor_client = apify_client.actor('username/actor-name')



    # Define the input for the Actor.

    run_input = {

        'some': 'input',

    }



    # Start an Actor and wait for it to finish.

    call_result = await actor_client.call(run_input=run_input)
```

```
from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    apify_client = ApifyClient(TOKEN)

    actor_client = apify_client.actor('username/actor-name')



    # Define the input for the Actor.

    run_input = {

        'some': 'input',

    }



    # Start an Actor and wait for it to finish.

    call_result = actor_client.call(run_input=run_input)
```

## Step 3: Get results from the dataset[](#step-3-get-results-from-the-dataset)

To get the results from the dataset, call the [`apify_client.dataset()`](https://docs.apify.com/api/client/python/api/client/python/reference/class/DatasetClient.md) method with the dataset ID, then call [`list_items()`](https://docs.apify.com/api/client/python/api/client/python/reference/class/DatasetClient.md#list_items) to retrieve the data. You can get the dataset ID from the `default_dataset_id` attribute of the [`Run`](https://docs.apify.com/api/client/python/api/client/python/reference/class/Run.md) object returned by `call()`.

* Async client
* Sync client

```
from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(TOKEN)

    dataset_client = apify_client.dataset('dataset-id')



    # Lists items from the Actor's dataset.

    dataset_items = (await dataset_client.list_items()).items
```

```
from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    apify_client = ApifyClient(TOKEN)

    dataset_client = apify_client.dataset('dataset-id')



    # Lists items from the Actor's dataset.

    dataset_items = dataset_client.list_items().items
```

Dataset access

Running an Actor might take time, depending on the Actor's complexity and the amount of data it processes. If you want only to get data and have an immediate response, you should access the existing dataset of the finished [Actor run](https://docs.apify.com/platform/actors/running/runs-and-builds#runs).

## Next steps[](#next-steps)

### Concepts[](#concepts)

To learn more about how the client works, check out the Concepts section in the sidebar:

* [Asyncio support](https://docs.apify.com/api/client/python/api/client/python/docs/concepts/asyncio-support.md) - asynchronous programming with the client
* [Single and collection clients](https://docs.apify.com/api/client/python/api/client/python/docs/concepts/single-and-collection-clients.md) - resource clients and collection clients
* [Error handling](https://docs.apify.com/api/client/python/api/client/python/docs/concepts/error-handling.md) - automatic data extraction and error debugging
* [Retries](https://docs.apify.com/api/client/python/api/client/python/docs/concepts/retries.md) - automatic retries with exponential backoff
* [Pagination](https://docs.apify.com/api/client/python/api/client/python/docs/concepts/pagination.md) - iterating through large result sets

### Guides[](#guides)

For practical examples of common tasks, see the Guides section:

* [Pass input to an Actor](https://docs.apify.com/api/client/python/api/client/python/docs/guides/passing-input-to-actor.md)
* [Retrieve Actor data](https://docs.apify.com/api/client/python/api/client/python/docs/guides/retrieve-actor-data.md)
* [Integrate with data libraries](https://docs.apify.com/api/client/python/api/client/python/docs/guides/integration-with-data-libraries.md)
