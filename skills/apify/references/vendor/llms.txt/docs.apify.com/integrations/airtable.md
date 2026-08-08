---
title: Airtable integration
url: https://docs.apify.com/integrations/airtable.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Data and storage](https://docs.apify.com/integrations/data-and-storage.md)
previous: [Airbyte](https://docs.apify.com/integrations/airbyte.md)
next: [Google Drive](https://docs.apify.com/integrations/drive.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Airtable integration

The [Airtable Data Import Actor](https://console.apify.com/actors/f4DM1wGmMQdnTLbrE/info/readme?build=latest) transfers items from any Apify dataset into an [Airtable](https://www.airtable.com/) base. Use it standalone or chain it after other Actors in automated workflows via [integrations](https://docs.apify.com/integrations.md).

Help keep this page up to date

This integration uses a third-party service. If you find outdated content, please [submit an issue on GitHub](https://github.com/apify/apify-docs/issues).

## Prerequisites

* An [Apify account](https://console.apify.com/)
* An [Airtable account](https://www.airtable.com/)
* The [Airtable Data Import Actor](https://console.apify.com/actors/f4DM1wGmMQdnTLbrE/info/readme?build=latest)

## Connect to Airtable

The Actor uses OAuth 2.0 to connect to your Airtable account:

1. Navigate to the Actor's **Integrations** tab in Apify Console.
2. Click **Connect** next to Airtable and follow the OAuth consent flow.
3. Once connected, the OAuth account field is populated automatically.

![Apify Console Integrations tab showing Airtable OAuth connection screen](/assets/images/airtable_console_oauth-42e71ecbc7f44ae2a1db1b3a1b213c3d.webp)

The Actor retrieves a fresh access token at the start of each run. Your Airtable credentials are never stored or logged.

## Configure the Actor

Configure the Actor from its **Input** tab in Apify Console. Each field is described below.

### Operation

Controls how the Actor handles the target table and its existing records.

* `Append`: Adds new records to the table. Existing records are never modified or deleted. Use for continuous pipelines.
* `Override`: Deletes all existing records before importing. Use for full data refreshes.
* `Create`: Creates a new table with fields from your mappings, then imports data. If the table already exists, the `clearOnCreate` setting determines what happens.

### Clear on create

A safety switch for `Create` mode only. Defaults to `false`.

* `Enabled` (`true`): Clears all existing records and imports. Use for automated fresh starts.
* `Disabled` (`false`): Throws an error if the table exists, preventing accidental data loss.

### Base

Accepts a base ID (`appXXXXXXXXXXXXXX`, found in the Airtable URL) or a base name. Base IDs are recommended - they are immutable and globally unique.

![Airtable base URL highlighting the base ID portion](/assets/images/airtable_console_app_id-4834f3da8fde34c6e0c112506defc2d2.webp)

### Table

Accepts a table name or table ID (`tblXXXXXXXXXXXXXX`). When using `Create` mode and the table does not exist, the Actor creates it automatically.

### Dataset ID

Specifies the dataset to import from:

* A static dataset ID (for example, `cqxkhXcn2SCjTpeCz`).
* `{{resource.defaultDatasetId}}` when used as an integration - automatically passes the upstream Actor's dataset.

### Unique ID

Enables duplicate detection. When set, the Actor reads existing values from the mapped Airtable column and skips records with matching values. The value must match one of the `source` fields in your `dataMappings`.

### Field mappings

An array defining how dataset fields map to Airtable columns.

![Field mappings editor showing multiple mapping rows configured](/assets/images/airtable_console_field_mapping-d2fde3e7a7fb8b74c967126672cbd19d.webp)

Each mapping has these properties:

| Property     | Description                                                                                  |
| ------------ | -------------------------------------------------------------------------------------------- |
| `source`     | Dataset field path, supports dot notation for nested objects (`crawl.depth`, `metadata.uid`) |
| `target`     | Airtable column name                                                                         |
| `targetType` | `existing` if the field already exists, `new` to create it automatically                     |
| `fieldType`  | Airtable field type: `singleLineText`, `multilineText`, `number`, `checkbox`                 |

#### Dot notation

Use dot notation to access nested object properties. `items[0].name` style array indexing is not supported.

#### Field types

| Field type       | Use for                                     |
| ---------------- | ------------------------------------------- |
| `singleLineText` | Names, URLs, IDs (max 10,000 characters)    |
| `multilineText`  | Descriptions, paragraphs, JSON blobs        |
| `number`         | Prices, counts, ratings (integer precision) |
| `checkbox`       | Boolean values, yes/no flags                |

Number precision

The Actor creates `number` fields with integer precision. For decimal values, create the field manually in Airtable and use `targetType: "existing"`.

## Import results from another Actor automatically

Chain the Actor after a scraping or extraction Actor so its dataset flows into Airtable on every run. Use this for scheduled scrapers, recurring price checks, or any pipeline where fresh data should reach your base without a manual trigger.

1. Open the upstream Actor in Apify Console and go to the **Integrations** tab.
2. Click **Add integration** and select **Airtable Data Import Actor**.
3. Set `datasetId` to `{{resource.defaultDatasetId}}`.
4. Configure the base, table, operation, and field mappings.
5. Connect your Airtable account via OAuth.

![Apify integration setup screen showing Airtable Data Import Actor configuration](data:image/webp;base64,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)

## Example of the output

After each run, the Actor writes a JSON summary to its dataset. Each field reports a key result from the import operation:


```json
{

    "success": true,

    "operation": "Append",

    "baseName": "My Product Catalog",

    "tableName": "Products",

    "totalItems": 1500,

    "importedCount": 1450,

    "skippedDuplicates": 50,

    "duration": 330

}
```


## Example configurations

Append with duplicate detection


```json
{

    "operation": "Append",

    "base": "appABC123456789",

    "table": "Products",

    "datasetId": "{{resource.defaultDatasetId}}",

    "uniqueId": "url",

    "dataMappings": [

        { "source": "title", "target": "Product Name", "targetType": "existing", "fieldType": "singleLineText" },

        { "source": "price", "target": "Price", "targetType": "existing", "fieldType": "number" },

        { "source": "url", "target": "URL", "targetType": "existing", "fieldType": "singleLineText" },

        { "source": "description", "target": "Description", "targetType": "existing", "fieldType": "multilineText" },

        { "source": "inStock", "target": "In Stock", "targetType": "existing", "fieldType": "checkbox" }

    ]

}
```


Create new table


```json
{

    "operation": "Create",

    "clearOnCreate": false,

    "base": "appXYZ987654321",

    "table": "Customer Feedback",

    "datasetId": "cqxkhXcn2SCjTpeCz",

    "dataMappings": [

        { "source": "reviewer.name", "target": "Reviewer Name", "targetType": "new", "fieldType": "singleLineText" },

        { "source": "rating", "target": "Rating", "targetType": "new", "fieldType": "number" },

        { "source": "comment", "target": "Review Text", "targetType": "new", "fieldType": "multilineText" },

        { "source": "verified", "target": "Verified Purchase", "targetType": "new", "fieldType": "checkbox" }

    ]

}
```


Override for full refresh


```json
{

    "operation": "Override",

    "base": "Daily Competitor Prices",

    "table": "Pricing",

    "datasetId": "{{resource.defaultDatasetId}}",

    "dataMappings": [

        { "source": "sku", "target": "SKU", "targetType": "existing", "fieldType": "singleLineText" },

        { "source": "competitor", "target": "Competitor", "targetType": "existing", "fieldType": "singleLineText" },

        { "source": "price", "target": "Price", "targetType": "existing", "fieldType": "number" }

    ]

}
```


## Limits

* Text values are truncated at 10,000 characters per field.
* `number` fields are created with integer precision; create decimal fields manually.
* Imports process at approximately 50 records per second (10 per batch, 200ms delay).
* Progress is automatically persisted - if a run is interrupted, it resumes from where it left off.

## Next steps

* [Learn about Apify integrations](https://docs.apify.com/integrations.md) to chain Actors in automated workflows.
* [Explore Airtable's API documentation](https://airtable.com/developers/web/api/field-model) for field type details.
* If you have questions about the Airtable integration, reach out via the support live chat in Console or the [developer community on Discord](https://discord.com/invite/jyEM2PRvMU).
