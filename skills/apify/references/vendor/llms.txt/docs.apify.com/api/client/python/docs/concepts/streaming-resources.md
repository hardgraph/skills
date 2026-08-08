# Streaming resources

Copy for LLM

Certain resources, such as dataset items, key-value store records, and logs, support streaming directly from the Apify API. This allows you to process large resources incrementally without downloading them entirely into memory, making it ideal for handling large or continuously updated data.

Supported streaming methods:

* [`DatasetClient.stream_items`](https://docs.apify.com/api/client/python/api/client/python/reference/class/DatasetClient.md#stream_items) - Stream dataset items incrementally. Yields a raw streaming [`HttpResponse`](https://docs.apify.com/api/client/python/api/client/python/reference/class/HttpResponse.md).
* [`KeyValueStoreClient.stream_record`](https://docs.apify.com/api/client/python/api/client/python/reference/class/KeyValueStoreClient.md#stream_record) - Stream a key-value store record as raw data. Yields a `dict` with the `key`, `value`, and `content_type` fields, where `value` holds the raw streaming response, or `None` when the record doesn't exist.
* [`LogClient.stream`](https://docs.apify.com/api/client/python/api/client/python/reference/class/LogClient.md#stream) - Stream logs in real time. Yields a raw streaming [`HttpResponse`](https://docs.apify.com/api/client/python/api/client/python/reference/class/HttpResponse.md), or `None` when the log doesn't exist.

All three methods are context managers. Consume the streamed data within a `with` block to ensure that the connection is closed automatically, preventing memory leaks or unclosed connections.

The following example shows how to stream the logs of an Actor run incrementally:

* Async client
* Sync client

```
from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(TOKEN)

    run_client = apify_client.run('MY-RUN-ID')

    log_client = run_client.log()



    async with log_client.stream() as log_stream:

        if log_stream:

            async for bytes_chunk in log_stream.aiter_bytes():

                print(bytes_chunk)
```

```
from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    apify_client = ApifyClient(TOKEN)

    run_client = apify_client.run('MY-RUN-ID')

    log_client = run_client.log()



    with log_client.stream() as log_stream:

        if log_stream:

            for bytes_chunk in log_stream.iter_bytes():

                print(bytes_chunk)
```

Streaming is ideal for processing large logs, datasets, or files incrementally without downloading them entirely into memory.
