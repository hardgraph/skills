---
title: Multiple datasets
url: https://docs.apify.com/storage/dataset-schema/multiple-datasets.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Storage](https://docs.apify.com/storage.md)
  - [Dataset](https://docs.apify.com/storage/dataset.md)
previous: [Dataset validation](https://docs.apify.com/storage/dataset-schema/validation.md)
next: [Key-value store](https://docs.apify.com/storage/key-value-store.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Multiple datasets

Actors that scrape different data types can store each type in its own dataset with separate validation rules. For example, an e-commerce scraper might store products in one dataset and categories in another.

Each dataset:

* Is created when the run starts
* Follows the run's data retention policy
* Can have its own validation schema

## Define multiple datasets

Define datasets in your Actor schema using the `datasets` object:

.actor/actor.json


```json
{

    "actorSpecification": 1,

    "name": "my-e-commerce-scraper",

    "title": "E-Commerce Scraper",

    "version": "1.0.0",

    "storages": {

        "datasets": {

            "default": "./products_dataset_schema.json",

            "categories": "./categories_dataset_schema.json"

        }

    }

}
```


Provide schemas for individual datasets as file references or inline. Schemas follow the same structure as single-dataset schemas.

The keys of the `datasets` object are aliases that refer to specific datasets. The previous example defines two datasets aliased as `default` and `categories`.

Requirements:

* The `datasets` object must contain the `default` alias
* The `datasets` and `dataset` objects are mutually exclusive (use one or the other)

Alias versus named dataset

On the Apify platform, aliases and names behave differently. Named datasets are persistent. The automatic data retention policy doesn't apply to them. Aliased datasets follow the data retention of their run, and aliases only have meaning within a specific run.

Behavior differs when an SDK runs outside the platform. See the SDK notes below.

See the full [Actor schema reference](https://docs.apify.com/actors/development/actor-definition/actor-json.md#reference).

## Access datasets in Actor code

Access aliased datasets through the Apify SDK or by reading the `ACTOR_STORAGES_JSON` environment variable directly.

### Apify SDK

**JavaScript**

In the JavaScript/TypeScript SDK `>=3.7.0`, use [Actor.openDataset](https://docs.apify.com/sdk/js/reference/class/Actor#openDataset) with the `alias` option:


```js
const categoriesDataset = await Actor.openDataset({alias: 'categories'});
```


Running outside the Apify platform

When the JavaScript SDK runs outside the Apify platform, aliases fall back to names (using an alias is the same as using a named dataset). The dataset is purged on the first access when accessed using the `alias` option.

**Python**

In the Python SDK `>=3.3.0`, use [Actor.open_dataset](https://docs.apify.com/sdk/python/reference/class/Actor#open_dataset) with the `alias` parameter:


```py
categories_dataset = await Actor.open_dataset(alias='categories')
```


Running outside the Apify platform

When the Python SDK runs outside the Apify platform, it uses the [Crawlee for Python aliasing mechanism](https://crawlee.dev/python/docs/guides/storages#named-and-unnamed-storages). Aliases are created as unnamed and purged on Actor start.

### Environment variable

`ACTOR_STORAGES_JSON` contains JSON-encoded unique identifiers of all storages associated with the current Actor run. Use this approach when working without the SDK:


```sh
echo $ACTOR_STORAGES_JSON | jq '.datasets.categories'

# This will output id of the categories dataset, e.g. `"3ZojQDdFTsyE7Moy4"`
```


## View and export datasets

The **Storage** tab in the Actor run view displays all datasets defined by the Actor and used by the run (up to 10).

To export a non-default dataset:

1. On the Actor run page, select the **Storage** tab.
2. Open the **Dataset** dropdown and select the dataset you want to export.
3. Under **Export dataset**, choose a format: JSON, CSV, XML, Excel, HTML Table, RSS, or JSONL.
4. Select **Download**.

Run page Export button

The **Export** button on the Run page exports only the `default` dataset.

To export programmatically:

* Call the [Dataset API](https://docs.apify.com/api/v2/dataset-items-get.md) with the dataset ID from `ACTOR_STORAGES_JSON`. The API returns items in any supported format via query parameters.
* From inside an Actor, open the dataset (see Access datasets in Actor code), then call `getData` / `get_data` to read items into memory, or `exportTo` / `export_to` to write a JSON or CSV file to the key-value store.

See [Datasets](https://docs.apify.com/storage/dataset.md) for formats and query parameters.

## Surface datasets on the run page

The Storage tab shows data but doesn't surface it clearly to end users. To present datasets more prominently on the run page, define an [output schema](https://docs.apify.com/actors/development/actor-definition/output-schema.md) that references each dataset by alias:


```json
{

    "actorOutputSchemaVersion": 1,

    "title": "Output schema",

    "properties": {

        "products": {

            "type": "string",

            "title": "Products",

            "template": "{{storages.datasets.default.apiUrl}}/items"

        },

        "categories": {

            "type": "string",

            "title": "Categories",

            "template": "{{storages.datasets.categories.apiUrl}}/items"

        }

    }

}
```


[Read more](https://docs.apify.com/actors/development/actor-definition/output-schema.md#how-templates-work) about how templates work.

## Billing for non-default datasets

When an Actor uses multiple datasets, only items pushed to the `default` dataset trigger the built-in `apify-default-dataset-item` event. Items in other datasets are not charged automatically.

To charge for items in other datasets, implement custom billing in your Actor code. Refer to the [billing documentation](https://docs.apify.com/actors/publishing/monetize/pay-per-event.md) for implementation details.
