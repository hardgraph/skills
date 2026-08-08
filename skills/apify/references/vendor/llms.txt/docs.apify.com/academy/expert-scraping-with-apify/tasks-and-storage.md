---
title: "Tasks & storage"
url: https://docs.apify.com/academy/expert-scraping-with-apify/tasks-and-storage.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Expert scraping with Apify](https://docs.apify.com/academy/expert-scraping-with-apify.md)
previous: [II - Managing source code](https://docs.apify.com/academy/expert-scraping-with-apify/managing-source-code.md)
next: [IV - Apify API & client](https://docs.apify.com/academy/expert-scraping-with-apify/apify-api-and-client.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Tasks & storage

**Understand how to save the configurations for Actors with Actor tasks. Also, learn about storage and the different types Apify offers.**

***

Both of these are very different things; however, they are also tied together in many ways. **Tasks** run Actors, Actors return data, and data is stored in different types of **Storages**.

## Tasks

Tasks are a very useful feature which allow us to save pre-configured inputs for Actors. This means that rather than configuring the Actor every time, or rather than having to save screenshots of various different Actor configurations, you can store the configurations right in your Apify account instead, and run the Actor at will with them.

## Storage

Storage allows us to save persistent data for further processing. As you'll learn, there are two main storage options on the Apify platform, as well as two main storage types (**named** and **unnamed**) with one big difference between them.

## Learning 🧠

* Check out [the docs about Actor tasks](https://docs.apify.com/actors/running/tasks.md).
* Read about the [two main storage options](https://docs.apify.com/storage/dataset.md) on the Apify platform.
* Understand the [differences between named and unnamed storages](https://docs.apify.com/storage.md#named-and-unnamed-storages).
* Learn about the [Dataset](https://docs.apify.com/sdk/js/reference/class/Dataset) and [KeyValueStore](https://docs.apify.com/sdk/js/reference/class/KeyValueStore) objects in the Apify SDK.

## Knowledge check 📝

1. What is the relationship between Actors and tasks?
2. What are the differences between default (unnamed) and named storage? Which one would you use for everyday usage?
3. What is data retention, and how does it work for all types of storages (default and named)?

[Solution](https://docs.apify.com/academy/expert-scraping-with-apify/solutions/using-storage-creating-tasks.md)

## Next up

The [next lesson](https://docs.apify.com/academy/expert-scraping-with-apify/apify-api-and-client.md) is very exciting, as it will unlock the ability to seamlessly integrate your Apify Actors into your own external projects and applications with the Apify API.
