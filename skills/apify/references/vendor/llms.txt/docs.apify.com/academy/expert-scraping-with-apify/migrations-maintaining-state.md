---
title: "Migrations & maintaining state"
url: https://docs.apify.com/academy/expert-scraping-with-apify/migrations-maintaining-state.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Expert scraping with Apify](https://docs.apify.com/academy/expert-scraping-with-apify.md)
previous: [IV - Apify API & client](https://docs.apify.com/academy/expert-scraping-with-apify/apify-api-and-client.md)
next: [VI - Bypassing anti-scraping methods](https://docs.apify.com/academy/expert-scraping-with-apify/bypassing-anti-scraping.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Migrations & maintaining state

**Learn about what Actor migrations are and how to handle them properly so that the state is not lost and runs can safely be resurrected.**

***

We already know that Actors are Docker containers that can be run on any server. This means that they can be allocated anywhere there is space available, making them very efficient. Unfortunately, there is one big caveat: Actors move - a lot. When an Actor moves, it is called a **migration**.

On migration, the process inside of an Actor is completely restarted and everything in its memory is lost, meaning that any values stored within variables or classes are lost.

When a migration happens, you want to do a so-called "state transition", which means saving any data you care about so the Actor can continue right where it left off before the migration.

## Learning 🧠

Read this [article](https://docs.apify.com/actors/development/builds-and-runs/state-persistence.md) on migrations and dealing with state transitions.

Before moving forward, read about Actor [events](https://docs.apify.com/sdk/js/docs/upgrading/upgrading-to-v3#events) and how to listen for them.

## Knowledge check 📝

1. Actors have an option in the **Settings** tab to **Restart on error**. Would you use this feature for regular Actors? When would you use this feature?
2. Migrations happen randomly, but by [aborting gracefully](https://docs.apify.com/actors/running/runs-and-builds.md#aborting-runs), you can simulate a similar situation. Try this out on the platform and observe what happens. What changes occur, and what remains the same for the restarted Actor's run?
3. Why don't you (usually) need to add any special migration handling code for a standard crawling/scraping Actor? Are there any features in the Crawlee/Apify SDK that handle this under the hood?
4. How can you intercept the migration event? How much time do you have after this event happens and before the Actor migrates?
5. When would you persist data to the default key-value store instead of to a named key-value store?

## Our task

Once again returning to our Amazon **demo-actor**, let's say that we need to store an object in memory (as a variable) containing all of the scraped ASINs as keys and the number of offers scraped from each ASIN as values. The object should follow this format:


```json
{

    "B079ZJ1BPR": 3,

    "B07D4R4258": 21

}
```


Every 10 seconds, we should log the most up-to-date version of this object to the console. Additionally, the object should be able to solve Actor migrations, which means that even if the Actor were to migrate, its data would not be lost upon resurrection.

[Solution](https://docs.apify.com/academy/expert-scraping-with-apify/solutions/handling-migrations.md)

## Next up

You might have already noticed that we've been using the **RESIDENTIAL** proxy group in the `proxyConfiguration` within our Amazon scraping Actor. But what does that mean? Learn why we've used this group, about proxies, and about avoiding anti-scraping measures in the [next lesson](https://docs.apify.com/academy/expert-scraping-with-apify/bypassing-anti-scraping.md).
