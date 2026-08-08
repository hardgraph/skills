---
title: Key-value store
url: https://docs.apify.com/storage/key-value-store.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Storage](https://docs.apify.com/storage.md)
children:
  - [Key-value store schema](https://docs.apify.com/storage/key-value-store-schema.md)
previous: [Multiple datasets](https://docs.apify.com/storage/dataset-schema/multiple-datasets.md)
next: [Key-value store schema](https://docs.apify.com/storage/key-value-store-schema.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Key-value store

The key-value store is simple storage that can be used for storing any kind of data. It can be JSON or HTML documents, zip files, images, or strings. The data are stored along with their [MIME content type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types).

Each Actor run is assigned its own key-value store when it is created. The store contains the Actor's input, and, if necessary, other data such as its output.

Key-value stores are mutable - you can both add entries and delete them.

Retention period

Named key-value stores are retained indefinitely. Unnamed key-value stores expire after 7 days unless otherwise specified. [Learn more](https://docs.apify.com/storage.md#named-and-unnamed-storages)

![Key-value store graphic](/assets/images/key-value-overview-cc78407f157ecfa2b9ccb942e59d54b6.svg)

## Basic usage

You can access key-value stores through several methods

* [Apify Console](https://console.apify.com) - provides an easy-to-understand interface.
* [Apify API](https://docs.apify.com/api/v2.md) - for accessing your key-value stores programmatically.
* [Apify API clients](https://docs.apify.com/api.md) - to access your key-value stores from any Node.js/Python application.
* [Apify SDKs](https://docs.apify.com/sdk.md) - when building your own JavaScript/Python Actor.

### Apify Console

In [Apify Console](https://console.apify.com), you can view your key-value stores in the [Storage](https://console.apify.com/storage) section under the [Key-value stores](https://console.apify.com/storage?tab=keyValueStores) tab.

![Key-value stores in app](/assets/images/key-value-stores-app-e32c8eb13addf4990370a0b02b7f3919.png)

To view a key-value store's content, click on its **Store ID**. Under the **Actions** menu, you can rename your store (which extends its [retention period](https://docs.apify.com/storage.md#named-and-unnamed-storages)) and grant [access rights](https://docs.apify.com/account/collaboration.md) using the **Share** button. Click on the **API** button to view and test a store's [API endpoints](https://docs.apify.com/api/v2/storage-key-value-stores.md).

![Key-value stores detail](/assets/images/key-value-stores-detail-header-065c0dbe9b1522325fb960e2a04069e5.png)

At the bottom of the page, you can work with records in your key-value store:

* Upload one or more new records and set their keys.
* View individual records.
* Download individual records, or download all records at once.
* Copy shareable links to records.
* Delete records.

![Key-value stores detail](/assets/images/key-value-stores-detail-records-f6c4e8a4fc702ac81ec0fb4578538c3c.png)

### Apify API

The [Apify API](https://docs.apify.com/api/v2/storage-key-value-stores.md) enables you programmatic access to your key-value stores using [HTTP requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods).

If you are accessing your key-value stores using the `username~store-name` [store ID format](https://docs.apify.com/storage.md), you will need to use your secret API token. You can find the token (and your user ID) on the [API & Integrations](https://console.apify.com/settings/integrations) tab of **Settings** page of your Apify account.

Authentication

When providing your API authentication token, we recommend using the request's `Authorization` header, rather than the URL. For more information, refer to the [API integration](https://docs.apify.com/integrations/api.md#authentication) documentation.

To retrieve a list of your key-value stores, send a GET request to the [Get list of key-value stores](https://docs.apify.com/api/v2/key-value-stores-get.md) endpoint.


```text
https://api.apify.com/v2/key-value-stores
```


To get information about a key-value store such as its creation time and item count, send a GET request to the [Get store](https://docs.apify.com/api/v2/key-value-store-get.md) endpoint.


```text
https://api.apify.com/v2/key-value-stores/{STORE_ID}
```


To get a record (its value) from a key-value store, send a GET request to the [Get record](https://docs.apify.com/api/v2/key-value-store-record-get.md) endpoint.


```text
https://api.apify.com/v2/key-value-stores/{STORE_ID}/records/{KEY_ID}
```


To add a record with a specific key in a key-value store, send a PUT request to the [Store record](https://docs.apify.com/api/v2/key-value-store-record-put.md) endpoint.


```text
https://api.apify.com/v2/key-value-stores/{STORE_ID}/records/{KEY_ID}
```


Example payload:


```json
{

    "foo": "bar",

    "fos": "baz"

}
```


To delete a record, send a DELETE request specifying the key from a key-value store to the [Delete record](https://docs.apify.com/api/v2/key-value-store-record-delete.md) endpoint.


```text
https://api.apify.com/v2/key-value-stores/{STORE_ID}/records/{KEY_ID}
```


For further details and a breakdown of each storage API endpoint, refer to the [API documentation](https://docs.apify.com/api/v2/storage-key-value-stores.md).

### Apify API Clients

#### JavaScript API client

The Apify [JavaScript API client](https://docs.apify.com/api/client/js/reference/class/KeyValueStoreClient) (`apify-client`) enables you to access your key-value stores from any Node.js application, whether hosted on the Apify platform or externally.

After importing and initiating the client, you can save each key-value store to a variable for easier access.


```js
const myKeyValStoreClient = apifyClient.keyValueStore(

    'jane-doe/my-key-val-store',

);
```


You can then use that variable to [access the key-value store's items and manage it](https://docs.apify.com/api/client/js/reference/class/KeyValueStoreClient).

Check out the [JavaScript API client documentation](https://docs.apify.com/api/client/js/reference/class/KeyValueStoreClient) for [help with setup](https://docs.apify.com/api/client/js/docs) and more details.

#### Python API client

The Apify [Python API client](https://docs.apify.com/api/client/python/reference/class/KeyValueStoreClient) (`apify-client`) allows you to access your key-value stores from any Python application, whether it is running on the Apify platform or externally.

After importing and initiating the client, you can save each key-value store to a variable for easier access.


```python
my_key_val_store_client = apify_client.key_value_store('jane-doe/my-key-val-store')
```


You can then use that variable to [access the key-value store's items and manage it](https://docs.apify.com/api/client/python/reference/class/KeyValueStoreClient).

Check out the [Python API client documentation](https://docs.apify.com/api/client/python/reference/class/KeyValueStoreClient) for [help with setup](https://docs.apify.com/api/client/python/docs/overview/introduction) and more details.

### Apify SDKs

#### JavaScript SDK

When working with a JavaScript [Actor](https://docs.apify.com/actors.md), the [JavaScript SDK](https://docs.apify.com/sdk/js/docs/guides/result-storage#key-value-store) is an essential tool, especially for key-value store management. The primary class for this purpose is the [KeyValueStore](https://docs.apify.com/sdk/js/reference/class/KeyValueStore). This class allows you to decide whether your data will be stored locally or in the Apify cloud. For data manipulation, it offers the [getValue()](https://docs.apify.com/sdk/js/reference/class/KeyValueStore#getValue) and [setValue()](https://docs.apify.com/sdk/js/reference/class/KeyValueStore#setValue) methods to retrieve and assign values, respectively.

Additionally, you can iterate over the keys in your store using the [forEachKey()](https://docs.apify.com/sdk/js/reference/class/KeyValueStore#forEachKey) method.

Every Actor run is linked to a default key-value store that is automatically created for that specific run. If you're running your Actors and opt to store data locally, you can easily supply the [input](https://docs.apify.com/actors/running/input-and-output.md) by placing an *INPUT.json* file in the corresponding directory of the default key-value store. This method ensures that your Actor has all the necessary data readily available for its execution.

You can find *INPUT.json* and other key-value store files in the location below.


```text
{APIFY_LOCAL_STORAGE_DIR}/key_value_stores/{STORE_ID}/{KEY}.{EXT}
```


The default key-value store's ID is *default*. The `{KEY}` is the record's *key* and `{EXT}` corresponds to the record value's MIME content type.

To manage your key-value stores, you can use the following methods. See the `KeyValueStore` class's [API reference](https://docs.apify.com/sdk/js/reference/class/KeyValueStore) for the full list.


```js
import { Actor } from 'apify';



await Actor.init();

// ...



// Get the default input

const input = await Actor.getInput();



// Open a named key-value store

const exampleStore = await Actor.openKeyValueStore('my-store');



// Read a record in the exampleStore storage

const value = await exampleStore.getValue('some-key');



// Write a record to exampleStore

await exampleStore.setValue('some-key', { foo: 'bar' });



// Delete a record from exampleStore

await exampleStore.setValue('some-key', null);



// ...

await Actor.exit();
```


Automatic parsing

JSON is automatically parsed to a JavaScript object, text data is returned as a string, and other data is returned as a binary buffer.


```js
import { Actor } from 'apify';



await Actor.init();

// ...



// Get input of your Actor

const input = await Actor.getInput();

const value = await Actor.getValue('my-key');



// ...

await Actor.setValue('OUTPUT', imageBuffer, { contentType: 'image/jpeg' });



// ...

await Actor.exit();
```


The `Actor.getInput()` method is not only a shortcut to `Actor.getValue('INPUT')`; it is also compatible with [Actor.metamorph()](https://docs.apify.com/actors/development/programming-interface/metamorph.md). This is because a metamorphed Actor run's input is stored in the *INPUT-METAMORPH-1* key instead of *INPUT*, which hosts the original input.

Check out the [JavaScript SDK documentation](https://docs.apify.com/sdk/js/docs/guides/result-storage#key-value-store) and the `KeyValueStore` class's [API reference](https://docs.apify.com/sdk/js/reference/class/KeyValueStore) for details on managing your key-value stores with the JavaScript SDK.

#### Python SDK

For Python [Actor](https://docs.apify.com/actors.md), the [Python SDK](https://docs.apify.com/sdk/python/docs/concepts/storages#working-with-key-value-stores) is essential. The key-value store is represented by a [KeyValueStore](https://docs.apify.com/sdk/python/reference/class/KeyValueStore) class. You can use this class to specify whether your data is stored locally or in the Apify cloud. For further data manipulation it offers [get_value()](https://docs.apify.com/sdk/python/reference/class/KeyValueStore#get_value) and [set_value()](https://docs.apify.com/sdk/python/reference/class/KeyValueStore#set_value) methods to retrieve and assign values, respectively.

Every Actor run is linked to a default key-value store that is automatically created for that specific run. If you're running your Actors and opt to store data locally, you can easily supply the [input](https://docs.apify.com/actors/running/input-and-output.md) by placing an *INPUT.json* file in the corresponding directory of the default key-value store. This method ensures that your Actor has all the necessary data readily available for its execution.

You can find *INPUT.json* and other key-value store files in the location below.


```text
{APIFY_LOCAL_STORAGE_DIR}/key_value_stores/{STORE_ID}/{KEY}.{EXT}
```


The default key-value store's ID is *default*. The `{KEY}` is the record's *key* and `{EXT}` corresponds to the record value's MIME content type.

To manage your key-value stores, you can use the following methods. See the `KeyValueStore` class [documentation](https://docs.apify.com/sdk/python/reference/class/KeyValueStore) for the full list.


```python
from apify import Actor

from apify.storages import KeyValueStore



async def main():

    async with Actor:

        # Open a named key-value store

        example_store: KeyValueStore = await Actor.open_key_value_store(name='my-store')



        # Read a record in the example_store storage

        value = await example_store.get_value('some-key')



        # Write a record to example_store

        await example_store.set_value('some-key', {'foo': 'bar'})



        # Delete a record from example_store

        await example_store.set_value('some-key', None)
```


Automatic parsing

JSON is automatically parsed to a Python dictionary, text data is returned as a string, and other data is returned as a binary buffer.


```python
from apify import Actor



async def main():

    async with Actor:

        value = await Actor.get_value('my-key')

        # ...

        image_buffer = ...  # Get image data

        await Actor.set_value(key='OUTPUT', value=image_buffer, content_type='image/jpeg')
```


The `Actor.get_input()` method is not only a shortcut to `Actor.get_value('INPUT')`; it is also compatible with [Actor.metamorph()](https://docs.apify.com/actors/development/programming-interface/metamorph.md). This is because a metamorphed Actor run's input is stored in the *INPUT-METAMORPH-1* key instead of *INPUT*, which hosts the original input.

Check out the [Python SDK documentation](https://docs.apify.com/sdk/python/docs/concepts/storages#working-with-key-value-stores) and the `KeyValueStore` class's [API reference](https://docs.apify.com/sdk/python/reference/class/KeyValueStore) for details on managing your key-value stores with the Python SDK.

## Compression

Previously, when using the [Store record](https://docs.apify.com/api/v2/key-value-store-record-put.md) endpoint, every record was automatically compressed with Gzip before being uploaded. However, this process has been updated. *Now, records are stored exactly as you upload them.* This change means that it is up to you whether the record is stored compressed or uncompressed.

You can compress a record and use the [Content-Encoding request header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Encoding) to let the platform know which compression it uses. We recommend compressing large key-value records to save storage space and network traffic.

*Using the [JavaScript SDK](https://docs.apify.com/sdk/js/reference/class/KeyValueStore#setValue) or the [JavaScript API client](https://docs.apify.com/api/client/js/reference/class/KeyValueStoreClient#setRecord) automatically compresses your files.* We advise utilizing the JavaScript API client for data compression prior to server upload and decompression upon retrieval, minimizing storage costs.

## Share

You can grant [access rights](https://docs.apify.com/account/collaboration.md) to your key-value store through the **Share** button under the **Actions** menu. For more details check the [full list of permissions](https://docs.apify.com/account/collaboration/list-of-permissions.md).

You can also share key-value stores by link using their ID or name, depending on your account or resource-level general access setting. Learn how link-based access works in [General resource access](https://docs.apify.com/account/collaboration/general-resource-access.md).

For one-off sharing of specific records when access is restricted, you can generate time-limited pre-signed URLs. See [Sharing restricted resources with pre-signed URLs](https://docs.apify.com/account/collaboration/general-resource-access.md#pre-signed-urls).

### Share key-value stores between runs

You can access a key-value store from any [Actor](https://docs.apify.com/actors.md) or [task](https://docs.apify.com/actors/running/tasks.md) run as long as you know its *name* or *ID*.

To access a key-value store from another run using the [Apify SDK](https://docs.apify.com/sdk.md), open it using the same method as you would do with any other store.

**JavaScript**


```js
import { Actor } from 'apify';



await Actor.init();



const otherStore = await Actor.openKeyValueStore('old-store');

// ...



await Actor.exit();
```


**Python**


```python
from apify import Actor



async def main():

    async with Actor:

        other_store = await Actor.open_key_value_store(name='old-store')

        # ...
```


In the [JavaScript API client](https://docs.apify.com/api/client/js/reference/class/KeyValueStoreClient) as well as in [Python API client](https://docs.apify.com/api/client/python/reference/class/KeyValueStoreClient), you can access a store using its client. Once you've opened a store, read and manage its contents like you would do with a key-value store from your current run.

**JavaScript**


```js
const otherStoreClient = apifyClient.keyValueStore('jane-doe/old-store');
```


**Python**


```python
other_store_client = apify_client.key_value_store('jane-doe/old-store')
```


The same applies for the Apify API - you can use the same endpoints as you would normally do.

Check out the [Storage overview](https://docs.apify.com/storage.md#share-storages-between-runs) for details on sharing storages between runs.

## Data consistency

Key-value storage uses the [AWS S3](https://aws.amazon.com/s3/) service. According to the [S3 documentation](https://aws.amazon.com/s3/consistency/), it provides *strong read-after-write* consistency.

## Limits

* The maximum length for a key in a key-value store is 256 characters. Keys may only contain the following characters: `a-zA-Z0-9!-_.'()`.
