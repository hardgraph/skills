# Single and collection clients

Copy for LLM

The Apify client provides two types of resource clients: single-resource clients for managing an individual resource, and collection clients for listing or creating resources.

* [`ActorClient`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ActorClient.md) - Manages a single resource.
* [`ActorCollectionClient`](https://docs.apify.com/api/client/python/api/client/python/reference/class/ActorCollectionClient.md) - Manages a collection of resources.

- Async client
- Sync client

```
from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(TOKEN)



    # Collection clients do not require a parameter

    actor_collection_client = apify_client.actors()



    # Create an Actor with the name: my-actor

    my_actor = await actor_collection_client.create(name='my-actor')



    # List all of your Actors

    actor_list = (await actor_collection_client.list()).items
```

```
from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    apify_client = ApifyClient(TOKEN)



    # Collection clients do not require a parameter

    actor_collection_client = apify_client.actors()



    # Create an Actor with the name: my-actor

    my_actor = actor_collection_client.create(name='my-actor')



    # List all of your Actors

    actor_list = actor_collection_client.list().items
```

The resource ID can be the resource's `id` or a combination of `username/resource-name`.

* Async client
* Sync client

```
from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    apify_client = ApifyClientAsync(TOKEN)



    # Resource clients accept an ID of the resource

    actor_client = apify_client.actor('username/actor-name')



    # Fetch the 'username/actor-name' object from the API

    my_actor = await actor_client.get()



    # Start the run of 'username/actor-name' and return the Run object

    my_actor_run = await actor_client.start()
```

```
from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    apify_client = ApifyClient(TOKEN)



    # Resource clients accept an ID of the resource

    actor_client = apify_client.actor('username/actor-name')



    # Fetch the 'username/actor-name' object from the API

    my_actor = actor_client.get()



    # Start the run of 'username/actor-name' and return the Run object

    my_actor_run = actor_client.start()
```

By utilizing the appropriate collection or resource client, you can simplify how you interact with the Apify API.
