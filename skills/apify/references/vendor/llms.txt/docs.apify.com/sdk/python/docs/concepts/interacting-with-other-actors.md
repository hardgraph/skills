# Interacting with other Actors

Copy for LLM

The Apify SDK lets you start, call, and transform (metamorph) other Actors directly from your Actor code. This is useful for composing complex workflows from smaller, reusable Actors.

For the full list of methods for interacting with other Actors, see the [`Actor`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md) class in the API reference. For details on running Actors and Actor tasks on the platform, see [Actors](https://docs.apify.com/platform/actors) and [Actor tasks](https://docs.apify.com/platform/actors/tasks).

## Actor start[](#actor-start)

The [`Actor.start`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#start) method starts another Actor on the Apify platform, and immediately returns the details of the started Actor run.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIFN0YXJ0IHlvdXIgb3duIEFjdG9yIG5hbWVkICdteS1mYW5jeS1hY3RvcicuXFxuICAgICAgICBhY3Rvcl9ydW4gPSBhd2FpdCBBY3Rvci5zdGFydChcXG4gICAgICAgICAgICBhY3Rvcl9pZD0nfm15LWZhbmN5LWFjdG9yJyxcXG4gICAgICAgICAgICBydW5faW5wdXQ9eydmb28nOiAnYmFyJ30sXFxuICAgICAgICApXFxuXFxuICAgICAgICAjIExvZyB0aGUgQWN0b3IgcnVuIElELlxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidBY3RvciBydW4gSUQ6IHthY3Rvcl9ydW4uaWR9JylcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.OZwyqz2jbWOpDGJBr-KQx6Jnp1-dBrbV7peOD9O_dlA\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Start your own Actor named 'my-fancy-actor'.

        actor_run = await Actor.start(

            actor_id='~my-fancy-actor',

            run_input={'foo': 'bar'},

        )



        # Log the Actor run ID.

        Actor.log.info(f'Actor run ID: {actor_run.id}')





if __name__ == '__main__':

    asyncio.run(main())
```

## Actor call[](#actor-call)

The [`Actor.call`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#call) method starts another Actor on the Apify platform, and waits for the started Actor run to finish.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIFN0YXJ0IHRoZSBhcGlmeS9zY3JlZW5zaG90LXVybCBBY3RvciBhbmQgd2FpdCBmb3IgaXQgdG8gZmluaXNoLlxcbiAgICAgICAgYWN0b3JfcnVuID0gYXdhaXQgQWN0b3IuY2FsbChcXG4gICAgICAgICAgICBhY3Rvcl9pZD0nYXBpZnkvc2NyZWVuc2hvdC11cmwnLFxcbiAgICAgICAgICAgIHJ1bl9pbnB1dD17XFxuICAgICAgICAgICAgICAgICd1cmxzJzogW3sndXJsJzogJ2h0dHBzOi8vd3d3LmFwaWZ5LmNvbS8nfV0sXFxuICAgICAgICAgICAgICAgICdkZWxheSc6IDEwMDAsXFxuICAgICAgICAgICAgICAgICd3YWl0VW50aWwnOiAnbG9hZCcsXFxuICAgICAgICAgICAgfSxcXG4gICAgICAgIClcXG5cXG4gICAgICAgICMgR2V0IHRoZSBBY3RvciBvdXRwdXQgZnJvbSB0aGUgZGF0YXNldC5cXG4gICAgICAgIHJ1bl9jbGllbnQgPSBBY3Rvci5hcGlmeV9jbGllbnQucnVuKGFjdG9yX3J1bi5pZClcXG4gICAgICAgIGRhdGFzZXRfY2xpZW50ID0gcnVuX2NsaWVudC5kYXRhc2V0KClcXG4gICAgICAgIGl0ZW1fbGlzdCA9IGF3YWl0IGRhdGFzZXRfY2xpZW50Lmxpc3RfaXRlbXMoKVxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidBY3RvciBvdXRwdXQ6IHtpdGVtX2xpc3QuaXRlbXN9JylcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.hsciKmplnMha-pFG4gi-eeJdA9qMkao0QnHjgA4KmDY\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Start the apify/screenshot-url Actor and wait for it to finish.

        actor_run = await Actor.call(

            actor_id='apify/screenshot-url',

            run_input={

                'urls': [{'url': 'https://www.apify.com/'}],

                'delay': 1000,

                'waitUntil': 'load',

            },

        )



        # Get the Actor output from the dataset.

        run_client = Actor.apify_client.run(actor_run.id)

        dataset_client = run_client.dataset()

        item_list = await dataset_client.list_items()

        Actor.log.info(f'Actor output: {item_list.items}')





if __name__ == '__main__':

    asyncio.run(main())
```

## Actor call task[](#actor-call-task)

The [`Actor.call_task`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#call_task) method starts an [Actor task](https://docs.apify.com/platform/actors/tasks) on the Apify platform, and waits for the started Actor run to finish.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIFN0YXJ0IHRoZSBBY3RvciB0YXNrIGJ5IGl0cyBJRCBhbmQgd2FpdCBmb3IgaXQgdG8gZmluaXNoLlxcbiAgICAgICAgYWN0b3JfcnVuID0gYXdhaXQgQWN0b3IuY2FsbF90YXNrKHRhc2tfaWQ9J1ozbTZGUFNqMEdZWjI1clFjJylcXG5cXG4gICAgICAgICMgR2V0IHRoZSB0YXNrIHJ1biBkYXRhc2V0IGl0ZW1zLlxcbiAgICAgICAgcnVuX2NsaWVudCA9IEFjdG9yLmFwaWZ5X2NsaWVudC5ydW4oYWN0b3JfcnVuLmlkKVxcbiAgICAgICAgZGF0YXNldF9jbGllbnQgPSBydW5fY2xpZW50LmRhdGFzZXQoKVxcbiAgICAgICAgaXRlbXMgPSBhd2FpdCBkYXRhc2V0X2NsaWVudC5saXN0X2l0ZW1zKClcXG4gICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnVGFzayBydW4gZGF0YXNldCBpdGVtczoge2l0ZW1zfScpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.WLfGer-Fm58cwB-SidaEhBiqNxVoRaMbEp5M3L6HRog\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Start the Actor task by its ID and wait for it to finish.

        actor_run = await Actor.call_task(task_id='Z3m6FPSj0GYZ25rQc')



        # Get the task run dataset items.

        run_client = Actor.apify_client.run(actor_run.id)

        dataset_client = run_client.dataset()

        items = await dataset_client.list_items()

        Actor.log.info(f'Task run dataset items: {items}')





if __name__ == '__main__':

    asyncio.run(main())
```

## Actor metamorph[](#actor-metamorph)

The [`Actor.metamorph`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#metamorph) operation transforms an Actor run into a run of another Actor with a new input. This feature is useful if you want to use another Actor to finish the work of your current Actor, instead of internally starting a new Actor run and waiting for its finish. With metamorph, you can easily create new Actors on top of existing ones, and give your users nicer input structure and user interface for the final Actor. For the users of your Actors, the metamorph operation is completely transparent; they will just see your Actor got the work done.

Internally, the system stops the container corresponding to the original Actor run and starts a new container using a different container image. All the default storages are preserved, and the new Actor input is stored under the `INPUT-METAMORPH-1` key in the same default key-value store.

To make your Actor compatible with the metamorph operation, use [`Actor.get_input`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#get_input) instead of [`Actor.get_value('INPUT')`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#get_value) to read your Actor input. This method will fetch the input using the right key in a case of metamorphed run.

For example, imagine you have an Actor that accepts a hotel URL on input, and then internally uses the [`apify/web-scraper`](https://apify.com/apify/web-scraper) public Actor to scrape all the hotel reviews. The metamorphing code would look as follows:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIEdldCB0aGUgb3JpZ2luYWwgQWN0b3IgaW5wdXQuXFxuICAgICAgICBhY3Rvcl9pbnB1dCA9IGF3YWl0IEFjdG9yLmdldF9pbnB1dCgpIG9yIHt9XFxuICAgICAgICBob3RlbF91cmwgPSBhY3Rvcl9pbnB1dC5nZXQoJ2hvdGVsX3VybCcpXFxuXFxuICAgICAgICAjIENyZWF0ZSBuZXcgaW5wdXQgZm9yIGFwaWZ5L3dlYi1zY3JhcGVyIEFjdG9yLlxcbiAgICAgICAgd2ViX3NjcmFwZXJfaW5wdXQgPSB7XFxuICAgICAgICAgICAgJ3N0YXJ0VXJscyc6IFt7J3VybCc6IGhvdGVsX3VybH1dLFxcbiAgICAgICAgICAgICdwYWdlRnVuY3Rpb24nOiBcXFwiXFxcIlxcXCJhc3luYyBmdW5jdGlvbiBwYWdlRnVuY3Rpb24oY29udGV4dCkge1xcbiAgICAgICAgICAgICAgICAvLyBIZXJlIHlvdSBwYXNzIHRoZSBKYXZhU2NyaXB0IHBhZ2UgZnVuY3Rpb25cXG4gICAgICAgICAgICAgICAgLy8gdGhhdCBzY3JhcGVzIGFsbCB0aGUgcmV2aWV3cyBmcm9tIHRoZSBob3RlbCdzIFVSTFxcbiAgICAgICAgICAgIH1cXFwiXFxcIlxcXCIsXFxuICAgICAgICB9XFxuXFxuICAgICAgICAjIE1ldGFtb3JwaCB0aGUgQWN0b3IgcnVuIHRvIGBhcGlmeS93ZWItc2NyYXBlcmAgd2l0aCB0aGUgbmV3IGlucHV0LlxcbiAgICAgICAgYXdhaXQgQWN0b3IubWV0YW1vcnBoKCdhcGlmeS93ZWItc2NyYXBlcicsIHdlYl9zY3JhcGVyX2lucHV0KVxcblxcbiAgICAgICAgIyBUaGlzIGNvZGUgd2lsbCBub3QgYmUgY2FsbGVkLCBzaW5jZSB0aGUgYG1ldGFtb3JwaGAgYWN0aW9uIHRlcm1pbmF0ZXNcXG4gICAgICAgICMgdGhlIGN1cnJlbnQgQWN0b3IgcnVuIGNvbnRhaW5lci5cXG4gICAgICAgIEFjdG9yLmxvZy5pbmZvKCdZb3Ugd2lsbCBub3Qgc2VlIHRoaXMhJylcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.7gaZdkSgrQM-KPC6t1yEiLF1pExuMvFwZo_uVxQo2Do\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Get the original Actor input.

        actor_input = await Actor.get_input() or {}

        hotel_url = actor_input.get('hotel_url')



        # Create new input for apify/web-scraper Actor.

        web_scraper_input = {

            'startUrls': [{'url': hotel_url}],

            'pageFunction': """async function pageFunction(context) {

                // Here you pass the JavaScript page function

                // that scrapes all the reviews from the hotel's URL

            }""",

        }



        # Metamorph the Actor run to `apify/web-scraper` with the new input.

        await Actor.metamorph('apify/web-scraper', web_scraper_input)



        # This code will not be called, since the `metamorph` action terminates

        # the current Actor run container.

        Actor.log.info('You will not see this!')





if __name__ == '__main__':

    asyncio.run(main())
```

## Aborting an Actor run[](#aborting-an-actor-run)

The [`Actor.abort`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#abort) method aborts a running Actor on the Apify platform. You can use it to cancel a long-running Actor that is no longer needed.

When you set `gracefully=True`, the platform sends `ABORTING` and `PERSIST_STATE` events to the target Actor, giving it time to save its state, and then force-stops it after 30 seconds. Without the `gracefully` flag, the Actor is stopped immediately.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIFN0YXJ0IGFub3RoZXIgQWN0b3JcXG4gICAgICAgIGFjdG9yX3J1biA9IGF3YWl0IEFjdG9yLnN0YXJ0KFxcbiAgICAgICAgICAgIGFjdG9yX2lkPSdhcGlmeS93ZWItc2NyYXBlcicsXFxuICAgICAgICAgICAgcnVuX2lucHV0PXsnc3RhcnRVcmxzJzogW3sndXJsJzogJ2h0dHBzOi8vZXhhbXBsZS5jb20nfV19LFxcbiAgICAgICAgKVxcblxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidTdGFydGVkIHJ1biB7YWN0b3JfcnVuLmlkfScpXFxuXFxuICAgICAgICAjIC4uLiBsYXRlciwgZGVjaWRlIHRoZSBydW4gaXMgbm8gbG9uZ2VyIG5lZWRlZCAuLi5cXG5cXG4gICAgICAgICMgR3JhY2VmdWwgYWJvcnQgc2VuZHMgQUJPUlRJTkcgYW5kIFBFUlNJU1RfU1RBVEUgZXZlbnRzIHRvIHRoZSB0YXJnZXQgQWN0b3IsXFxuICAgICAgICAjIHRoZW4gZm9yY2Utc3RvcHMgaXQgYWZ0ZXIgMzAgc2Vjb25kcy5cXG4gICAgICAgIGFib3J0ZWRfcnVuID0gYXdhaXQgQWN0b3IuYWJvcnQoXFxuICAgICAgICAgICAgcnVuX2lkPWFjdG9yX3J1bi5pZCxcXG4gICAgICAgICAgICBncmFjZWZ1bGx5PVRydWUsXFxuICAgICAgICAgICAgc3RhdHVzX21lc3NhZ2U9J05vIGxvbmdlciBuZWVkZWQnLFxcbiAgICAgICAgKVxcblxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidBYm9ydGVkIHJ1biBzdGF0dXM6IHthYm9ydGVkX3J1bi5zdGF0dXN9JylcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.fXYBvjcs_o1INW1mVQXFM2zLX2CB_pYLWSQH_MkNHF8\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Start another Actor

        actor_run = await Actor.start(

            actor_id='apify/web-scraper',

            run_input={'startUrls': [{'url': 'https://example.com'}]},

        )



        Actor.log.info(f'Started run {actor_run.id}')



        # ... later, decide the run is no longer needed ...



        # Graceful abort sends ABORTING and PERSIST_STATE events to the target Actor,

        # then force-stops it after 30 seconds.

        aborted_run = await Actor.abort(

            run_id=actor_run.id,

            gracefully=True,

            status_message='No longer needed',

        )



        Actor.log.info(f'Aborted run status: {aborted_run.status}')





if __name__ == '__main__':

    asyncio.run(main())
```
