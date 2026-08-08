---
title: Local Actor development
url: https://docs.apify.com/actors/development/quick-start/locally.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Development](https://docs.apify.com/actors/development.md)
  - [Quick start](https://docs.apify.com/actors/development/quick-start.md)
previous: [Quick start](https://docs.apify.com/actors/development/quick-start.md)
next: [Web IDE](https://docs.apify.com/actors/development/quick-start/web-ide.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Local Actor development

Use pre-built prompt to get started faster.

Show promptCopy prompt

## What you'll learn

This guide walks you through the full lifecycle of an Actor. You'll start by creating and running it locally with the Apify CLI, then learn to configure its input and data storage. Finally, you will deploy the Actor to the Apify platform, making it ready to run in the cloud.

### Prerequisites

* [Node.js](https://nodejs.org/en/) version 16 or higher with `npm` installed on your computer.
* The [Apify CLI](https://docs.apify.com/cli/docs/installation) installed.
* Optional: To deploy your Actor, [sign in](https://console.apify.com/sign-in).

### Step 1: Create your Actor

Use the Apify CLI to create a new Actor:


```bash
apify create
```


The CLI will ask you to:

1. Name your Actor (e.g., `your-actor-name`)

2. Choose a programming language (`JavaScript`, `TypeScript`, or `Python`)

3. Select a development template

   Explore Actor templates

   Browse the [full list of templates](https://apify.com/templates) to find the best fit for your Actor.

The CLI will:

* Create a `your-actor-name` directory with boilerplate code
* Install all project dependencies

Now, you can navigate to your new Actor directory:


```bash
cd `your-actor-name`
```


### Step 2: Run your Actor

Run your Actor with:


```bash
apify run
```


You'll see output similar to this in your terminal:


```bash
INFO  System info {"apifyVersion":"3.4.3","apifyClientVersion":"2.12.6","crawleeVersion":"3.13.10","osType":"Darwin","nodeVersion":"v22.17.0"}

Extracted heading { level: 'h1', text: 'Your full‑stack platform for web scraping' }

Extracted heading { level: 'h3', text: 'TikTok Scraper' }

Extracted heading { level: 'h3', text: 'Google Maps Scraper' }

Extracted heading { level: 'h3', text: 'Instagram Scraper' }
```


As you can see in the logs, the Actor extracts text from a web page. The main logic lives in `src/main.js`. Depending on your template, this file may be `src/main.ts` (TypeScript) or `src/main.py` (Python).

In the next step, we’ll explore the results in more detail.

### Step 3: Explore the Actor

Let's explore the Actor structure.

#### The `.actor` folder

The `.actor` folder contains the Actor configuration. The `actor.json` file defines the Actor's name, description, and other settings. Find more info in the [actor.json](https://docs.apify.com/actors/development/actor-definition/actor-json) definition.

#### Actor's `input`

Each Actor accepts an `input object` that tells it what to do. The object uses JSON format and lives in `storage/key_value_stores/default/INPUT.json`.

Edit the schema to change input

To change the `INPUT.json`, edit the `input_schema.json` in the `.actor` folder first.

This JSON Schema validates input automatically (no error handling needed), powers the Actor's user interface, generates API docs, and enables smart integration with tools like Zapier or Make by auto-linking input fields.

Find more info in the [Input schema](https://docs.apify.com/actors/development/actor-definition/input-schema.md) documentation.

#### Actor's `storage`

The Actor system provides two storage types for files and results: [key-value](https://docs.apify.com/storage/key-value-store.md) store and [dataset](https://docs.apify.com/storage/dataset.md).

##### Key-value store

The key-value store saves and reads files or data records. Key-value stores work well for screenshots, PDFs, or persisting Actor state as JSON files.

##### Dataset

The dataset stores a series of data objects from web scraping, crawling, or data processing jobs. You can export datasets to JSON, CSV, XML, RSS, Excel, or HTML formats.

#### Actor's `output`

You define the Actor output using the Output schema files:

* [Dataset Schema Specification](https://docs.apify.com/storage/dataset-schema.md)
* [Key-value Store Schema Specification](https://docs.apify.com/storage/key-value-store-schema.md)

The system uses this to generate an immutable JSON file that tells users where to find the Actor's results.

### Step 4: Deploy your Actor

Let's now deploy your Actor to the Apify platform, where you can run the Actor on a scheduled basis, or you can make the Actor public for other users.

1. Login first:


   ```bash
   apify login
   ```


   Your Apify token location

   After you successfully login, your Apify token is stored in `~/.apify/auth.json`, or `C:\Users\<name>\.apify` based on your system.

2. Push your Actor to the Apify platform:


   ```bash
   apify push
   ```


### Step 5: It's time to iterate!

Good job! 🎉 You're ready to develop your Actor. You can make changes to your Actor and implement your use case.

## Next steps

* Visit the [Apify Academy](https://docs.apify.com/academy.md) to access a comprehensive collection of tutorials, documentation, and learning resources.
* To understand Actors in detail, read the [Actor Whitepaper](https://whitepaper.actor/).
* Check [Continuous integration](https://docs.apify.com/actors/development/deployment/continuous-integration.md) documentation to automate your Actor development process.
* After you finish building your first Actor, you can [share it with other users and even monetize it](https://docs.apify.com/actors/publishing.md).
