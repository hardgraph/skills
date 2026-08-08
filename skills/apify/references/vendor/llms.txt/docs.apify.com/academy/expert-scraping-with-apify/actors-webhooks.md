---
title: "Webhooks & advanced Actor overview"
url: https://docs.apify.com/academy/expert-scraping-with-apify/actors-webhooks.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Expert scraping with Apify](https://docs.apify.com/academy/expert-scraping-with-apify.md)
previous: [Expert scraping with Apify](https://docs.apify.com/academy/expert-scraping-with-apify.md)
next: [II - Managing source code](https://docs.apify.com/academy/expert-scraping-with-apify/managing-source-code.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Webhooks & advanced Actor overview

**Learn more advanced details about Actors, how they work, and the default configurations they can take. Also, learn how to integrate your Actor with webhooks.**

Updates coming

This lesson is subject to change because it currently relies on code from our archived **Web scraping basics for JavaScript devs** course. For now you can still access the archived course, but we plan to completely retire it in a few months. This lesson will be updated to remove the dependency.

***

Thus far, you've run Actors on the platform and written an Actor of your own, which you published to the platform yourself using the Apify CLI; therefore, it's fair to say that you are becoming more familiar and comfortable with the concept of **Actors**. Within this lesson, we'll take a more in-depth look at Actors and what they can do.

## Advanced Actor overview

In this course, we'll be working out of the Amazon scraper built in the [Web scraping basics for JavaScript devs](https://docs.apify.com/academy/scraping-basics-javascript/legacy.md) course. If you haven't gone through it yet, we recommend doing so, it covers the fundamentals this project is built on. If you'd rather skip straight to this course, a working implementation is available at [academy-amazon-scraper](https://github.com/apify-projects/academy-amazon-scraper).

Take another look at the files within your Amazon scraper project. You'll notice that there is a **Dockerfile**. Every single Actor has a Dockerfile (the Actor's **Image**) which tells Docker how to spin up a container on the Apify platform which can successfully run the Actor's code. "Apify Actors" is a serverless platform that runs multiple Docker containers. For a deeper understanding of Actor Dockerfiles, refer to the [Apify Actor Dockerfile docs](https://docs.apify.com/sdk/js/docs/guides/docker-images#example-dockerfile).

## Webhooks

Webhooks are a powerful tool that can be used for just about anything. You can set up actions to be taken when an Actor reaches a certain state (started, failed, succeeded, etc). These actions usually take the form of an API call (generally a POST request).

## Learning 🧠

Prior to moving forward, please read over these resources:

* Read about [running Actors, handling Actor inputs, memory and CPU](https://docs.apify.com/actors/running.md).
* Learn about [Actor webhooks](https://docs.apify.com/integrations/webhooks.md), which we will implement in the next lesson.
* Learn [how to run Actors](https://docs.apify.com/academy/api/run-actor-and-retrieve-data-via-api.md) using Apify's REST API.

## Knowledge check 📝

1. How do you allocate more CPU for an Actor's run?
2. Within itself, can you get the exact time that an Actor was started?
3. What are the types of default storages connected to an Actor's run?
4. Can you change the allocated memory of an Actor while it's running?
5. How can you run an Actor with Puppeteer on the Apify platform with headless mode set to `false`?

## Our task

In this task, we'll be building on top of the [academy-amazon-scraper](https://github.com/apify-projects/academy-amazon-scraper).

Once our Amazon Actor has completed its run, we might want to send an email to ourselves, but let's call another Actor through a webhook instead. The Actor called will be a new Actor that you will create. It will take the dataset ID as input, then filter through all of the results and return only the cheapest result for each unique ASIN. All of the results of the Actor will be pushed to its default dataset.

Starter repo

The [starter repo](https://github.com/apify-projects/academy-amazon-scraper) produces one result per product, so in practice the filtering Actor will pass every item through unchanged. That's fine, the goal here is to learn how to pass data between Actors using webhooks, not to do complex filtering.

[Solution](https://docs.apify.com/academy/expert-scraping-with-apify/solutions/integrating-webhooks.md)

## Next up

This course's [next lesson](https://docs.apify.com/academy/expert-scraping-with-apify/managing-source-code.md) is brief, but discusses a very important topic: managing your code and storing it in a safe place.
