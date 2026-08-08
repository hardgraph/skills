---
title: Actors
url: https://docs.apify.com/actors.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
next: [Running Actors](https://docs.apify.com/actors/running.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Actors

Actors are serverless cloud programs that take a structured JSON input, perform a task (web scraping, browser automation, data processing, and more), and optionally produce a structured output. Run them manually in [Apify Console](https://console.apify.com/actors), through the [API](https://docs.apify.com/api.md) or [CLI](https://docs.apify.com/cli), or on a [schedule](https://docs.apify.com/actors/running/schedules.md), and combine them into larger automations.

Additional context

For more context, read the [Actor whitepaper](https://whitepaper.actor/).

## Actor lifecycle

#### [Running Actors](https://docs.apify.com/actors/running.md)

[In this section, you learn how to run Apify Actors. You will learn about their configuration, versioning, data retention, usage, and pricing.](https://docs.apify.com/actors/running.md)

#### [Actor development](https://docs.apify.com/actors/development.md)

[Read about the technical part of building Apify Actors. Learn to define Actor inputs, build new versions, persist Actor state, and choose base Docker images.](https://docs.apify.com/actors/development.md)

#### [Publishing and monetization](https://docs.apify.com/actors/publishing.md)

[Learn about publishing, and monetizing your Actors on the Apify platform.](https://docs.apify.com/actors/publishing.md)

## Actor components

Actors consist of these elements:

* *Dockerfile* which specifies where the Actor's source code is, how to build it, and run it.
* *Documentation* in a form of a README.md file.
* *Input and output schemas* that describe what input the Actor requires, and what results it produces.
* Access to an out-of-the-box *storage system* for Actor data, results, and files.
* *Metadata* such as the Actor name, description, author, and version.

The documentation and input/output schemas help people understand what the Actor does, enter required inputs in the user interface or API, and integrate results into other workflows. Actors can call and interact with each other to build more complex systems from simple ones.

![Apify Actor diagram](/assets/images/apify-actor-drawing-9e5b2c6bbe7a85acac72e5c7a13290a4.png)

## Public and private Actors

Actors can be [public](https://docs.apify.com/actors/running/actors-in-store.md) or private. Private Actors are yours to use and keep; no one will see them if you don't want them to. Public Actors are available to everyone in [Apify Store](https://apify.com/store). You can make them free to use, or you can [charge for them](https://blog.apify.com/make-regular-passive-income-developing-web-automation-actors-b0392278d085/).
