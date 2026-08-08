# Quick start

Copy for LLM

Learn how to authenticate, run Actors, and retrieve results using the Apify API client for JavaScript.

***

## Step 1: Authenticate the client[](#step-1-authenticate-the-client)

To use the client, you need an [API token](https://docs.apify.com/platform/integrations/api#api-token). You can find your token under [Integrations](https://console.apify.com/account/integrations) tab in Apify Console. Copy the token and initialize the client by providing the token (`MY-APIFY-TOKEN`) as a parameter to the `ApifyClient` constructor.

```
import { ApifyClient } from 'apify-client';



// Client initialization with the API token

const client = new ApifyClient({

    token: 'MY-APIFY-TOKEN',

});
```

Secure access

The API token is used to authorize your requests to the Apify API. You can be charged for the usage of the underlying services, so do not share your API token with untrusted parties or expose it on the client side of your applications.

## Step 2: Run an Actor[](#step-2-run-an-actor)

To start an Actor, call the [`client.actor()`](https://docs.apify.com/api/client/js/api/client/js/reference/class/ActorClient.md) method with the Actor's ID (e.g. `john-doe/my-cool-actor`). The Actor's ID is a combination of the Actor owner's username and the Actor name. You can run both your own Actors and Actors from Apify Store.

To define the Actor's input, pass a JSON object to the [`call()`](https://docs.apify.com/api/client/js/api/client/js/reference/class/ActorClient.md#call) method that matches the Actor's [input schema](https://docs.apify.com/platform/actors/development/actor-definition/input-schema). The input can include URLs to scrape, search terms, or other configuration data.

```
import { ApifyClient } from 'apify-client';



const client = new ApifyClient({ token: 'MY-APIFY-TOKEN' });



// Runs an Actor with an input and waits for it to finish.

const { defaultDatasetId } = await client.actor('username/actor-name').call({

    some: 'input',

});
```

## Step 3: Get results from the dataset[](#step-3-get-results-from-the-dataset)

To get the results from the dataset, call the [`client.dataset()`](https://docs.apify.com/api/client/js/api/client/js/reference/class/DatasetClient.md) method with the dataset ID, then call [`listItems()`](https://docs.apify.com/api/client/js/api/client/js/reference/class/DatasetClient.md#listItems) to retrieve the data. You can get the dataset ID from the Actor's run object (represented by `defaultDatasetId`).

```
import { ApifyClient } from 'apify-client';



const client = new ApifyClient({ token: 'MY-APIFY-TOKEN' });



// Runs an Actor and waits for it to finish

const { defaultDatasetId } = await client.actor('username/actor-name').call();



// Lists items from the Actor's dataset

const { items } = await client.dataset(defaultDatasetId).listItems();

console.log(items);
```

Dataset access

Running an Actor might take time, depending on the Actor's complexity and the amount of data it processes. If you want only to get data and have an immediate response you should access the existing dataset of the finished [Actor run](https://docs.apify.com/platform/actors/running/runs-and-builds#runs).

## Next steps[](#next-steps)

### Concepts[](#concepts)

To learn more about how the client works, check out the Concepts section in the sidebar:

* [Usage patterns](https://docs.apify.com/api/client/js/api/client/js/docs/concepts/usage-patterns.md) - resource clients, collection clients, and nested clients
* [Error handling and retries](https://docs.apify.com/api/client/js/api/client/js/docs/concepts/error-handling.md) - automatic retries with exponential backoff
* [Convenience functions](https://docs.apify.com/api/client/js/api/client/js/docs/concepts/convenience-functions.md) - `call()`, `waitForFinish()`, and more
* [Pagination](https://docs.apify.com/api/client/js/api/client/js/docs/concepts/pagination.md) - iterating through large result sets

### Guides[](#guides)

For practical examples of common tasks, see [Code examples](https://docs.apify.com/api/client/js/api/client/js/docs/guides/examples.md).
