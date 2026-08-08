---
title: Publish your Actor task
url: https://docs.apify.com/actors/publishing/publish-task.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Publishing and monetization](https://docs.apify.com/actors/publishing.md)
previous: [Create an Actor README](https://docs.apify.com/actors/publishing/actor-readme.md)
next: [Monetize your Actor](https://docs.apify.com/actors/publishing/monetize.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Publish your Actor task

[Actor tasks](https://docs.apify.com/actors/running/tasks.md) are shareable, pre-configured inputs for your Actors. Publishing a task creates a public landing page that shows what the Actor does, what inputs it uses, and what output to expect. Public tasks also appear in your Actor's **Examples** tab, which helps users discover your Actor through search engines and AI agents. For monetized Actors, this can lead to more page views, more runs, and more revenue.

You can create up to 50 tasks per Actor.

## Prerequisites

Before you publish a task, make sure you have:

* A [published Actor](https://docs.apify.com/actors/publishing/publish.md) that you own or maintain.
* A [saved task](https://docs.apify.com/actors/running/tasks.md) with a complete input configuration.
* An [input schema](https://docs.apify.com/actors/development/actor-definition/input-schema.md) and at least one [dataset schema view](https://docs.apify.com/storage/dataset-schema.md) defined on the Actor.

## What tasks to publish

Focus on tasks that represent a real use case someone would search for and that show your Actor solving a specific problem. Not every saved task needs a public landing page.

### Focus on a real use case

The best tasks aim to solve a problem a user actually has. Instead of publishing a generic "default configuration" task, publish one that answers a question someone might type into a search engine or AI agent.

For example, if you maintain Google Maps, Amazon, and Yahoo scraper Actors:

| ❌ Weak task                 | ✅ Strong task                                                |
| ---------------------------- | ------------------------------------------------------------- |
| Google Maps - default config | Find dentists in San Francisco with reviews                   |
| Test run 2                   | Extract restaurant emails for local marketing                 |
| All fields enabled           | Compare gym ratings across New York boroughs                  |
| Amazon default               | Monitor Amazon product prices and reviews for market research |
| Yahoo Finance test           | Track stock prices and news on Yahoo Finance                  |

Each strong task focuses on a specific industry, location, or workflow. A user searching for "dentist data San Francisco" is far more likely to land on a page with that exact framing.

### Target search traffic and AI discovery

Each landing page is indexed by search engines and includes a markdown (`.md`) version that AI agents can access directly, making your task discoverable through AI-powered search and AI assistants.

Think of each task as a keyword-targeted landing page:

* Location-specific tasks: "Scrape real estate listings in Austin" rather than "Scrape real estate listings"
* Industry-specific tasks: "Monitor competitor pricing for electronics" rather than "Monitor competitor pricing"
* Workflow-specific tasks: "Export LinkedIn company data to Google Sheets" rather than "Export LinkedIn data"

The more specific the task, the less competition it has in search results and the more relevant it is to the person who finds it.

Show the range of your Actor

If your Actor supports multiple use cases, publish a task for each one. A web scraping Actor might have tasks for lead generation, price monitoring, and content aggregation. Each task demonstrates a different capability and attracts a different audience.

## Publish your tasks

Once your task runs reliably and produces the output you want users to see, publish it from Apify Console.

1. From your task's page in Apify Console, open the **Publication** tab.
2. Complete the three sections: Display information, Input, and Dataset schema.
3. Click **Publish task**.

![Publication tab on a task page.](/assets/images/task-publication-tab-cc088fab2e51db4776406e00fa678d78.webp)

### Display information

This section controls how your task appears on the public task landing page and in search results.

There are three fields in this section:

* **Slug:** the URL-friendly name used in the task's landing page URL.
* **SEO task title:** shown as the heading on the landing page and as the page title in search results.
* **SEO description:** appears under the title on the landing page and as the meta description in search results.

You can live preview how the fields propagate on the right (they update as you type):

* **Page Preview:** how the landing page will look to visitors.
* **SERP Preview:** how the page will appear in search engine results pages (SERPs).

![Display information section with page and SERP previews.](/assets/images/task-display-information-2cc89087eb003960d0888d97c576b3e0.webp)

Title format

Frame the title as the user's goal, not as a description of the Actor's mechanics. A good title combines an action, a target, and a qualifier.

Good examples:

* Track competitors' store locations
* Get a local B2B leads list
* Verify business emails from Google Maps

Avoid titles like "Google Maps scraper - task 3" or "Test task" that describe the Actor instead of the user's goal.

### Input

The Input section controls which fields from your task's input configuration appear on the public landing page. By default, every field defined in your task input is selected. Deselect any field that is not relevant to the task's use case.

This control affects display only. The task itself always runs with the full input configuration, regardless of which fields are selected here.

![Input section with field selection.](/assets/images/task-input-section-0389da9b9cf80716d25646ea072c0bf0.webp)

Use this section to:

* Show only the fields that demonstrate how the task is configured for its specific use case.
* Hide configuration fields that are not relevant to the task and would confuse first-time users.

Secret fields are protected automatically

All input fields with `"isSecret": true` in the Actor's [input schema](https://docs.apify.com/actors/development/actor-definition/input-schema.md) are automatically masked and never shown on the landing page. Before publishing, confirm sensitive fields are marked as secret.

### Dataset schema

The Dataset schema section selects which view of the Actor's dataset is rendered on the landing page. Each view organizes the output fields differently. Pick the one that best matches the task's use case.

![Dataset schema view selection.](/assets/images/task-dataset-schema-80cf352d509d756a8be03a6e917253d1.webp)

For more on configuring views, see [Dataset schema](https://docs.apify.com/storage/dataset-schema.md).

## Task landing page

Each published task gets its own standalone landing page on the [Apify website](https://apify.com). This page is publicly accessible, indexed by search engines, and readable by AI agents. Published tasks also appear in the **Examples** tab of the Actor's detail page, linking visitors directly to the landing page.

![Examples tab on the Actor detail page showing published tasks.](/assets/images/actor-examples-tab-6b4743ab62ccd4388a2dd9b76991eeb6.webp)

### URL structure

The landing page URL is built from your username, Actor name, and task name. For example:


```text
https://apify.com/john/google-maps-scraper/examples/find-dentists-san-francisco
```


Keep the task name short, descriptive, and focused on the use case keyword.

### Landing page content

![Task landing page with title, input fields, and output preview.](/assets/images/task-landing-page-fdf0d8b1cb42764938fcf7ea5f263202.webp)

The landing page displays the information you configured in the Display information, Input, and Dataset schema sections:

* The **SEO task title** and **SEO description** at the top of the page.
* The selected **input fields** with their configured values, so visitors can see exactly how the task is set up.
* A preview of the **Dataset schema fields** based on the dataset view you selected.
* A call-to-action button that lets visitors try the task immediately.
* Generic sections explaining how Apify works and how to integrate it.

### What happens when a visitor tries the task

When a visitor clicks the call-to-action button, Apify creates a new task under their account with the same input configuration. The visitor gets their own independent copy. They can modify, run, and manage it without affecting your original task. You don't need to worry about other users consuming your resources or altering your configuration.

## Next steps

* Set up [monetization](https://docs.apify.com/actors/publishing/monetize.md) for your Actor.
* Track quality with the [Actor quality score](https://docs.apify.com/actors/publishing/quality-score.md).
