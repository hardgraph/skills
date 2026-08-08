---
title: Storage
url: https://docs.apify.com/storage.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
next: [Dataset](https://docs.apify.com/storage/dataset.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Storage

The Apify platform provides three types of storage, accessible from [Apify Console](https://console.apify.com/storage) and externally through the [REST API](https://docs.apify.com/api/v2.md), [API clients](https://docs.apify.com/api.md), and [SDKs](https://docs.apify.com/sdk.md).

## Storage types

#### [Dataset](https://docs.apify.com/storage/dataset.md)

[Stores results from web scraping and data processing; each Actor run gets a unique dataset. Includes table-like visualization and export formats like JSON and Excel.](https://docs.apify.com/storage/dataset.md)

#### [Key-value store](https://docs.apify.com/storage/key-value-store.md)

[Stores data of any type: JSON, HTML, images, strings. Accessible via Apify Console or API.](https://docs.apify.com/storage/key-value-store.md)

#### [Request queue](https://docs.apify.com/storage/request-queue.md)

[Manages URL processing for web crawling and similar tasks. Supports different crawling orders and lets you query and update URLs, accessible via Apify Console or API.](https://docs.apify.com/storage/request-queue.md)

## Access your storage

Access your storage through Apify Console, the API, the API clients, or the SDKs.

### Apify Console

To view your storages in [Apify Console](https://console.apify.com/storage):

1. Open the **Storage** section in the left-side menu.
2. Select a tab to view your key-value stores, datasets, or request queues.
3. Select a storage's **ID** to open its detail page.

To view the related API endpoints, select **API** in the top right corner.

![Storages in app](/assets/images/datasets-app-7f95b1edcb4e2cd28d7885c648820bf0.png)

Toggle unnamed storages

Use the **Include unnamed storages** checkbox to either display or hide unnamed storages. By default Apify Console displays them.

To rename a store, open the **Actions** menu and select **Rename**.

To share a storage, select **Share** in the **Actions** menu and provide an email, username, or user ID.

These URLs link to API *endpoints* where your data is stored. *Read* endpoints don't require an [authentication token](https://docs.apify.com/api/v2.md#authentication). Calls are authenticated by a hard-to-guess ID, which keeps sharing secure. Operations such as *update* or *delete* do require the token.

Token security

Never share a URL containing your authentication token. It can compromise your account's security. If the data you want to share requires a token, download it first and share it as a file.

### Apify API

The [Apify API](https://docs.apify.com/api/v2/storage-key-value-stores.md) lets you access your storages programmatically using [HTTP requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) and share your crawling results.

When accessing storages via API, provide a `store ID` in one of these formats:

* `WkzbQMuFYuamGv3YF` - the store's alphanumerical ID if the store is unnamed.
* `~store-name` - the store's name prefixed with tilde (`~`) character if the store is named (e.g. `~ecommerce-scraping-results`)
* `username~store-name` - username and the store's name separated by a tilde (`~`) character if the store is named and belongs to a different account (e.g. `janedoe~ecommerce-scraping-results`). Note that in this case, the store's owner needs to grant you access first.

For read (GET) requests, the alphanumerical ID alone is enough, since it's hard to guess and serves as an authentication key.

For other request types, and when using `username~store-name`, provide your secret API token in the request's [Authorization](https://docs.apify.com/api/v2.md#authentication) header or as a query parameter. Find your token on the [API & Integrations](https://console.apify.com/settings/integrations) page of your Apify account.

For a breakdown of each storage endpoint, see the [API documentation](https://docs.apify.com/api/v2/storage-datasets.md).

### Apify API clients

The Apify API clients let you access your storages from any Node.js or Python application, whether it runs on the Apify platform or externally.

For more details, see the [API client docs](https://docs.apify.com/api.md).

### Apify SDKs

The Apify SDKs are JavaScript and Python libraries for building your own Actors.

* JavaScript SDK requires [Node.js](https://nodejs.org/en/) 16 or later.
* Python SDK requires [Python](https://www.python.org/downloads/release/python-380/) 3.8 or above.

## Named and unnamed storages

The default storages for an Actor run are unnamed, identified only by an *ID*. Naming a storage ensures indefinite retention regardless of plan; unnamed storages follow the data retention rules below.

Named and unnamed storages are identical except for their retention period. Named storages are easier to identify and confirm. The names `janedoe~my-storage-1` and `janedoe~web-scrape-results` are easier to tell apart than the IDs `cAbcYOfuXemTPwnIB` and `CAbcsuZbp7JHzkw1B`. Storage names can be up to 63 characters long.

### Name a storage

You can name a storage via Apify Console or through the API.

In Apify Console:

1. Open your run's details and select the **Dataset**, **Key-value store**, or **Request queue** tab as appropriate.

2. Find the store's ID:

   ![Finding your store\&#39;s ID](/assets/images/find-store-id-0c95342b8b520433938455a67069f81e.png)

3. Click the ID to open the storage details.

4. Click on the **Actions** menu and choose **Rename**.

5. Enter a new name. Your storage is now preserved indefinitely.

Via API: get the storage's ID from the run that generated it using the [Get run](https://docs.apify.com/api/v2/actor-run-get.md) endpoint, then rename it using the `Update [storage]` endpoint (for example, [Update dataset](https://docs.apify.com/api/v2/dataset-put.md)).

The SDKs and clients each have their own naming conventions. See:

* [SDKs](https://docs.apify.com/sdk.md)
* [API clients](https://docs.apify.com/api.md)

## Data retention

How long Apify keeps your data depends on your plan. Unnamed storages and runs beyond the 10 most recent are deleted automatically; named storages are retained indefinitely.

How retention periods are enforced

* Free plan: Your 10 most recent runs are retained for 4 months.
* Paid plans: All data (including your 10 most recent runs) follows your plan's retention period, which you can configure in your billing settings.
* Named storages: Always exempt from deletion regardless of retention periods.

Unnamed storages beyond the 10 most recent runs are deleted when the retention period expires.

## Estimate your costs

Use this tool to estimate storage costs by plan and storage type.

Estimate your storage costs

1. Select a storage type.
2. Choose a plan.
3. Enter storage, duration, and operation counts.
4. Review the estimated total and breakdown.

### Storage Pricing

Estimate costs for your storage usage

This is an estimate

This is an estimate based on current pricing. Actual costs may vary.

#### Storage Type

\[x]

**Dataset**Stores results from web scraping and data processing

\[ ]

**Key-value Store**Stores various data types like JSON, HTML, images, and strings

\[ ]

**Request Queue**Manages URL processing for web crawling and other tasks

#### Plan

\[x]

**Free/Starter**$0/month & $29/month

\[ ]

**Scale**$199/month

\[ ]

**Business**$999/month

#### Usage

Storage (GB)1

Reads (count)1000

Duration (hours)24

Writes (count)1000

#### Estimated Costs

Storage (

<!-- -->

1

<!-- -->

GB ×

<!-- -->

24

<!-- -->

hours)$

<!-- -->

0.0240

Reads (

<!-- -->

1,000

<!-- -->

operations)$

<!-- -->

0.0004

Writes (

<!-- -->

1,000

<!-- -->

operations)$

<!-- -->

0.0050

**Total Estimated Cost****$<!-- -->0.0294**

## Rate limiting

All API endpoints limit their request rate to protect Apify servers from overload. The default rate limit for storage objects is *60 requests per second*. However, there are exceptions limited to *400 requests per second* per storage object, including:

* [Push items](https://docs.apify.com/api/v2/dataset-items-post.md) to dataset.
* CRUD ([add](https://docs.apify.com/api/v2/request-queue-requests-post.md), [get](https://docs.apify.com/api/v2/request-queue-request-get.md), [update](https://docs.apify.com/api/v2/request-queue-request-put.md), [delete](https://docs.apify.com/api/v2/request-queue-request-delete.md)) operations of *request queue* requests.

If a client exceeds this limit, the API endpoints respond with the HTTP status code `429 Too Many Requests` and the following body:


```json
{

    "error": {

        "type": "rate-limit-exceeded",

        "message": "You have exceeded the rate limit of ... requests per second"

    }

}
```


Go to the [API documentation](https://docs.apify.com/api/v2.md#rate-limiting) for details and to learn what to do if you exceed the rate limit.

## Share

You can grant [access rights](https://docs.apify.com/account/collaboration.md) to other Apify users to view or modify your storages. Check the [full list of permissions](https://docs.apify.com/account/collaboration/list-of-permissions.md).

You can also share storages by link using their ID or name, depending on your account or resource-level general access setting. Learn how link-based access works in [General resource access](https://docs.apify.com/account/collaboration/general-resource-access.md).

For one-off sharing when access is restricted, generate time-limited pre-signed URLs. See [Sharing restricted resources with pre-signed URLs](https://docs.apify.com/account/collaboration/general-resource-access.md#pre-signed-urls).

Accessing restricted storage resources via API

If your storage resource is set to *restricted*, all API calls must include a valid authentication token in the `Authorization` header. If you're using **apify-client** the header is passed in automatically.

## Concurrent access

Storage can be accessed from any [Actor](https://docs.apify.com/actors.md) or [task](https://docs.apify.com/actors/running/tasks.md) run, provided you have its *name* or *ID*. Use the same methods and endpoints you'd use for the current run's storages.

[Datasets](https://docs.apify.com/storage/dataset.md) and [key-value stores](https://docs.apify.com/storage/key-value-store.md) support concurrent use. Multiple Actors or tasks can write to the same dataset or key-value store, and multiple runs can read from them at the same time.

[Request queues](https://docs.apify.com/storage/request-queue.md), on the other hand, only allow multiple runs to add new data. A request queue can only be processed by one Actor or task run at any one time.

Concurrent write order

When multiple runs write to a storage simultaneously, the order of writes is not guaranteed. Data is written as each request is processed. The same applies in key-value stores and request queues: if a delete request precedes a read request for the same record, the read request fails.

Accessing restricted storage resources between runs

If a storage resource access is set to **Restricted**, the run from which it's accessed must have explicit access to it. Learn how restricted access works in [General resource access](https://docs.apify.com/account/collaboration/general-resource-access.md).

## Delete storages

Named storages are only removed upon your request. You can delete storages in the following ways:

* [Apify Console](https://console.apify.com/storage) - using the **Actions** button in the store's detail page.
* [JavaScript SDK](https://docs.apify.com/sdk/js) - using the `.drop()` method of the [Dataset](https://docs.apify.com/sdk/js/api/apify/class/Dataset#drop), [Key-value store](https://docs.apify.com/sdk/js/api/apify/class/KeyValueStore#drop), or [Request queue](https://docs.apify.com/sdk/js/api/apify/class/RequestQueue#drop) class.
* [Python SDK](https://docs.apify.com/sdk/python) - using the `.drop()` method of the [Dataset](https://docs.apify.com/sdk/python/reference/class/Dataset#drop), [Key-value store](https://docs.apify.com/sdk/python/reference/class/KeyValueStore#drop), or [Request queue](https://docs.apify.com/sdk/python/reference/class/RequestQueue#drop) class.
* [JavaScript API client](https://docs.apify.com/api/client/js) - using the `.delete()` method in the [dataset](https://docs.apify.com/api/client/js/reference/class/DatasetClient), [key-value store](https://docs.apify.com/api/client/js/reference/class/KeyValueStoreClient), or [request queue](https://docs.apify.com/api/client/js/reference/class/RequestQueueClient) clients.
* [Python API client](https://docs.apify.com/api/client/python) - using the `.delete()` method in the [dataset](https://docs.apify.com/api/client/python#datasetclient), [key-value store](https://docs.apify.com/api/client/python/reference/class/KeyValueStoreClient), or [request queue](https://docs.apify.com/api/client/python/reference/class/RequestQueueClient) clients.
* [API](https://docs.apify.com/api/v2/key-value-store-delete.md) - using the `Delete [store]` endpoint, where `[store]` is the storage type you want to delete.
