# Retries

Copy for LLM

The Apify client automatically retries requests that fail due to:

* Network errors
* Internal errors in the Apify API (HTTP status codes 500 and above)
* Rate limit errors (HTTP status code 429)

By default, the client retries a failed request up to 4 times. The retry intervals use an exponential backoff strategy:

* The first retry occurs after approximately 500 milliseconds.
* The second retry occurs after approximately 1,000 milliseconds, and so on.

You can customize this behavior using the following options in the [`ApifyClient`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ApifyClient.md) constructor:

* `max_retries`: Defines the maximum number of retry attempts.
* `min_delay_between_retries`: Sets the minimum delay between retries as a `timedelta`.

Retries with exponential backoff help reduce the load on the server and increase the chances of a successful request.

* Async client
* Sync client

```
from datetime import timedelta



from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(

        token=TOKEN,

        max_retries=4,

        min_delay_between_retries=timedelta(milliseconds=500),

        timeout_short=timedelta(seconds=5),

        timeout_medium=timedelta(seconds=30),

        timeout_long=timedelta(seconds=360),

        timeout_max=timedelta(seconds=360),

    )
```

```
from datetime import timedelta



from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    apify_client = ApifyClient(

        token=TOKEN,

        max_retries=4,

        min_delay_between_retries=timedelta(milliseconds=500),

        timeout_short=timedelta(seconds=5),

        timeout_medium=timedelta(seconds=30),

        timeout_long=timedelta(seconds=360),

        timeout_max=timedelta(seconds=360),

    )
```
