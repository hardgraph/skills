# Storages

Copy for LLM

The [`Actor`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md) class provides methods to work either with the default storages of the Actor, or with any other storage, named or unnamed.

For details on storage types, see [Storage](https://docs.apify.com/platform/storage).

## Types of storages[](#types-of-storages)

There are three types of storages available to Actors.

First are [datasets](https://docs.apify.com/platform/storage/dataset), which are append-only tables for storing the results of your Actors. You can open a dataset through the [`Actor.open_dataset`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#open_dataset) method, and work with it through the resulting [`Dataset`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Dataset.md) class instance.

Next there are [key-value stores](https://docs.apify.com/platform/storage/key-value-store), which function as a read/write storage for storing file-like objects, typically the Actor state or binary results. You can open a key-value store through the [`Actor.open_key_value_store`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#open_key_value_store) method, and work with it through the resulting [`KeyValueStore`](https://docs.apify.com/sdk/python/sdk/python/reference/class/KeyValueStore.md) class instance.

Finally, there are [request queues](https://docs.apify.com/platform/storage/request-queue). These are queues into which you can put the URLs you want to scrape, and from which the Actor can dequeue them and process them. You can open a request queue through the [`Actor.open_request_queue`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#open_request_queue) method, and work with it through the resulting [`RequestQueue`](https://docs.apify.com/sdk/python/sdk/python/reference/class/RequestQueue.md) class instance.

Each Actor run has its default dataset, default key-value store and default request queue.

## Local storage emulation[](#local-storage-emulation)

To be able to develop Actors locally, the storages that the Apify platform provides are emulated on the local filesystem.

The storage contents are loaded from and saved to the `storage` folder in the Actor's main folder. Each storage type is stored in its own subfolder, so for example datasets are stored in the `storage/datasets` folder.

Each storage is then stored in its own folder, named after the storage, or called `default` if it's the default storage. For example, a request queue with the name `my-queue` would be stored in `storage/request_queues/my-queue`.

Each dataset item, key-value store record, or request in a request queue is then stored in its own file in the storage folder. Dataset items and request queue requests are always JSON files, and key-value store records can be any file type, based on its content type. For example, the Actor input is typically stored in `storage/key_value_stores/default/INPUT.json`.

## Local Actor run with remote storage[](#local-actor-run-with-remote-storage)

When developing locally, opening any storage will by default use local storage. To change this behavior and to use remote storage you have to use `force_cloud=True` argument in [`Actor.open_dataset`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#open_dataset), [`Actor.open_request_queue`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#open_request_queue) or [`Actor.open_key_value_store`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#open_key_value_store). Proper use of this argument allows you to work with both local and remote storages.

Calling another remote Actor and accessing its default storage is a typical use-case for using `force_cloud=True` argument to open remote Actor's storages.

### Local storage persistence[](#local-storage-persistence)

By default, the storage contents are persisted across multiple Actor runs. To clean up the Actor storages before running the Actor, use the `--purge` flag of the [`apify run`](https://docs.apify.com/cli/docs/reference#apify-run) command of the Apify CLI.

```
apify run --purge
```

## Convenience methods for working with default storages[](#convenience-methods-for-working-with-default-storages)

There are several methods for directly working with the default key-value store or default dataset of the Actor.

* [`Actor.get_value('my-record')`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#get_value) reads a record from the default key-value store of the Actor.
* [`Actor.set_value('my-record', 'my-value')`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#set_value) saves a new value to the record in the default key-value store.
* [`Actor.get_input`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#get_input) reads the Actor input from the default key-value store of the Actor.
* [`Actor.push_data([{'result': 'Hello, world!'}, ...])`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#push_data) saves results to the default dataset of the Actor. When using the [pay-per-event pricing model](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/pay-per-event.md), `push_data` returns a `ChargeResult` object that indicates whether the charge limit has been reached. You can also pass a `charged_event_name` parameter to charge for a custom event for each pushed item.

## Opening named and unnamed storages[](#opening-named-and-unnamed-storages)

The [`Actor.open_dataset`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#open_dataset), [`Actor.open_key_value_store`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#open_key_value_store) and [`Actor.open_request_queue`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#open_request_queue) methods can be used to open any storage for reading and writing. You can either use them without arguments to open the default storages, or you can pass a storage ID or name to open another storage.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3IsIFJlcXVlc3RcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIFdvcmsgd2l0aCB0aGUgZGVmYXVsdCBkYXRhc2V0IG9mIHRoZSBBY3RvclxcbiAgICAgICAgZGF0YXNldCA9IGF3YWl0IEFjdG9yLm9wZW5fZGF0YXNldCgpXFxuICAgICAgICBhd2FpdCBkYXRhc2V0LnB1c2hfZGF0YSh7J3Jlc3VsdCc6ICdIZWxsbywgd29ybGQhJ30pXFxuXFxuICAgICAgICAjIFdvcmsgd2l0aCB0aGUga2V5LXZhbHVlIHN0b3JlIHdpdGggSUQgJ21JSlZac1JRckRRZjRyVUFmJ1xcbiAgICAgICAga2V5X3ZhbHVlX3N0b3JlID0gYXdhaXQgQWN0b3Iub3Blbl9rZXlfdmFsdWVfc3RvcmUoaWQ9J21JSlZac1JRckRRZjRyVUFmJylcXG4gICAgICAgIGF3YWl0IGtleV92YWx1ZV9zdG9yZS5zZXRfdmFsdWUoJ3JlY29yZCcsICdIZWxsbywgd29ybGQhJylcXG5cXG4gICAgICAgICMgV29yayB3aXRoIHRoZSByZXF1ZXN0IHF1ZXVlIHdpdGggdGhlIG5hbWUgJ215LXF1ZXVlJ1xcbiAgICAgICAgcmVxdWVzdF9xdWV1ZSA9IGF3YWl0IEFjdG9yLm9wZW5fcmVxdWVzdF9xdWV1ZShuYW1lPSdteS1xdWV1ZScpXFxuICAgICAgICBhd2FpdCByZXF1ZXN0X3F1ZXVlLmFkZF9yZXF1ZXN0KFJlcXVlc3QuZnJvbV91cmwoJ2h0dHBzOi8vYXBpZnkuY29tJykpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.Kyo_kGWHonIZHpoiBgmpYFFDn6Wma3zXFeYsbmEXySw\&asrc=run_on_apify)

```
import asyncio



from apify import Actor, Request





async def main() -> None:

    async with Actor:

        # Work with the default dataset of the Actor

        dataset = await Actor.open_dataset()

        await dataset.push_data({'result': 'Hello, world!'})



        # Work with the key-value store with ID 'mIJVZsRQrDQf4rUAf'

        key_value_store = await Actor.open_key_value_store(id='mIJVZsRQrDQf4rUAf')

        await key_value_store.set_value('record', 'Hello, world!')



        # Work with the request queue with the name 'my-queue'

        request_queue = await Actor.open_request_queue(name='my-queue')

        await request_queue.add_request(Request.from_url('https://apify.com'))





if __name__ == '__main__':

    asyncio.run(main())
```

Besides `id` and `name`, the `open_*` methods also accept an `alias` parameter. An alias creates an unnamed storage scoped to the current Actor run — it does not persist across runs, but lets you reference the same storage within a single run using a human-readable label. The `alias` parameter is mutually exclusive with `id` and `name`.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIE9wZW4gYSBkYXRhc2V0IHdpdGggYW4gYWxpYXMg4oCUIHRoaXMgY3JlYXRlcyBhbiB1bm5hbWVkIGRhdGFzZXRcXG4gICAgICAgICMgdGhhdCBjYW4gYmUgcmVmZXJlbmNlZCBieSB0aGlzIGFsaWFzIHdpdGhpbiB0aGUgY3VycmVudCBydW5cXG4gICAgICAgIGRhdGFzZXQgPSBhd2FpdCBBY3Rvci5vcGVuX2RhdGFzZXQoYWxpYXM9J2ludGVybWVkaWF0ZS1yZXN1bHRzJylcXG4gICAgICAgIGF3YWl0IGRhdGFzZXQucHVzaF9kYXRhKHsnc3RlcCc6IDEsICdyZXN1bHQnOiAncGFydGlhbCBkYXRhJ30pXFxuXFxuICAgICAgICAjIExhdGVyLCBvcGVuIHRoZSBzYW1lIGRhdGFzZXQgdXNpbmcgdGhlIHNhbWUgYWxpYXNcXG4gICAgICAgIHNhbWVfZGF0YXNldCA9IGF3YWl0IEFjdG9yLm9wZW5fZGF0YXNldChhbGlhcz0naW50ZXJtZWRpYXRlLXJlc3VsdHMnKVxcbiAgICAgICAgZGF0YSA9IGF3YWl0IHNhbWVfZGF0YXNldC5nZXRfZGF0YSgpXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ0l0ZW1zIGluIGRhdGFzZXQ6IHtkYXRhLmNvdW50fScpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.KWx_vuzWAHOsb9JliOUQ9zxVwDUjMmICKdmDBxxY6c4\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Open a dataset with an alias — this creates an unnamed dataset

        # that can be referenced by this alias within the current run

        dataset = await Actor.open_dataset(alias='intermediate-results')

        await dataset.push_data({'step': 1, 'result': 'partial data'})



        # Later, open the same dataset using the same alias

        same_dataset = await Actor.open_dataset(alias='intermediate-results')

        data = await same_dataset.get_data()

        Actor.log.info(f'Items in dataset: {data.count}')





if __name__ == '__main__':

    asyncio.run(main())
```

## Deleting storages[](#deleting-storages)

To delete a storage, you can use the [`Dataset.drop`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Dataset.md#drop), [`KeyValueStore.drop`](https://docs.apify.com/sdk/python/sdk/python/reference/class/KeyValueStore.md#drop) or [`RequestQueue.drop`](https://docs.apify.com/sdk/python/sdk/python/reference/class/RequestQueue.md#drop) methods.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIE9wZW4gYSBrZXktdmFsdWUgc3RvcmUgd2l0aCB0aGUgbmFtZSAnbXktY29vbC1zdG9yZSdcXG4gICAgICAgIGtleV92YWx1ZV9zdG9yZSA9IGF3YWl0IEFjdG9yLm9wZW5fa2V5X3ZhbHVlX3N0b3JlKG5hbWU9J215LWNvb2wtc3RvcmUnKVxcbiAgICAgICAgYXdhaXQga2V5X3ZhbHVlX3N0b3JlLnNldF92YWx1ZSgncmVjb3JkJywgJ0hlbGxvLCB3b3JsZCEnKVxcblxcbiAgICAgICAgIyBEbyBzb21ldGhpbmcgLi4uXFxuXFxuICAgICAgICAjIE5vdyB3ZSBkb24ndCB3YW50IGl0IGFueW1vcmVcXG4gICAgICAgIGF3YWl0IGtleV92YWx1ZV9zdG9yZS5kcm9wKClcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.W2wBx6Lar5dng_IDIi9LVdgd30ehIq9QsIL2dcNKUVA\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Open a key-value store with the name 'my-cool-store'

        key_value_store = await Actor.open_key_value_store(name='my-cool-store')

        await key_value_store.set_value('record', 'Hello, world!')



        # Do something ...



        # Now we don't want it anymore

        await key_value_store.drop()





if __name__ == '__main__':

    asyncio.run(main())
```

## Working with datasets[](#working-with-datasets)

In this section we will show you how to work with [datasets](https://docs.apify.com/platform/storage/dataset).

### Reading & writing items[](#reading--writing-items)

To write data into a dataset, you can use the [`Dataset.push_data`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Dataset.md#push_data) method.

To read data from a dataset, you can use the [`Dataset.get_data`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Dataset.md#get_data) method.

To get an iterator of the data, you can use the [`Dataset.iterate_items`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Dataset.md#iterate_items) method.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIE9wZW4gYSBkYXRhc2V0IGFuZCB3cml0ZSBzb21lIGRhdGEgaW4gaXRcXG4gICAgICAgIGRhdGFzZXQgPSBhd2FpdCBBY3Rvci5vcGVuX2RhdGFzZXQobmFtZT0nbXktY29vbC1kYXRhc2V0JylcXG4gICAgICAgIGF3YWl0IGRhdGFzZXQucHVzaF9kYXRhKFt7J2l0ZW1Obyc6IGl9IGZvciBpIGluIHJhbmdlKDEwMDApXSlcXG5cXG4gICAgICAgICMgUmVhZCBiYWNrIHRoZSBmaXJzdCBoYWxmIG9mIHRoZSBkYXRhXFxuICAgICAgICBmaXJzdF9oYWxmID0gYXdhaXQgZGF0YXNldC5nZXRfZGF0YShsaW1pdD01MDApXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ1RoZSBmaXJzdCBoYWxmIG9mIGl0ZW1zID0ge2ZpcnN0X2hhbGYuaXRlbXN9JylcXG5cXG4gICAgICAgICMgSXRlcmF0ZSBvdmVyIHRoZSBzZWNvbmQgaGFsZlxcbiAgICAgICAgc2Vjb25kX2hhbGYgPSBbaXRlbSBhc3luYyBmb3IgaXRlbSBpbiBkYXRhc2V0Lml0ZXJhdGVfaXRlbXMob2Zmc2V0PTUwMCldXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ1RoZSBzZWNvbmQgaGFsZiBvZiBpdGVtcyA9IHtzZWNvbmRfaGFsZn0nKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.vvpj4JD5IeEE_JVcMxMeCY9TUFiPyc8AVjGWaLc63LE\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Open a dataset and write some data in it

        dataset = await Actor.open_dataset(name='my-cool-dataset')

        await dataset.push_data([{'itemNo': i} for i in range(1000)])



        # Read back the first half of the data

        first_half = await dataset.get_data(limit=500)

        Actor.log.info(f'The first half of items = {first_half.items}')



        # Iterate over the second half

        second_half = [item async for item in dataset.iterate_items(offset=500)]

        Actor.log.info(f'The second half of items = {second_half}')





if __name__ == '__main__':

    asyncio.run(main())
```

### Exporting items[](#exporting-items)

You can also export the dataset items into a key-value store, as either a CSV or a JSON record, using the [`Dataset.export_to`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Dataset.md#export_to) method with the `content_type` argument set to `'csv'` or `'json'`.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIE9wZW4gYSBkYXRhc2V0IGFuZCB3cml0ZSBzb21lIGRhdGEgaW4gaXRcXG4gICAgICAgIGRhdGFzZXQgPSBhd2FpdCBBY3Rvci5vcGVuX2RhdGFzZXQobmFtZT0nbXktY29vbC1kYXRhc2V0JylcXG4gICAgICAgIGF3YWl0IGRhdGFzZXQucHVzaF9kYXRhKFt7J2l0ZW1Obyc6IGl9IGZvciBpIGluIHJhbmdlKDEwMDApXSlcXG5cXG4gICAgICAgICMgRXhwb3J0IHRoZSBkYXRhIGFzIENTVlxcbiAgICAgICAgYXdhaXQgZGF0YXNldC5leHBvcnRfdG8oXFxuICAgICAgICAgICAgY29udGVudF90eXBlPSdjc3YnLFxcbiAgICAgICAgICAgIGtleT0nZGF0YS5jc3YnLFxcbiAgICAgICAgICAgIHRvX2t2c19uYW1lPSdteS1jb29sLWtleS12YWx1ZS1zdG9yZScsXFxuICAgICAgICApXFxuXFxuICAgICAgICAjIEV4cG9ydCB0aGUgZGF0YSBhcyBKU09OXFxuICAgICAgICBhd2FpdCBkYXRhc2V0LmV4cG9ydF90byhcXG4gICAgICAgICAgICBjb250ZW50X3R5cGU9J2pzb24nLFxcbiAgICAgICAgICAgIGtleT0nZGF0YS5qc29uJyxcXG4gICAgICAgICAgICB0b19rdnNfbmFtZT0nbXktY29vbC1rZXktdmFsdWUtc3RvcmUnLFxcbiAgICAgICAgKVxcblxcbiAgICAgICAgIyBQcmludCB0aGUgZXhwb3J0ZWQgcmVjb3Jkc1xcbiAgICAgICAgc3RvcmUgPSBhd2FpdCBBY3Rvci5vcGVuX2tleV92YWx1ZV9zdG9yZShuYW1lPSdteS1jb29sLWtleS12YWx1ZS1zdG9yZScpXFxuXFxuICAgICAgICBjc3ZfZGF0YSA9IGF3YWl0IHN0b3JlLmdldF92YWx1ZSgnZGF0YS5jc3YnKVxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidDU1YgZGF0YToge2Nzdl9kYXRhfScpXFxuXFxuICAgICAgICBqc29uX2RhdGEgPSBhd2FpdCBzdG9yZS5nZXRfdmFsdWUoJ2RhdGEuanNvbicpXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ0pTT04gZGF0YToge2pzb25fZGF0YX0nKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.xF4h0wYEHBtvFza6xWawolv5xRnxPYIqlTsusB9x7OQ\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Open a dataset and write some data in it

        dataset = await Actor.open_dataset(name='my-cool-dataset')

        await dataset.push_data([{'itemNo': i} for i in range(1000)])



        # Export the data as CSV

        await dataset.export_to(

            content_type='csv',

            key='data.csv',

            to_kvs_name='my-cool-key-value-store',

        )



        # Export the data as JSON

        await dataset.export_to(

            content_type='json',

            key='data.json',

            to_kvs_name='my-cool-key-value-store',

        )



        # Print the exported records

        store = await Actor.open_key_value_store(name='my-cool-key-value-store')



        csv_data = await store.get_value('data.csv')

        Actor.log.info(f'CSV data: {csv_data}')



        json_data = await store.get_value('data.json')

        Actor.log.info(f'JSON data: {json_data}')





if __name__ == '__main__':

    asyncio.run(main())
```

## Working with key-value stores[](#working-with-key-value-stores)

In this section we will show you how to work with [key-value stores](https://docs.apify.com/platform/storage/key-value-store).

### Reading and writing records[](#reading-and-writing-records)

To read records from a key-value store, you can use the [`KeyValueStore.get_value`](https://docs.apify.com/sdk/python/sdk/python/reference/class/KeyValueStore.md#get_value) method.

To write records into a key-value store, you can use the [`KeyValueStore.set_value`](https://docs.apify.com/sdk/python/sdk/python/reference/class/KeyValueStore.md#set_value) method. You can set the content type of a record with the `content_type` argument. To delete a record, set its value to `None`.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIE9wZW4gYSBuYW1lZCBrZXktdmFsdWUgc3RvcmVcXG4gICAgICAgIGt2cyA9IGF3YWl0IEFjdG9yLm9wZW5fa2V5X3ZhbHVlX3N0b3JlKG5hbWU9J215LWNvb2wta2V5LXZhbHVlLXN0b3JlJylcXG5cXG4gICAgICAgICMgV3JpdGUgc29tZSBkYXRhIHRvIGl0XFxuICAgICAgICBhd2FpdCBrdnMuc2V0X3ZhbHVlKCdhdXRvbWF0aWNfdGV4dCcsICdhYmNkJylcXG4gICAgICAgIGF3YWl0IGt2cy5zZXRfdmFsdWUoJ2F1dG9tYXRpY19qc29uJywgeydhYic6ICdjZCd9KVxcbiAgICAgICAgYXdhaXQga3ZzLnNldF92YWx1ZSgnZXhwbGljaXRfY3N2JywgJ2EsYlxcXFxuYyxkJywgY29udGVudF90eXBlPSd0ZXh0L2NzdicpXFxuXFxuICAgICAgICAjIEdldCB0aGUgdmFsdWVzIGFuZCBsb2cgdGhlbVxcbiAgICAgICAgYXV0b21hdGljX3RleHQgPSBhd2FpdCBrdnMuZ2V0X3ZhbHVlKCdhdXRvbWF0aWNfdGV4dCcpXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ0F1dG9tYXRpYyB0ZXh0OiB7YXV0b21hdGljX3RleHR9JylcXG5cXG4gICAgICAgIGF1dG9tYXRpY19qc29uID0gYXdhaXQga3ZzLmdldF92YWx1ZSgnYXV0b21hdGljX2pzb24nKVxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidBdXRvbWF0aWMgSlNPTjoge2F1dG9tYXRpY19qc29ufScpXFxuXFxuICAgICAgICBleHBsaWNpdF9jc3YgPSBhd2FpdCBrdnMuZ2V0X3ZhbHVlKCdleHBsaWNpdF9jc3YnKVxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidFeHBsaWNpdCBDU1Y6IHtleHBsaWNpdF9jc3Z9JylcXG5cXG4gICAgICAgICMgRGVsZXRlIHRoZSBgYXV0b21hdGljX3RleHRgIHZhbHVlXFxuICAgICAgICBhd2FpdCBrdnMuc2V0X3ZhbHVlKCdhdXRvbWF0aWNfdGV4dCcsIE5vbmUpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.u3JN0vfRXA7eQEQbPiFKvNcnkSYdmbMGA9HQ4dkH9TY\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Open a named key-value store

        kvs = await Actor.open_key_value_store(name='my-cool-key-value-store')



        # Write some data to it

        await kvs.set_value('automatic_text', 'abcd')

        await kvs.set_value('automatic_json', {'ab': 'cd'})

        await kvs.set_value('explicit_csv', 'a,b\nc,d', content_type='text/csv')



        # Get the values and log them

        automatic_text = await kvs.get_value('automatic_text')

        Actor.log.info(f'Automatic text: {automatic_text}')



        automatic_json = await kvs.get_value('automatic_json')

        Actor.log.info(f'Automatic JSON: {automatic_json}')



        explicit_csv = await kvs.get_value('explicit_csv')

        Actor.log.info(f'Explicit CSV: {explicit_csv}')



        # Delete the `automatic_text` value

        await kvs.set_value('automatic_text', None)





if __name__ == '__main__':

    asyncio.run(main())
```

### Iterating keys[](#iterating-keys)

To get an iterator of the key-value store record keys, you can use the [`KeyValueStore.iterate_keys`](https://docs.apify.com/sdk/python/sdk/python/reference/class/KeyValueStore.md#iterate_keys) method.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIE9wZW4gYSBuYW1lZCBrZXktdmFsdWUgc3RvcmVcXG4gICAgICAgIGt2cyA9IGF3YWl0IEFjdG9yLm9wZW5fa2V5X3ZhbHVlX3N0b3JlKG5hbWU9J215LWNvb2wta2V5LXZhbHVlLXN0b3JlJylcXG5cXG4gICAgICAgICMgV3JpdGUgc29tZSBkYXRhIHRvIGl0XFxuICAgICAgICBhd2FpdCBrdnMuc2V0X3ZhbHVlKCdhdXRvbWF0aWNfdGV4dCcsICdhYmNkJylcXG4gICAgICAgIGF3YWl0IGt2cy5zZXRfdmFsdWUoJ2F1dG9tYXRpY19qc29uJywgeydhYic6ICdjZCd9KVxcbiAgICAgICAgYXdhaXQga3ZzLnNldF92YWx1ZSgnZXhwbGljaXRfY3N2JywgJ2EsYlxcXFxuYyxkJywgY29udGVudF90eXBlPSd0ZXh0L2NzdicpXFxuXFxuICAgICAgICAjIFByaW50IHRoZSBpbmZvIGZvciBlYWNoIHJlY29yZFxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oJ1JlY29yZHMgaW4gc3RvcmU6JylcXG5cXG4gICAgICAgIGFzeW5jIGZvciBpdGVtIGluIGt2cy5pdGVyYXRlX2tleXMoKTpcXG4gICAgICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ2tleT17aXRlbS5rZXl9LCBzaXplPXtpdGVtLnNpemV9JylcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.iTYfs1diyO5R0BipFu3qs1rsiqwHm8kOFCWvA7PHnD8\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Open a named key-value store

        kvs = await Actor.open_key_value_store(name='my-cool-key-value-store')



        # Write some data to it

        await kvs.set_value('automatic_text', 'abcd')

        await kvs.set_value('automatic_json', {'ab': 'cd'})

        await kvs.set_value('explicit_csv', 'a,b\nc,d', content_type='text/csv')



        # Print the info for each record

        Actor.log.info('Records in store:')



        async for item in kvs.iterate_keys():

            Actor.log.info(f'key={item.key}, size={item.size}')





if __name__ == '__main__':

    asyncio.run(main())
```

### Public URLs of records[](#public-urls-of-records)

To get a publicly accessible URL of a key-value store record, you can use the [`KeyValueStore.get_public_url`](https://docs.apify.com/sdk/python/sdk/python/reference/class/KeyValueStore.md#get_public_url) method.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIE9wZW4gYSBuYW1lZCBrZXktdmFsdWUgc3RvcmVcXG4gICAgICAgIHN0b3JlID0gYXdhaXQgQWN0b3Iub3Blbl9rZXlfdmFsdWVfc3RvcmUobmFtZT0nbXktY29vbC1rZXktdmFsdWUtc3RvcmUnKVxcblxcbiAgICAgICAgIyBHZXQgdGhlIHB1YmxpYyBVUkwgb2YgYSByZWNvcmRcXG4gICAgICAgIG15X3JlY29yZF91cmwgPSBhd2FpdCBzdG9yZS5nZXRfcHVibGljX3VybCgnbXlfcmVjb3JkJylcXG4gICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnVVJMIG9mIFxcXCJteV9yZWNvcmRcXFwiOiB7bXlfcmVjb3JkX3VybH0nKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.In3DecM8oE05rLcU6qSS1v7IqS40WHBN73rdcf66BQ8\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Open a named key-value store

        store = await Actor.open_key_value_store(name='my-cool-key-value-store')



        # Get the public URL of a record

        my_record_url = await store.get_public_url('my_record')

        Actor.log.info(f'URL of "my_record": {my_record_url}')





if __name__ == '__main__':

    asyncio.run(main())
```

## Working with request queues[](#working-with-request-queues)

In this section we will show you how to work with [request queues](https://docs.apify.com/platform/storage/request-queue).

By default, a request queue is optimized for a single consumer. To let multiple Actor runs process the same queue at once, see [single vs. shared request queue](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/storage-clients.md#single-vs-shared-request-queue).

### Adding requests to a queue[](#adding-requests-to-a-queue)

To add a request into the queue, you can use the [`RequestQueue.add_request`](https://docs.apify.com/sdk/python/sdk/python/reference/class/RequestQueue.md#add_request) method.

You can use the `forefront` boolean argument to specify whether the request should go to the beginning of the queue, or to the end.

You can use the `unique_key` of the request to uniquely identify a request. If you try to add more requests with the same unique key, only the first one will be added.

Check out the [`Request`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Request.md) for more information on how to create requests and what properties they have.

### Reading requests[](#reading-requests)

To fetch the next request from the queue for processing, you can use the [`RequestQueue.fetch_next_request`](https://docs.apify.com/sdk/python/sdk/python/reference/class/RequestQueue.md#fetch_next_request) method.

To get info about a specific request from the queue, you can use the [`RequestQueue.get_request`](https://docs.apify.com/sdk/python/sdk/python/reference/class/RequestQueue.md#get_request) method.

### Handling requests[](#handling-requests)

To mark a request as handled, you can use the [`RequestQueue.mark_request_as_handled`](https://docs.apify.com/sdk/python/sdk/python/reference/class/RequestQueue.md#mark_request_as_handled) method.

To mark a request as not handled, so that it gets retried, you can use the [`RequestQueue.reclaim_request`](https://docs.apify.com/sdk/python/sdk/python/reference/class/RequestQueue.md#reclaim_request) method.

To check if all the requests in the queue are handled, you can use the [`RequestQueue.is_finished`](https://docs.apify.com/sdk/python/sdk/python/reference/class/RequestQueue.md#is_finished) method.

### Full example[](#full-example)

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuaW1wb3J0IHJhbmRvbVxcblxcbmZyb20gYXBpZnkgaW1wb3J0IEFjdG9yLCBSZXF1ZXN0XFxuXFxuRkFJTFVSRV9SQVRFID0gMC4zXFxuXFxuXFxuYXN5bmMgZGVmIG1haW4oKSAtPiBOb25lOlxcbiAgICBhc3luYyB3aXRoIEFjdG9yOlxcbiAgICAgICAgIyBPcGVuIHRoZSBxdWV1ZVxcbiAgICAgICAgcXVldWUgPSBhd2FpdCBBY3Rvci5vcGVuX3JlcXVlc3RfcXVldWUoKVxcblxcbiAgICAgICAgIyBBZGQgc29tZSByZXF1ZXN0cyB0byB0aGUgcXVldWVcXG4gICAgICAgIGZvciBpIGluIHJhbmdlKDEsIDEwKTpcXG4gICAgICAgICAgICBhd2FpdCBxdWV1ZS5hZGRfcmVxdWVzdChSZXF1ZXN0LmZyb21fdXJsKGYnaHR0cDovL2V4YW1wbGUuY29tL3tpfScpKVxcblxcbiAgICAgICAgIyBBZGQgYSByZXF1ZXN0IHRvIHRoZSBzdGFydCBvZiB0aGUgcXVldWUsIGZvciBwcmlvcml0eSBwcm9jZXNzaW5nXFxuICAgICAgICBhd2FpdCBxdWV1ZS5hZGRfcmVxdWVzdChSZXF1ZXN0LmZyb21fdXJsKCdodHRwOi8vZXhhbXBsZS5jb20vMCcpLCBmb3JlZnJvbnQ9VHJ1ZSlcXG5cXG4gICAgICAgICMgSWYgeW91IHRyeSB0byBhZGQgYW4gZXhpc3RpbmcgcmVxdWVzdCBhZ2FpbiwgaXQgd2lsbCBub3QgZG8gYW55dGhpbmdcXG4gICAgICAgIGFkZF9yZXF1ZXN0X2luZm8gPSBhd2FpdCBxdWV1ZS5hZGRfcmVxdWVzdChcXG4gICAgICAgICAgICBSZXF1ZXN0LmZyb21fdXJsKCdodHRwOi8vZXhhbXBsZS5jb20vNScpXFxuICAgICAgICApXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ0FkZCByZXF1ZXN0IGluZm86IHthZGRfcmVxdWVzdF9pbmZvfScpXFxuXFxuICAgICAgICAjIEZpbmFsbHksIHByb2Nlc3MgdGhlIHF1ZXVlIHVudGlsIGFsbCByZXF1ZXN0cyBhcmUgaGFuZGxlZFxcbiAgICAgICAgd2hpbGUgbm90IGF3YWl0IHF1ZXVlLmlzX2ZpbmlzaGVkKCk6XFxuICAgICAgICAgICAgIyBGZXRjaCB0aGUgbmV4dCB1bmhhbmRsZWQgcmVxdWVzdCBpbiB0aGUgcXVldWVcXG4gICAgICAgICAgICByZXF1ZXN0ID0gYXdhaXQgcXVldWUuZmV0Y2hfbmV4dF9yZXF1ZXN0KClcXG4gICAgICAgICAgICAjIFRoaXMgY2FuIGhhcHBlbiBkdWUgdG8gdGhlIGV2ZW50dWFsIGNvbnNpc3RlbmN5IG9mIHRoZSB1bmRlcmx5aW5nIHJlcXVlc3RcXG4gICAgICAgICAgICAjIHF1ZXVlIHN0b3JhZ2UsIGJlc3Qgc29sdXRpb24gaXMganVzdCB0byBzbGVlcCBhIGJpdC5cXG4gICAgICAgICAgICBpZiByZXF1ZXN0IGlzIE5vbmU6XFxuICAgICAgICAgICAgICAgIGF3YWl0IGFzeW5jaW8uc2xlZXAoMSlcXG4gICAgICAgICAgICAgICAgY29udGludWVcXG5cXG4gICAgICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ1Byb2Nlc3NpbmcgcmVxdWVzdCB7cmVxdWVzdC51bmlxdWVfa2V5fS4uLicpXFxuICAgICAgICAgICAgQWN0b3IubG9nLmluZm8oZidTY3JhcGluZyBVUkwge3JlcXVlc3QudXJsfS4uLicpXFxuXFxuICAgICAgICAgICAgIyBEbyBzb21lIGZha2Ugd29yaywgd2hpY2ggZmFpbHMgMzAlIG9mIHRoZSB0aW1lXFxuICAgICAgICAgICAgYXdhaXQgYXN5bmNpby5zbGVlcCgxKVxcbiAgICAgICAgICAgIGlmIHJhbmRvbS5yYW5kb20oKSA-IEZBSUxVUkVfUkFURTpcXG4gICAgICAgICAgICAgICAgIyBJZiBwcm9jZXNzaW5nIHRoZSByZXF1ZXN0IHdhcyBzdWNjZXNzZnVsLCBtYXJrIGl0IGFzIGhhbmRsZWRcXG4gICAgICAgICAgICAgICAgQWN0b3IubG9nLmluZm8oJ1JlcXVlc3Qgc3VjY2Vzc2Z1bC4nKVxcbiAgICAgICAgICAgICAgICBhd2FpdCBxdWV1ZS5tYXJrX3JlcXVlc3RfYXNfaGFuZGxlZChyZXF1ZXN0KVxcbiAgICAgICAgICAgIGVsc2U6XFxuICAgICAgICAgICAgICAgICMgSWYgcHJvY2Vzc2luZyB0aGUgcmVxdWVzdCB3YXMgdW5zdWNjZXNzZnVsLCByZWNsYWltIGl0IHNvIGl0IGNhbiBiZVxcbiAgICAgICAgICAgICAgICAjIHByb2Nlc3NlZCBhZ2Fpbi5cXG4gICAgICAgICAgICAgICAgQWN0b3IubG9nLndhcm5pbmcoJ1JlcXVlc3QgZmFpbGVkLCB3aWxsIHJldHJ5IScpXFxuICAgICAgICAgICAgICAgIGF3YWl0IHF1ZXVlLnJlY2xhaW1fcmVxdWVzdChyZXF1ZXN0KVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.QW5gOe0--c85jC5Izfb6QX3I7m4M6z22ufbd2AFq4QM\&asrc=run_on_apify)

```
import asyncio

import random



from apify import Actor, Request



FAILURE_RATE = 0.3





async def main() -> None:

    async with Actor:

        # Open the queue

        queue = await Actor.open_request_queue()



        # Add some requests to the queue

        for i in range(1, 10):

            await queue.add_request(Request.from_url(f'http://example.com/{i}'))



        # Add a request to the start of the queue, for priority processing

        await queue.add_request(Request.from_url('http://example.com/0'), forefront=True)



        # If you try to add an existing request again, it will not do anything

        add_request_info = await queue.add_request(

            Request.from_url('http://example.com/5')

        )

        Actor.log.info(f'Add request info: {add_request_info}')



        # Finally, process the queue until all requests are handled

        while not await queue.is_finished():

            # Fetch the next unhandled request in the queue

            request = await queue.fetch_next_request()

            # This can happen due to the eventual consistency of the underlying request

            # queue storage, best solution is just to sleep a bit.

            if request is None:

                await asyncio.sleep(1)

                continue



            Actor.log.info(f'Processing request {request.unique_key}...')

            Actor.log.info(f'Scraping URL {request.url}...')



            # Do some fake work, which fails 30% of the time

            await asyncio.sleep(1)

            if random.random() > FAILURE_RATE:

                # If processing the request was successful, mark it as handled

                Actor.log.info('Request successful.')

                await queue.mark_request_as_handled(request)

            else:

                # If processing the request was unsuccessful, reclaim it so it can be

                # processed again.

                Actor.log.warning('Request failed, will retry!')

                await queue.reclaim_request(request)





if __name__ == '__main__':

    asyncio.run(main())
```

## Storage clients[](#storage-clients)

Behind the scenes, the SDK uses storage clients to communicate with the storage backend. The appropriate client is selected automatically based on the runtime environment. On the Apify platform, data is persisted via the Apify API, while local runs use the filesystem. For most use cases, you don't need to think about storage clients at all. To learn about the available implementations, how to switch between a single and shared request queue, or how to configure a custom client, see [Storage clients](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/storage-clients.md). For a deeper look at how storage clients work internally, see the [Crawlee storage clients guide](https://crawlee.dev/python/docs/guides/storage-clients).
