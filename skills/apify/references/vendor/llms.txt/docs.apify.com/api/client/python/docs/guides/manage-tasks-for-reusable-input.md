# Manage tasks for reusable input

Copy for LLM

When you need to run multiple inputs with the same Actor, the most convenient approach is to create multiple [tasks](https://docs.apify.com/platform/actors/running/tasks), each with different input configurations. Task inputs are stored on the Apify platform when the task is created, allowing you to reuse them easily.

The following example shows how to create tasks for the `apify/instagram-hashtag-scraper` Actor with different inputs, manage task clients, and execute them asynchronously:

* Async client
* Sync client

```
import asyncio



from apify_client import ApifyClientAsync



TOKEN = 'MY-APIFY-TOKEN'

HASHTAGS = ['zebra', 'lion', 'hippo']





async def main() -> None:

    apify_client = ApifyClientAsync(token=TOKEN)



    # Create Apify tasks

    apify_tasks = []

    apify_tasks_client = apify_client.tasks()



    for hashtag in HASHTAGS:

        apify_task = await apify_tasks_client.create(

            name=f'hashtags-{hashtag}',

            actor_id='apify/instagram-hashtag-scraper',

            task_input={'hashtags': [hashtag], 'resultsLimit': 20},

            memory_mbytes=1024,

        )

        apify_tasks.append(apify_task)



    print('Tasks created:', apify_tasks)



    # Create Apify task clients

    apify_task_clients = [apify_client.task(task.id) for task in apify_tasks]



    print('Task clients created:', apify_task_clients)



    # Execute Apify tasks

    async with asyncio.TaskGroup() as tg:

        tasks = [tg.create_task(client.call()) for client in apify_task_clients]



    task_run_results = [task.result() for task in tasks]



    # Filter out None results (tasks that failed to return a run)

    successful_runs = [run for run in task_run_results if run is not None]



    print('Task results:', successful_runs)





if __name__ == '__main__':

    asyncio.run(main())
```

```
from apify_client import ApifyClient



TOKEN = 'MY-APIFY-TOKEN'

HASHTAGS = ['zebra', 'lion', 'hippo']





def main() -> None:

    apify_client = ApifyClient(token=TOKEN)



    # Create Apify tasks

    apify_tasks = []

    apify_tasks_client = apify_client.tasks()



    for hashtag in HASHTAGS:

        apify_task = apify_tasks_client.create(

            name=f'hashtags-{hashtag}',

            actor_id='apify/instagram-hashtag-scraper',

            task_input={'hashtags': [hashtag], 'resultsLimit': 20},

            memory_mbytes=1024,

        )

        apify_tasks.append(apify_task)



    print('Tasks created:', apify_tasks)



    # Create Apify task clients

    apify_task_clients = [apify_client.task(task.id) for task in apify_tasks]



    print('Task clients created:', apify_task_clients)



    # Execute Apify tasks

    task_run_results = [client.call() for client in apify_task_clients]



    # Filter out None results (tasks that failed to return a run)

    successful_runs = [run for run in task_run_results if run is not None]



    print('Task results:', successful_runs)





if __name__ == '__main__':

    main()
```
