# Nested clients

Copy for LLM

Nested clients let you access related resources directly from a parent resource client, without manually constructing new client instances.

* Async client
* Sync client

```
from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(TOKEN)



    actor_client = apify_client.actor('username/actor-name')

    runs_client = actor_client.runs()



    # List the last 10 runs of the Actor.

    actor_runs = (await runs_client.list(limit=10, desc=True)).items



    # Select the last run of the Actor that finished with a SUCCEEDED status.

    last_succeeded_run_client = actor_client.last_run(status='SUCCEEDED')



    # Get dataset

    actor_run_dataset_client = last_succeeded_run_client.dataset()



    # Fetch items from the run's dataset

    dataset_items = (await actor_run_dataset_client.list_items()).items
```

```
from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    apify_client = ApifyClient(TOKEN)



    actor_client = apify_client.actor('username/actor-name')

    runs_client = actor_client.runs()



    # List the last 10 runs of the Actor.

    actor_runs = runs_client.list(limit=10, desc=True).items



    # Select the last run of the Actor that finished with a SUCCEEDED status.

    last_succeeded_run_client = actor_client.last_run(status='SUCCEEDED')



    # Get dataset

    actor_run_dataset_client = last_succeeded_run_client.dataset()



    # Fetch items from the run's dataset

    dataset_items = actor_run_dataset_client.list_items().items
```

This direct access to [Dataset](https://docs.apify.com/platform/storage/dataset) (and other storage resources) from the [`RunClient`](https://docs.apify.com/api/client/python/api/client/python/reference/class/RunClient.md) is especially convenient when used alongside the [`ActorClient.last_run`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ActorClient.md#last_run) method.
