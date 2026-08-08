# Quick start

Copy for LLM

Learn how to create and run Actors using the Apify SDK for JavaScript.

***

## Step 1: Create Actors[](#step-1-create-actors)

To create and run Actors in Apify Console, refer to the [Console documentation](https://docs.apify.com/platform/actors/development/quick-start/web-ide).

To create an Actor on your computer, use the [Apify CLI](https://docs.apify.com/sdk/js/cli):

```
apify create my-first-actor
```

The CLI will prompt you to select a template. After you choose a template, the CLI creates a new folder called `my-first-actor`, downloads and extracts the selected template, and installs dependencies using npm.

## Step 2: Run Actors[](#step-2-run-actors)

To run the Actor, you can use the [`apify run` command](https://docs.apify.com/sdk/js/cli/docs/reference#apify-run):

```
cd my-first-actor

apify run
```

This command:

* Starts the Actor with the appropriate environment variables for local running
* Configures it to use local storages from the `storage` folder

The Actor input, for example, will be in `storage/key_value_stores/default/INPUT.json`.

## Step 3: Understand Actor structure[](#step-3-understand-actor-structure)

All JavaScript Actor templates follow the same structure.

The `.actor` directory contains the [Actor configuration](https://docs.apify.com/platform/actors/development/actor-config), such as the Actor's definition and input schema, and the Dockerfile necessary to run the Actor on the Apify platform.

The Actor's runtime dependencies are specified in the `package.json` file.

The Actor's source code is typically in the `src` folder (or root for simple Actors). The main entry point is usually `src/main.js` or `src/main.ts`:

```
import { Actor, log } from 'apify';



// Initialize the Actor

await Actor.init();



// Get input from the Actor

const input = await Actor.getInput();

log.info('Actor input:', input);



// Your Actor logic goes here

await Actor.pushData({ message: 'Hello, world!' });



// Gracefully exit the Actor

await Actor.exit();
```

### Key methods[](#key-methods)

* **`Actor.init()`** - Initializes the Actor runtime, sets up storage, and prepares the environment
* **`Actor.getInput()`** - Retrieves input data passed to the Actor
* **`Actor.pushData()`** - Stores data in the default dataset
* **`Actor.exit()`** - Gracefully shuts down the Actor and saves its state

## Next steps[](#next-steps)

### Guides[](#guides)

To see how you can integrate the Apify SDK with some of the most popular web scraping libraries, check out our guides for working with:

* [CheerioCrawler](https://docs.apify.com/sdk/js/sdk/js/docs/guides/cheerio-crawler.md)
* [PuppeteerCrawler](https://docs.apify.com/sdk/js/sdk/js/docs/guides/puppeteer-crawler.md)
* [PlaywrightCrawler](https://docs.apify.com/sdk/js/sdk/js/docs/guides/playwright-crawler.md)

### Concepts[](#concepts)

To learn more about the features of the Apify SDK and how to use them, check out the Concepts section in the sidebar, especially:

* [Actor lifecycle](https://docs.apify.com/sdk/js/sdk/js/docs/concepts/actor-lifecycle.md)
* [Request storage](https://docs.apify.com/sdk/js/sdk/js/docs/concepts/request-storage.md)
* [Result storage](https://docs.apify.com/sdk/js/sdk/js/docs/concepts/result-storage.md)
* [Proxy management](https://docs.apify.com/sdk/js/sdk/js/docs/concepts/proxy-management.md)
