# Error handling

Copy for LLM

When you use the Apify client, it automatically extracts all relevant data from the endpoint and returns it in the expected format. Date strings, for instance, are seamlessly converted to Python `datetime.datetime` objects. If an error occurs, the client raises an [`ApifyApiError`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ApifyApiError.md). This exception wraps the raw JSON errors returned by the API and provides additional context, making it easier to debug any issues that arise.

## Error subclasses[](#error-subclasses)

The Apify client provides dedicated error subclasses based on the HTTP status code of the failed response, so you can branch on error kind without inspecting `status_code` or `type`:

| Status | Subclass                                                                                                                   |
| ------ | -------------------------------------------------------------------------------------------------------------------------- |
| 400    | [`InvalidRequestError`](https://docs.apify.com/api/client/python/api/client/python/reference/class/InvalidRequestError.md) |
| 401    | [`UnauthorizedError`](https://docs.apify.com/api/client/python/api/client/python/reference/class/UnauthorizedError.md)     |
| 403    | [`ForbiddenError`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ForbiddenError.md)           |
| 404    | [`NotFoundError`](https://docs.apify.com/api/client/python/api/client/python/reference/class/NotFoundError.md)             |
| 409    | [`ConflictError`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ConflictError.md)             |
| 429    | [`RateLimitError`](https://docs.apify.com/api/client/python/api/client/python/reference/class/RateLimitError.md)           |
| 5xx    | [`ServerError`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ServerError.md)                 |

All subclasses inherit from [`ApifyApiError`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ApifyApiError.md), so an existing `except ApifyApiError` handler still catches every API error. Catch a specific subclass when you want to react differently to, for example, a missing resource or a rate-limit:

* Async client
* Sync client

```
import asyncio



from apify_client import ApifyClientAsync

from apify_client.errors import ApifyApiError, NotFoundError



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(TOKEN)



    try:

        # Try to list items from a non-existing dataset.

        dataset_client = apify_client.dataset('non-existing-dataset-id')

        dataset_items = (await dataset_client.list_items()).items

    except NotFoundError:

        # 404 — branch on a specific subclass when you want to react to it.

        dataset_items = []

    except ApifyApiError as err:

        # Catch-all for every other API error.

        print(f'API error: {err}')

        dataset_items = []



    print(f'Fetched {len(dataset_items)} items.')





if __name__ == '__main__':

    asyncio.run(main())
```

```
from apify_client import ApifyClient

from apify_client.errors import ApifyApiError, NotFoundError



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    apify_client = ApifyClient(TOKEN)



    try:

        # Try to list items from a non-existing dataset.

        dataset_client = apify_client.dataset('non-existing-dataset-id')

        dataset_items = dataset_client.list_items().items

    except NotFoundError:

        # 404 — branch on a specific subclass when you want to react to it.

        dataset_items = []

    except ApifyApiError as err:

        # Catch-all for every other API error.

        print(f'API error: {err}')

        dataset_items = []



    print(f'Fetched {len(dataset_items)} items.')





if __name__ == '__main__':

    main()
```

For a complete list of error classes, see the [`ApifyApiError`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ApifyApiError.md) reference.
