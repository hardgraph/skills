---
title: Running Actors
url: https://docs.apify.com/actors/running.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
children:
  - [Actors in Store](https://docs.apify.com/actors/running/actors-in-store.md)
  - [Input and output](https://docs.apify.com/actors/running/input-and-output.md)
  - [Runs and builds](https://docs.apify.com/actors/running/runs-and-builds.md)
  - [Usage and resources](https://docs.apify.com/actors/running/usage-and-resources.md)
  - [Permissions](https://docs.apify.com/actors/running/permissions.md)
  - [Tasks](https://docs.apify.com/actors/running/tasks.md)
  - [Standby mode](https://docs.apify.com/actors/running/standby.md)
  - [Schedules](https://docs.apify.com/actors/running/schedules.md)
  - [Monitoring](https://docs.apify.com/actors/running/monitoring.md)
previous: [Overview](https://docs.apify.com/actors.md)
next: [Actors in Store](https://docs.apify.com/actors/running/actors-in-store.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Running Actors

## Run your first Apify Actor

To get started, we recommend trying one of the existing Actors from [Apify Store](https://apify.com/store). For details on building your own, see [Actor development](https://docs.apify.com/actors/development.md).

### Prerequisites

To complete this tutorial, you need an Apify account. If you don't have it yet, [sign up for free](https://console.apify.com/sign-up).

### 1. Choose your Actor

To find an Actor in Apify Store:

1. Sign in to [Apify Console](https://console.apify.com).
2. Go to [Apify Store](https://console.apify.com/store).
3. Use the search bar or browse by categories.

For this tutorial, let's choose [Website Content Crawler](https://console.apify.com/actors/aYG0l9s7dbB7j3gbS/information/version-0/readme).

### 2. Configure and run your Actor

Once you select the Actor, you will be taken to the Actor's detail page.

In the **Input** tab, you can customize your Actor's behavior. Website Content Crawler is pre-configured to run without extra input, so you don't need to change anything.

To run the Actor, click **Start**.

![Website Content Crawler in Apify Console. Input tab is open and the Start button is highlighted](/assets/images/configure-and-run-actor-ad32be8b3ff4710df7ab3917cd0ca93d.svg)

### 3. Wait for the results

The Actor might take a while to gather results and finish its run. While waiting, let's explore the remaining options:

* Check the tabs where you can find more information about the Actor run. For example, its logs or storage.
* Use the **API** button to view the related API endpoints.

![Website Content Crawler in Apify Console. Output tab is open and the API and Export buttons are highlighted](/assets/images/results-of-actor-run-ae3f6aeeef07bc0858d2758d8eac09df.svg)

### 4. Save the results

The results of the Actor run appear in the **Output** tab. To save the data, click **Export**. You can choose from multiple formats.

And that's it! You've run your first Actor!

Now you can go back to the **Input** tab and try again with different settings, run other [Apify Actors](https://apify.com/store), or [build your own](https://docs.apify.com/actors/development.md).

## Run Actors with the Apify API

To invoke Actors with the Apify API, send an HTTP POST request to the [Run Actor](https://docs.apify.com/api/v2/actors-runs-post.md) endpoint. For example:


```text
https://api.apify.com/v2/actors/compass~crawler-google-places/runs?token=<YOUR_API_TOKEN>
```


An Actor's input and its content type can be passed as a payload of the POST request, and additional options can be specified using URL query parameters. To learn more, see [Run an Actor and retrieve data via API](https://docs.apify.com/academy/api/run-actor-and-retrieve-data-via-api.md).

## Run Actors programmatically

You can also invoke Actors programmatically from your own applications or from other Actors.

To start an Actor from your own application, we recommend using Apify API client libraries for [JavaScript](https://docs.apify.com/api/client/js/reference/class/ActorClient#call) or [Python](https://docs.apify.com/api/client/python/reference/class/ActorClient#call).

**JavaScript**


```javascript
import { ApifyClient } from 'apify-client';



const client = new ApifyClient({

    token: 'MY-API-TOKEN',

});



// Start the Google Maps Scraper Actor and wait for it to finish.

const actorRun = await client.actor('compass/crawler-google-places').call({

    queries: 'apify',

});

// Fetch scraped results from the Actor's dataset.

const { items } = await client.dataset(actorRun.defaultDatasetId).listItems();

console.dir(items);
```


**Python**


```python
from apify_client import ApifyClient





apify_client = ApifyClient('MY-API-TOKEN')



# Start the Google Maps Scraper Actor and wait for it to finish.

actor_run = apify_client.actor('compass/crawler-google-places').call(

    run_input={ 'queries': 'apify' }

)



# Fetch scraped results from the Actor's dataset.

dataset_items = apify_client.dataset(actor_run['defaultDatasetId']).list_items().items

print(dataset_items)
```


The newly started Actor runs under the account associated with the provided `token`, so all consumed resources are charged to this user account.

Internally, the `call()` function invokes the [Run Actor](https://docs.apify.com/api/v2/actors-runs-post.md) API endpoint, waits for the Actor to finish, and reads its results from the default dataset using the [Get dataset items](https://docs.apify.com/api/v2/dataset-items-get.md) API endpoint.
