# Pagination

Copy for LLM

Most methods named `list` or `list_something` in the Apify client return a page model — a [Pydantic](https://docs.pydantic.dev/latest/) model such as [`ListOfActors`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ListOfActors.md) or [`ListOfDatasets`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ListOfDatasets.md). Unstructured dataset items use the [`DatasetItemsPage`](https://docs.apify.com/api/client/python/api/client/python/reference/class/DatasetItemsPage.md) dataclass instead. All page models share a consistent interface for working with paginated data and expose the following fields:

* `items` - The main results you're looking for.
* `total` - The total number of items available.
* `offset` - The starting point of the current page.
* `count` - The number of items in the current page.
* `limit` - The maximum number of items per page.

Some methods paginate differently. For example, [`RequestQueueClient.list_requests`](https://docs.apify.com/api/client/python/api/client/python/reference/class/RequestQueueClient.md#list_requests) returns a cursor-based [`ListOfRequests`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ListOfRequests.md) without the `total`, `offset`, and `count` fields. To fetch the next page, pass its `next_cursor` value back as the `cursor` parameter. Other examples include `list_keys` and `list_head`. Regardless, the primary results are always stored under the `items` field, and the `limit` field can be used to control the number of results returned.

The following example shows how to fetch all items from a dataset using pagination:

* Async client
* Sync client

```
from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(TOKEN)



    # Initialize the dataset client

    dataset_client = apify_client.dataset('dataset-id')



    # Define the pagination parameters

    limit = 1000  # Number items to request from API

    offset = 0  # Starting offset



    # Send single API call to fetch paginated items.

    # (number of items per single call can be limited by API)

    paginated_items = await dataset_client.list_items(limit=limit, offset=offset)



    # Inspect pagination metadata returned by API

    print(paginated_items.total)



    for item in paginated_items.items:

        print(item)  # Process the item as needed
```

```
from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    apify_client = ApifyClient(TOKEN)



    # Initialize the dataset client

    dataset_client = apify_client.dataset('dataset-id')



    # Define the pagination parameters

    limit = 1000  # Number items to request from API

    offset = 0  # Starting offset



    # Send single API call to fetch paginated items.

    # (number of items per single call can be limited by API)

    paginated_items = dataset_client.list_items(limit=limit, offset=offset)



    # Inspect pagination metadata returned by API

    print(paginated_items.total)



    for item in paginated_items.items:

        print(item)  # Process the item as needed
```

The page model interface offers several key benefits. Its consistent structure ensures predictable results for most `list` methods, providing a uniform way to work with paginated data. It also offers flexibility, allowing you to customize the `limit` and `offset` parameters to control data fetching according to your needs. Additionally, it provides scalability, enabling you to efficiently handle large datasets through pagination. This approach ensures efficient data retrieval while keeping memory usage under control, making it ideal for managing and processing large collections.

## Generator-based iteration[](#generator-based-iteration)

For collection clients, the `iterate` method returns an iterator that lazily fetches as many pages as needed to retrieve every item matching the filters. For dataset, key-value store and request queue clients, the matching helpers are `iterate_items`, `iterate_keys` and `iterate_requests`. They handle pagination automatically, so you don't need to manage offsets, limits or cursors yourself.

The example below iterates over every Actor owned by the current user using a collection client's `iterate` method:

* Async client
* Sync client

```
from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(TOKEN)



    # Iterate over all Actors owned by the current user, lazily fetching

    # as many pages as needed under the hood.

    async for actor in apify_client.actors().iterate(my=True):

        print(actor.id)
```

```
from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    apify_client = ApifyClient(TOKEN)



    # Iterate over all Actors owned by the current user, lazily fetching

    # as many pages as needed under the hood.

    for actor in apify_client.actors().iterate(my=True):

        print(actor.id)





if __name__ == '__main__':

    main()
```

The next example uses `iterate_items` on a dataset client to stream items past a given offset:

* Async client
* Sync client

```
from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(TOKEN)

    dataset_client = apify_client.dataset('dataset-id')



    # Define the pagination parameters

    limit = 1500  # Number of items in total

    offset = 100  # Starting offset



    # Iterate through items automatically, lazily sending as many API calls

    # as needed and receiving items in chunks.

    async for item in dataset_client.iterate_items(limit=limit, offset=offset):

        print(item)  # Process the item as needed
```

```
from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    apify_client = ApifyClient(TOKEN)

    dataset_client = apify_client.dataset('dataset-id')



    # Define the pagination parameters

    limit = 1500  # Number of items in total

    offset = 100  # Starting offset



    # Iterate through items automatically, lazily sending as many API calls

    # as needed and receiving items in chunks.

    for item in dataset_client.iterate_items(limit=limit, offset=offset):

        print(item)  # Process the item as needed





if __name__ == '__main__':

    main()
```
