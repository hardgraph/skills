---
title: Actors
url: https://docs.apify.com/academy/getting-started/actors.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Getting started](https://docs.apify.com/academy/getting-started.md)
previous: [Getting started](https://docs.apify.com/academy/getting-started.md)
next: [Creating Actors](https://docs.apify.com/academy/getting-started/creating-actors.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Actors

**What is an Actor? How do we create them? Learn the basics of what Actors are, how they work, and try out an Actor yourself right on the Apify platform!**

***

After you've followed the **Getting started** lesson, you're almost ready to start creating some Actors! But before we get into that, let's discuss what an Actor is, and a bit about how they work.

## What's an Actor?

When you deploy your script to the Apify platform, it is then called an **Actor**, which is a [serverless microservice](https://www.datadoghq.com/knowledge-center/serverless-architecture/serverless-microservices/#:~:text=Serverless%20microservices%20are%20cloud-based,suited%20for%20microservice-based%20architectures.) that accepts an input and produces an output. Actors can run for a few seconds, hours or even infinitely. An Actor can perform anything from a basic action such as filling out a web form or sending an email, to complex operations such as crawling an entire website and removing duplicates from a large dataset.

Once an Actor has been pushed to the Apify platform, they can be shared to the world through the [Apify Store](https://apify.com/store), and even monetized after going public.

Beyond scraping

Though the majority of Actors that are currently on the Apify platform are scrapers, crawlers, or automation software, Actors are not limited to scraping. They can be any program running in a Docker container.

## Actors on the Apify platform

For a super quick and dirty understanding of what a published Actor looks like, and how it works, let's run an SEO audit of *apify.com* using the [SEO audit Actor](https://apify.com/misceres/seo-audit-tool).

On the front page of the Actor, click the green **Try for free** button. If you're logged into your Apify account which you created during the [Getting started](https://docs.apify.com/academy/getting-started.md) lesson, you'll be taken to Apify Console and greeted with a page that looks like this:

![Actor configuration](/assets/images/seo-actor-config-6cde16dcb2bc752723bf7c6ed8364075.png)

This is where we can provide input to the Actor. The defaults here are just fine, so we'll leave it as is and click the green **Start** button to run it. While the Actor is running, you'll see it log some information about itself.

![Actor logs](/assets/images/actor-logs-a100ea07b38cdbe0ff6bc9cf3d808472.jpg)

After the Actor has completed its run (you'll know this when you see **SEO audit for apify.com finished.** in the logs), the results of the run can be viewed by clicking the **Results** tab, then subsequently the **View in another tab** option under **Export**.

## The "Actors" tab

While still on the platform, click on the tab with the **\</>** icon which says **Actors**. This tab is your one-stop-shop for seeing which Actors you've used recently, and which ones you've developed yourself. You will be frequently using this tab when developing and testing on the Apify platform.

![The \&quot;Actors\&quot; tab on the Apify platform](/assets/images/actors-tab-6244fff86563e1f10b96f275583162a2.jpg)

Now that you know the basics of what Actors are and how to use them, it's time to develop **an Actor of your own**!

## Next up

Get ready, because in the [next lesson](https://docs.apify.com/academy/getting-started/creating-actors.md), you'll be writing your very own Actor!
