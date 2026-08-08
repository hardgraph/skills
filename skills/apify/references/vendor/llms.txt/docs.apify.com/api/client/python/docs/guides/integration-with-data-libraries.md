# Integrate with data libraries

Copy for LLM

The Apify client for Python seamlessly integrates with data analysis libraries like [Pandas](https://pandas.pydata.org/). This allows you to load dataset items directly into a Pandas [DataFrame](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html) for efficient manipulation and analysis. Pandas provides robust data structures and tools for handling large datasets, making it a powerful addition to your Apify workflows.

The following example shows how to retrieve items from the most recent dataset of an Actor run and load them into a Pandas DataFrame for further analysis:

* Async client
* Sync client

```
import asyncio



import pandas as pd



from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'





async def main() -> None:

    # Initialize the Apify client

    apify_client = ApifyClientAsync(token=TOKEN)

    actor_client = apify_client.actor('apify/web-scraper')

    run_client = actor_client.last_run()

    dataset_client = run_client.dataset()



    # Load items from last dataset run

    dataset_data = await dataset_client.list_items()



    # Pass dataset items to Pandas DataFrame

    data_frame = pd.DataFrame(dataset_data.items)



    print(data_frame.info)





if __name__ == '__main__':

    asyncio.run(main())
```

```
import pandas as pd



from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'





def main() -> None:

    # Initialize the Apify client

    apify_client = ApifyClient(token=TOKEN)

    actor_client = apify_client.actor('apify/web-scraper')

    run_client = actor_client.last_run()

    dataset_client = run_client.dataset()



    # Load items from last dataset run

    dataset_data = dataset_client.list_items()



    # Pass dataset items to Pandas DataFrame

    data_frame = pd.DataFrame(dataset_data.items)



    print(data_frame.info)





if __name__ == '__main__':

    main()
```
