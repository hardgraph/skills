# Convenience methods

Copy for LLM

The Apify client provides several convenience methods to handle actions that the API alone cannot perform efficiently, such as waiting for an Actor run to finish without running into network timeouts. These methods simplify common tasks and enhance the usability of the client.

* [`ActorClient.call`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ActorClient.md#call) - Starts an Actor and waits for it to finish, handling network timeouts internally. Waits indefinitely by default, or up to the specified `wait_duration`.
* [`ActorClient.start`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ActorClient.md#start) - Starts an Actor and immediately returns the Run object without waiting for it to finish.
* [`RunClient.wait_for_finish`](https://docs.apify.com/api/client/python/api/client/python/reference/class/RunClient.md#wait_for_finish) - Waits for an already-started run to reach a terminal status.

Additionally, storage-related resources offer flexible options for data retrieval:

* [Key-value store](https://docs.apify.com/platform/storage/key-value-store) records can be retrieved as objects, buffers, or streams.
* [Dataset](https://docs.apify.com/platform/storage/dataset) items can be fetched as individual objects, serialized data, or iterated asynchronously.

- Async client
- Sync client

```
from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(TOKEN)

    actor_client = apify_client.actor('username/actor-name')



    # Start an Actor and wait for it to finish.

    finished_actor_run = await actor_client.call()



    # Start an Actor and wait up to 60 seconds for it to finish.

    actor_run = await actor_client.start(wait_for_finish=60)
```

```
from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    apify_client = ApifyClient(TOKEN)

    actor_client = apify_client.actor('username/actor-name')



    # Start an Actor and wait for it to finish.

    finished_actor_run = actor_client.call()



    # Start an Actor and wait up to 60 seconds for it to finish.

    actor_run = actor_client.start(wait_for_finish=60)
```

tip

The `call()` method polls internally and may take a long time for long-running Actors. Use `start()` and `wait_for_finish()` separately if you need more control over the waiting behavior.
