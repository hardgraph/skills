---
title: Run Actors
url: https://docs.apify.com/get-started/run-actors.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Get started](https://docs.apify.com/get-started.md)
previous: [Overview](https://docs.apify.com/get-started.md)
next: [Build Actors](https://docs.apify.com/get-started/build-actors.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Run Actors

You don't need to write any code to get data or automation out of Apify. [Apify Store](https://apify.com/store) offers thousands of ready-made Actors, and running one always follows the same loop: pick an Actor, give it input, start the run, and collect the results. This page walks you through that loop and points you to the right tutorial or concept page at each step.

## Pick an Actor from Store

Apify Store is a marketplace of Actors built by Apify and the community, covering most popular websites and automation tasks. Each Actor has its own page that describes what it does, what input it expects, and how it charges. Some Actors charge for the events they perform, such as producing a result (pay per event), while others bill only for the platform resources their runs consume (pay per usage).

* Browse [Apify Store](https://apify.com/store) and pick an Actor that matches your use case.
* Read [Actors in Store](https://docs.apify.com/actors/running/actors-in-store.md) to understand the pricing models before you start a large run.

## Give it input and run it

Every Actor takes structured input. In Apify Console, the input is a form where you enter your URLs, search terms, or other settings. Under the hood, that form is a JSON object - the same object you send when you later run the Actor through the API. Start with a tutorial, then read the concept pages as questions come up:

* [Run your first Actor](https://docs.apify.com/actors/running.md) - choose an Actor, fill in the input, start the run, and export the results.
* [Getting started course](https://docs.apify.com/academy/getting-started.md) - a guided Apify Academy walkthrough, from creating an account to your first run.
* [Input and output](https://docs.apify.com/actors/running/input-and-output.md) - how run configuration works in detail.

## Collect the results

A run doesn't just print results to a log - it writes them to storage on the platform, where they stay available after the run finishes. Structured results, like scraped records, go to a dataset that you can view in Console or export as JSON, CSV, or Excel. Files and other records go to a key-value store.

* [Storage](https://docs.apify.com/storage.md) - where results live: [datasets](https://docs.apify.com/storage/dataset.md) for structured data, the [key-value store](https://docs.apify.com/storage/key-value-store.md) for files and records.
* [Runs and builds](https://docs.apify.com/actors/running/runs-and-builds.md) - run states, and how to resurrect a finished run.
* [Usage and resources](https://docs.apify.com/actors/running/usage-and-resources.md) - what a run consumes and how that translates to cost.

## Automate the loop

Once a manual run produces the data you want, you rarely keep starting it by hand. The platform can store your input, run the Actor on a schedule, and deliver results straight to your other tools.

* [Save your input as a task](https://docs.apify.com/actors/running/tasks.md) so you can re-run it without re-typing the configuration.
* [Schedule an Actor](https://docs.apify.com/actors/running/schedules.md) to run automatically.
* [Send results to your tools](https://docs.apify.com/integrations.md) - Make, Zapier, n8n, Slack, webhooks, and more.
* [Use an Actor as a live API](https://docs.apify.com/actors/running/standby.md) with Actor Standby.
* [Monitor your runs](https://docs.apify.com/actors/running/monitoring.md) to catch failures and track performance.
* Run Actors programmatically via the [API](https://docs.apify.com/api/v2.md) or the [CLI](https://docs.apify.com/cli/docs).

Ready to build your own Actor instead of running someone else's? Switch to [Build Actors](https://docs.apify.com/get-started/build-actors.md).
