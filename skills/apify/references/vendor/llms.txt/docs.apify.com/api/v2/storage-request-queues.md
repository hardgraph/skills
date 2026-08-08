---
title: Request queues
url: https://docs.apify.com/api/v2/storage-request-queues.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify API documentation](https://docs.apify.com/api.md)
  - [Apify API](https://docs.apify.com/api/v2.md)
children:
  - [Get list of request queues](https://docs.apify.com/api/v2/request-queues-get.md)
  - [Create request queue](https://docs.apify.com/api/v2/request-queues-post.md)
  - [Get request queue](https://docs.apify.com/api/v2/request-queue-get.md)
  - [Update request queue](https://docs.apify.com/api/v2/request-queue-put.md)
  - [Delete request queue](https://docs.apify.com/api/v2/request-queue-delete.md)
  - [Add requests](https://docs.apify.com/api/v2/request-queue-requests-batch-post.md)
  - [Delete requests](https://docs.apify.com/api/v2/request-queue-requests-batch-delete.md)
previous: [Delete record](https://docs.apify.com/api/v2/key-value-store-record-delete.md)
next: [Get list of request queues](https://docs.apify.com/api/v2/request-queues-get.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Request queues

This section describes API endpoints to create, manage, and delete request queues.

Request queue is a storage for a queue of HTTP URLs to crawl, which is typically used for deep crawling of websites where you start with several URLs and then recursively follow links to other pages. The storage supports both breadth-first and depth-first crawling orders.

For more information, see the [Request queue documentation](https://docs.apify.com/platform/storage/request-queue).

note

Some of the endpoints do not require the authentication token, the calls are authenticated using the hard-to-guess ID of the queue.

<!-- -->

## [Get list of request queues](https://docs.apify.com/api/v2/request-queues-get.md)

[/request-queues](https://docs.apify.com/api/v2/request-queues-get.md)

## [Create request queue](https://docs.apify.com/api/v2/request-queues-post.md)

[/request-queues](https://docs.apify.com/api/v2/request-queues-post.md)

## [Get request queue](https://docs.apify.com/api/v2/request-queue-get.md)

[/request-queues/{queueId}](https://docs.apify.com/api/v2/request-queue-get.md)

## [Update request queue](https://docs.apify.com/api/v2/request-queue-put.md)

[/request-queues/{queueId}](https://docs.apify.com/api/v2/request-queue-put.md)

## [Delete request queue](https://docs.apify.com/api/v2/request-queue-delete.md)

[/request-queues/{queueId}](https://docs.apify.com/api/v2/request-queue-delete.md)

## [Add requests](https://docs.apify.com/api/v2/request-queue-requests-batch-post.md)

[/request-queues/{queueId}/requests/batch](https://docs.apify.com/api/v2/request-queue-requests-batch-post.md)

## [Delete requests](https://docs.apify.com/api/v2/request-queue-requests-batch-delete.md)

[/request-queues/{queueId}/requests/batch](https://docs.apify.com/api/v2/request-queue-requests-batch-delete.md)
