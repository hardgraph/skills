---
title: Get started
url: https://docs.apify.com/get-started.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
next: [Run Actors](https://docs.apify.com/get-started/run-actors.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Get started

Apify is a cloud platform for web scraping, data extraction, and automation. Everything on it revolves around *Actors* - serverless programs that run in the cloud. This page explains how the platform fits together and where to start, whether you want to use existing tools or build your own.

## How Apify works

An Actor takes a structured JSON input, performs a job - scraping a website, automating a browser, processing data - and stores its results on the platform. Results typically land in a structured dataset you can export or send elsewhere. You can start an Actor manually in Apify Console, call it through the [API](https://docs.apify.com/api/v2.md), or put it on a [schedule](https://docs.apify.com/actors/running/schedules.md).

Because every Actor shares this input > run > output model, they compose. One Actor's output can feed another, results flow into [integrations](https://docs.apify.com/integrations.md) like Make, Zapier, or n8n, and you can call any Actor from your code like a regular API.

## How Actors get to Apify Store

[Apify Store](https://apify.com/store) holds thousands of public Actors built by Apify and the community. Each one started as code that a developer wrote, deployed to the platform, and published - optionally setting a price, since Store Actors can [earn their developers money](https://docs.apify.com/actors/publishing.md).

You can stand on either side of that exchange. Run other people's Actors to get data without writing code, or build your own - for yourself, your team, or as a product in Store.

## Run your first Actor

The quickest way to understand the platform is to run something. Open [Apify Store](https://apify.com/store), pick an Actor, click **Start**, and watch the results arrive. No code required.

## Choose your path

Pick the path that matches what you want to do. Each one walks you through the loop and points you to the right tutorials and concept pages in order:

* [Run Actors](https://docs.apify.com/get-started/run-actors.md) - use existing Actors from Apify Store to get data or automate a task, no coding required.
* [Build Actors](https://docs.apify.com/get-started/build-actors.md) - build, deploy, and publish your own Actor on the Apify platform.

## Common next steps

* [Schedule an Actor to run automatically](https://docs.apify.com/actors/running/schedules.md)
* [Trigger Actors from Zapier, Make, n8n, or your own API](https://docs.apify.com/integrations.md)
* [Connect an external AI agent to Apify](https://docs.apify.com/get-started/agent-onboarding.md)
* [Set up an organization for your team](https://docs.apify.com/account/collaboration/organization.md)
* [Publish and monetize your own Actor](https://docs.apify.com/actors/publishing.md)
