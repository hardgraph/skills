---
title: Datasets
url: https://docs.apify.com/api/v2/storage-datasets.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify API documentation](https://docs.apify.com/api.md)
  - [Apify API](https://docs.apify.com/api/v2.md)
children:
  - [Get list of datasets](https://docs.apify.com/api/v2/datasets-get.md)
  - [Create dataset](https://docs.apify.com/api/v2/datasets-post.md)
  - [Get dataset](https://docs.apify.com/api/v2/dataset-get.md)
  - [Update dataset](https://docs.apify.com/api/v2/dataset-put.md)
  - [Delete dataset](https://docs.apify.com/api/v2/dataset-delete.md)
  - [Get dataset items](https://docs.apify.com/api/v2/dataset-items-get.md)
  - [Get dataset items headers](https://docs.apify.com/api/v2/dataset-items-head.md)
  - [Store items](https://docs.apify.com/api/v2/dataset-items-post.md)
  - [Get dataset statistics](https://docs.apify.com/api/v2/dataset-statistics-get.md)
previous: [Get last run](https://docs.apify.com/api/v2/actor-task-runs-last-get.md)
next: [Get list of datasets](https://docs.apify.com/api/v2/datasets-get.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Datasets

This section describes API endpoints to manage Datasets.

Dataset is a storage for structured data, where each record stored has the same attributes, such as online store products or real estate offers. You can imagine it as a table, where each object is a row and its attributes are columns. Dataset is an append-only storage - you can only add new records to it but you cannot modify or remove existing records. Typically it is used to store crawling results.

For more information, see the [Datasets documentation](https://docs.apify.com/platform/storage/dataset).

note

Some of the endpoints do not require the authentication token, the calls are authenticated using the hard-to-guess ID of the dataset.

<!-- -->

## [Get list of datasets](https://docs.apify.com/api/v2/datasets-get.md)

[/datasets](https://docs.apify.com/api/v2/datasets-get.md)

## [Create dataset](https://docs.apify.com/api/v2/datasets-post.md)

[/datasets](https://docs.apify.com/api/v2/datasets-post.md)

## [Get dataset](https://docs.apify.com/api/v2/dataset-get.md)

[/datasets/{datasetId}](https://docs.apify.com/api/v2/dataset-get.md)

## [Update dataset](https://docs.apify.com/api/v2/dataset-put.md)

[/datasets/{datasetId}](https://docs.apify.com/api/v2/dataset-put.md)

## [Delete dataset](https://docs.apify.com/api/v2/dataset-delete.md)

[/datasets/{datasetId}](https://docs.apify.com/api/v2/dataset-delete.md)

## [Get dataset items](https://docs.apify.com/api/v2/dataset-items-get.md)

[/datasets/{datasetId}/items](https://docs.apify.com/api/v2/dataset-items-get.md)

## [Get dataset items headers](https://docs.apify.com/api/v2/dataset-items-head.md)

[/datasets/{datasetId}/items](https://docs.apify.com/api/v2/dataset-items-head.md)

## [Store items](https://docs.apify.com/api/v2/dataset-items-post.md)

[/datasets/{datasetId}/items](https://docs.apify.com/api/v2/dataset-items-post.md)

## [Get dataset statistics](https://docs.apify.com/api/v2/dataset-statistics-get.md)

[/datasets/{datasetId}/statistics](https://docs.apify.com/api/v2/dataset-statistics-get.md)
