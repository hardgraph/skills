# Asyncio support

Copy for LLM

The package provides an asynchronous version of the client, [`ApifyClientAsync`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ApifyClientAsync.md), which allows you to interact with the Apify API using Python's standard async/await syntax. This enables you to perform non-blocking operations, see the Python [asyncio documentation](https://docs.python.org/3/library/asyncio-task.html) for more information. This is useful for applications that need to perform multiple API operations concurrently or integrate with other async frameworks.

The following example shows how to run an Actor asynchronously and stream its logs while it is running:

```
import asyncio



from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(TOKEN)

    actor_client = apify_client.actor('my-actor-id')



    # Start the Actor and get the run ID

    run_result = await actor_client.start()

    run_client = apify_client.run(run_result.id)

    log_client = run_client.log()



    # Stream the logs

    async with log_client.stream() as async_log_stream:

        if async_log_stream:

            async for bytes_chunk in async_log_stream.aiter_bytes():

                print(bytes_chunk)





if __name__ == '__main__':

    asyncio.run(main())
```

For the full async client API, see the [`ApifyClientAsync`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ApifyClientAsync.md) reference.
