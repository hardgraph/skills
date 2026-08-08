# Retrieve Actor data

Copy for LLM

Actor output data is stored in [datasets](https://docs.apify.com/platform/storage/dataset), which can be retrieved from individual Actor runs. Dataset items support pagination for efficient retrieval, and multiple datasets can be merged into a single dataset for further analysis. This merged dataset can then be exported into various formats such as CSV, JSON, XLSX, or XML. Additionally, [integrations](https://docs.apify.com/platform/integrations) provide powerful tools to automate data workflows.

The following example shows how to fetch datasets from an Actor's runs, paginate through their items, and merge them into a single dataset for unified analysis:

* Async client
* Sync client

```
import asyncio



from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    # Client initialization with the API token

    apify_client = ApifyClientAsync(token=TOKEN)

    actor_client = apify_client.actor('apify/instagram-hashtag-scraper')

    runs_client = actor_client.runs()



    # See pagination to understand how to get more datasets

    actor_datasets = await runs_client.list(limit=20)



    datasets_client = apify_client.datasets()

    merging_dataset = await datasets_client.get_or_create(name='merge-dataset')



    for dataset_item in actor_datasets.items:

        # Dataset items can be handled here. Dataset items can be paginated

        dataset_client = apify_client.dataset(dataset_item.id)

        dataset_items = await dataset_client.list_items(limit=1000)



        # Items can be pushed to single dataset

        merging_dataset_client = apify_client.dataset(merging_dataset.id)

        await merging_dataset_client.push_items(dataset_items.items)



        # ...





if __name__ == '__main__':

    asyncio.run(main())
```

```
from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    # Client initialization with the API token

    apify_client = ApifyClient(token=TOKEN)

    actor_client = apify_client.actor('apify/instagram-hashtag-scraper')

    runs_client = actor_client.runs()



    # See pagination to understand how to get more datasets

    actor_datasets = runs_client.list(limit=20)



    datasets_client = apify_client.datasets()

    merging_dataset = datasets_client.get_or_create(name='merge-dataset')



    for dataset_item in actor_datasets.items:

        # Dataset items can be handled here. Dataset items can be paginated

        dataset_client = apify_client.dataset(dataset_item.id)

        dataset_items = dataset_client.list_items(limit=1000)



        # Items can be pushed to single dataset

        merging_dataset_client = apify_client.dataset(merging_dataset.id)

        merging_dataset_client.push_items(dataset_items.items)



        # ...





if __name__ == '__main__':

    main()
```
