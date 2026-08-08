---
title: "Apify API & client"
url: https://docs.apify.com/academy/expert-scraping-with-apify/apify-api-and-client.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Expert scraping with Apify](https://docs.apify.com/academy/expert-scraping-with-apify.md)
previous: [III - Tasks & storage](https://docs.apify.com/academy/expert-scraping-with-apify/tasks-and-storage.md)
next: [V - Migrations & maintaining state](https://docs.apify.com/academy/expert-scraping-with-apify/migrations-maintaining-state.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Apify API & client

**Gain an in-depth understanding of the two main ways of programmatically interacting with the Apify platform - through the API, and through a client.**

***

You can use one of the two main ways to programmatically interact with the Apify platform: by directly using [Apify's RESTful API](https://docs.apify.com/api/v2.md), or by using the [JavaScript](https://docs.apify.com/api/client/js) and [Python](https://docs.apify.com/api/client/python) API clients. In the next two lessons, we'll be focusing on the first two.

> Apify's API and JavaScript API client allow us to do anything a regular user can do when interacting with the platform's web interface, only programmatically.

## Learning 🧠

* Scroll through the [Apify API docs](https://docs.apify.com/api/v2.md) (there's a whole lot there, so you're not expected to memorize everything).
* Read about the Apify client in [Apify's docs](https://docs.apify.com/api/client/js). It can also be seen on [GitHub](https://github.com/apify/apify-client-js) and [npm](https://www.npmjs.com/package/apify-client).
* Learn about the [Actor.newClient()](https://docs.apify.com/sdk/js/reference/class/Actor#newClient) function in the Apify SDK.
* Skim through [this article](https://help.apify.com/en/articles/2868670-how-to-pass-data-from-web-scraper-to-another-actor) about API integration (this article is old; however, still relevant).

## Knowledge check 📝

1. What is the relationship between the Apify API and the Apify client? Are there any significant differences?
2. How do you pass input when running an Actor or task via API?
3. Do you need to install the `apify-client` npm package when already using the `apify` package?

## Our task

We'll be creating another new Actor, which will have two jobs:

1. Programmatically call the task for the Amazon Actor.
2. Export its results into CSV format under a new key called **OUTPUT.csv** in the default key-value store.

Though it's a bit unintuitive, this is a perfect activity for learning how to use both the Apify API and the Apify JavaScript client.

The new Actor should take the following input values, which be mapped to parameters in the API calls:


```json
{

    // How much memory to allocate to the Amazon Actor

    // Must be a power of 2

    "memory": 4096,



    // Whether to use the JavaScript client to make the

    // call, or to use the API

    "useClient": false,



    // The fields in each item to return back. All other

    // fields should be omitted

    "fields": ["title", "itemUrl", "offer"],



    // The maximum number of items to return back

    "maxItems": 10

}
```


[Solution](https://docs.apify.com/academy/expert-scraping-with-apify/solutions/using-api-and-client.md)

## Next up

[Lesson VI](https://docs.apify.com/academy/expert-scraping-with-apify/migrations-maintaining-state.md) will teach us everything we need to know about migrations and how to handle them properly to avoid losing any state; therefore, increasing the reliability of our `demo-actor` Amazon scraper.
