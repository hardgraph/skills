# apify<!-- -->

[![npm version](https://badge.fury.io/js/apify.svg)](https://www.npmjs.com/package/apify) [![Downloads](https://img.shields.io/npm/dm/apify.svg)](https://www.npmjs.com/package/apify) [![Chat on discord](https://img.shields.io/discord/801163717915574323?label=discord)](https://discord.gg/jyEM2PRvMU) [![Build Status](https://github.com/apify/apify-sdk-js/actions/workflows/test-and-release.yaml/badge.svg?branch=master)](https://github.com/apify/apify-sdk-js/actions/workflows/test-and-release.yaml)

`apify` is the official SDK for building [Apify Actors](https://docs.apify.com/platform/actors) in JavaScript and TypeScript. It handles the Actor lifecycle, storage access, platform events, proxy configuration, and more.

## Quick Start[](#quick-start)

This short tutorial will set you up to start using Apify SDK in a minute or two. If you want to learn more, proceed to the [Apify Platform](https://docs.apify.com/sdk/js/sdk/js/docs/concepts/actor-lifecycle.md) guide that will take you step by step through running your Actor on Apify's platform.

Apify SDK requires [Node.js](https://nodejs.org/en/) 16 or later. Add Apify SDK to any Node.js project by running:

```
npm install apify
```

To initialize your Actor and to stop it use the `Actor.init()` and `Actor.exit()` functions. You also may use `Actor.main()` function for cases with multiple crawlers in one context.

```
import { Actor } from 'apify';



await Actor.init();



const input = (await Actor.getInput()) ?? {};

await Actor.setValue('OUTPUT', {

    message: 'Hello from Apify SDK!',

    input,

});



await Actor.exit();
```

> You can also install the [`crawlee`](https://www.npmjs.com/package/crawlee) module, as it now provides the crawlers that were previously exported by Apify SDK. If you don't plan to use crawlers in your Actors, then you don't need to install it. Keep in mind that neither `playwright` nor `puppeteer` are bundled with `crawlee` in order to reduce install size and allow greater flexibility. That's why we manually install it with NPM. You can choose one, both, or neither. For more information and example please check [`documentation.`](https://docs.apify.com/sdk/js/sdk/js/docs/concepts/actor-lifecycle.md#running-crawlee-code-as-an-actor)

## What are Actors?[](#what-are-actors)

Actors are serverless programs that can do almost anything. From simple scripts and web scrapers to complex automation workflows, AI agents, or even always-on services that expose HTTP endpoints.

They can run either locally or on the Apify platform, where you can scale their execution, monitor runs, schedule tasks, integrate them with other services, or even publish and monetize them. If you're new to Apify, learn more about the platform in the [Apify documentation](https://docs.apify.com/platform/about).

For more context, read the [Actor whitepaper](https://whitepaper.actor/).

## What you can build[](#what-you-can-build)

Almost any Node.js project can become an Actor, including projects for:

* **Web scraping and crawling** - The SDK works seamlessly with [Crawlee](https://crawlee.dev), which makes Apify a natural place to deploy and scale your crawlers. Start from a ready-made [Cheerio](https://apify.com/templates/js-crawlee-cheerio) template.
* **Browser automation** - Drive a real browser with [Playwright](https://playwright.dev), [Puppeteer](https://pptr.dev), or [Selenium](https://apify.com/apify/example-selenium) to automate tasks, fill in forms, or test web apps.
* **AI agents** - Host agents built with your framework of choice. Ready-made Actor templates cover [LangChain](https://apify.com/templates/js-langchain), [LangGraph](https://apify.com/templates/js-langgraph-agent), [BeeAI](https://apify.com/templates/ts-beeai-agent), and [Mastra](https://apify.com/templates/ts-mastraai).
* **MCP servers** - Deploy an MCP server as an Actor and make its tools available to any MCP client. See the [MCP server](https://apify.com/templates/ts-mcp-empty) and [MCP proxy](https://apify.com/templates/ts-mcp-proxy) templates.
* **Web servers and APIs** - Run a web server inside an Actor to serve HTTP requests, for example to expose your scraper as a live API. See the [Standby](https://apify.com/templates/js-standby) templates.

Whatever you build, the Apify SDK doesn't lock you into a particular framework. Bring the libraries you already use, and let Apify run your project in the cloud.

## Support[](#support)

If you find any bug or issue with the Apify SDK, please [submit an issue on GitHub](https://github.com/apify/apify-sdk-js/issues). For questions, you can ask on [Stack Overflow](https://stackoverflow.com/questions/tagged/apify) or contact <support@apify.com>

## Upgrading[](#upgrading)

Visit the [Upgrading Guide](https://docs.apify.com/sdk/js/sdk/js/docs/upgrading/upgrading-to-v3.md) to find out what changes you might want to make, and, if you encounter any issues, join our [Discord server](https://discord.gg/jyEM2PRvMU) for help!

## Contributing[](#contributing)

Your code contributions are welcome, and you'll be praised to eternity! If you have any ideas for improvements, either submit an issue or create a pull request. For contribution guidelines and the code of conduct, see [CONTRIBUTING.md](https://github.com/apify/apify-sdk-js/blob/master/CONTRIBUTING.md).

## License[](#license)

This project is licensed under the Apache License 2.0 - see the [LICENSE.md](https://github.com/apify/apify-sdk-js/blob/master/LICENSE.md) file for details.

## Acknowledgments[](#acknowledgments)

Many thanks to [Chema Balsas](https://www.npmjs.com/~jbalsas) for giving up the `apify` package name on NPM and renaming his project to [jsdocify](https://www.npmjs.com/package/jsdocify).

## Index[**](#Index)

### Result Stores

* [**Dataset](https://docs.apify.com/sdk/js/sdk/js/reference/class/Dataset.md)

### Scaling

* [**ProxyConfiguration](https://docs.apify.com/sdk/js/sdk/js/reference/class/ProxyConfiguration.md)

### Sources

* [**RequestQueue](https://docs.apify.com/sdk/js/sdk/js/reference/class/RequestQueue.md)

### Other

* [**LogLevel](https://docs.apify.com/sdk/js/sdk/js/reference/enum/LogLevel.md)
* [**Actor](https://docs.apify.com/sdk/js/sdk/js/reference/class/Actor.md)
* [**ApifyClient](https://docs.apify.com/sdk/js/sdk/js/reference/class/ApifyClient.md)
* [**ChargingManager](https://docs.apify.com/sdk/js/sdk/js/reference/class/ChargingManager.md)
* [**Configuration](https://docs.apify.com/sdk/js/sdk/js/reference/class/Configuration.md)
* [**KeyValueStore](https://docs.apify.com/sdk/js/sdk/js/reference/class/KeyValueStore.md)
* [**Log](https://docs.apify.com/sdk/js/sdk/js/reference/class/Log.md)
* [**Logger](https://docs.apify.com/sdk/js/sdk/js/reference/class/Logger.md)
* [**LoggerJson](https://docs.apify.com/sdk/js/sdk/js/reference/class/LoggerJson.md)
* [**LoggerText](https://docs.apify.com/sdk/js/sdk/js/reference/class/LoggerText.md)
* [**PlatformEventManager](https://docs.apify.com/sdk/js/sdk/js/reference/class/PlatformEventManager.md)
* [**AbortOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/AbortOptions.md)
* [**ActorPricingInfo](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ActorPricingInfo.md)
* [**ActorRun](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ActorRun.md)
* [**ApifyClientOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ApifyClientOptions.md)
* [**ApifyEnv](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ApifyEnv.md)
* [**CallOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/CallOptions.md)
* [**CallTaskOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/CallTaskOptions.md)
* [**ChargeOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ChargeOptions.md)
* [**ChargeResult](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ChargeResult.md)
* [**ConfigurationOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ConfigurationOptions.md)
* [**DatasetConsumer](https://docs.apify.com/sdk/js/sdk/js/reference/interface/DatasetConsumer.md)
* [**DatasetContent](https://docs.apify.com/sdk/js/sdk/js/reference/interface/DatasetContent.md)
* [**DatasetDataOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/DatasetDataOptions.md)
* [**DatasetIteratorOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/DatasetIteratorOptions.md)
* [**DatasetMapper](https://docs.apify.com/sdk/js/sdk/js/reference/interface/DatasetMapper.md)
* [**DatasetOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/DatasetOptions.md)
* [**DatasetReducer](https://docs.apify.com/sdk/js/sdk/js/reference/interface/DatasetReducer.md)
* [**ExitOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ExitOptions.md)
* [**InitOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/InitOptions.md)
* [**KeyConsumer](https://docs.apify.com/sdk/js/sdk/js/reference/interface/KeyConsumer.md)
* [**KeyValueStoreIteratorOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/KeyValueStoreIteratorOptions.md)
* [**KeyValueStoreOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/KeyValueStoreOptions.md)
* [**LoggerOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/LoggerOptions.md)
* [**MainOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/MainOptions.md)
* [**MetamorphOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/MetamorphOptions.md)
* [**OpenStorageOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/OpenStorageOptions.md)
* [**ProxyConfigurationOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ProxyConfigurationOptions.md)
* [**ProxyInfo](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ProxyInfo.md)
* [**QueueOperationInfo](https://docs.apify.com/sdk/js/sdk/js/reference/interface/QueueOperationInfo.md)
* [**RebootOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/RebootOptions.md)
* [**RecordOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/RecordOptions.md)
* [**RequestQueueOperationOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/RequestQueueOperationOptions.md)
* [**RequestQueueOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/RequestQueueOptions.md)
* [**StartOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/StartOptions.md)
* [**StorageAlias](https://docs.apify.com/sdk/js/sdk/js/reference/interface/StorageAlias.md)
* [**StorageId](https://docs.apify.com/sdk/js/sdk/js/reference/interface/StorageId.md)
* [**StorageName](https://docs.apify.com/sdk/js/sdk/js/reference/interface/StorageName.md)
* [**Timeout](https://docs.apify.com/sdk/js/sdk/js/reference/interface/Timeout.md)
* [**Token](https://docs.apify.com/sdk/js/sdk/js/reference/interface/Token.md)
* [**WebhookOptions](https://docs.apify.com/sdk/js/sdk/js/reference/interface/WebhookOptions.md)
* [**StorageIdentifier](https://docs.apify.com/sdk/js/sdk/js/reference.md#StorageIdentifier)
* [**StorageIdentifierWithoutAlias](https://docs.apify.com/sdk/js/sdk/js/reference.md#StorageIdentifierWithoutAlias)
* [**UserFunc](https://docs.apify.com/sdk/js/sdk/js/reference.md#UserFunc)
* [**log](https://docs.apify.com/sdk/js/sdk/js/reference.md#log)

## Other<!-- -->[**](#__CATEGORY__)

### [**](#StorageIdentifier)[**](https://github.com/apify/apify-sdk-js/blob/1c7a00f44bba808a75a9793059bf83398937c5f2/src/storage.ts#L46)StorageIdentifier

**StorageIdentifier: string | [StorageAlias](https://docs.apify.com/sdk/js/sdk/js/reference/interface/StorageAlias.md) | [StorageId](https://docs.apify.com/sdk/js/sdk/js/reference/interface/StorageId.md) | [StorageName](https://docs.apify.com/sdk/js/sdk/js/reference/interface/StorageName.md)

Identifies a storage to open. Can be:

* A plain `string` for backward compatibility (treated as ID or name)
* `{ alias: string }` to resolve from the Actor's schema storages (`ACTOR_STORAGES_JSON`)
* `{ id: string }` to open by explicit platform ID
* `{ name: string }` to open by explicit name

### [**](#StorageIdentifierWithoutAlias)[**](https://github.com/apify/apify-sdk-js/blob/1c7a00f44bba808a75a9793059bf83398937c5f2/src/storage.ts#L56)StorageIdentifierWithoutAlias

**StorageIdentifierWithoutAlias: string | [StorageId](https://docs.apify.com/sdk/js/sdk/js/reference/interface/StorageId.md) | [StorageName](https://docs.apify.com/sdk/js/sdk/js/reference/interface/StorageName.md)

Identifies a storage to open, without alias support. Used for key-value stores and request queues, which do not support aliases. Can be:

* A plain `string` for backward compatibility (treated as ID or name)
* `{ id: string }` to open by explicit platform ID
* `{ name: string }` to open by explicit name

### [**](#UserFunc)[**](https://github.com/apify/apify-sdk-js/blob/1c7a00f44bba808a75a9793059bf83398937c5f2/src/actor.ts#L254)UserFunc

**UserFunc\<T>: () => Awaitable\<T>

#### Type parameters

* **T** = unknown

#### Type declaration

* * **(): Awaitable\<T>

  - #### Returns Awaitable\<T>

### [**](#log)externalconstlog

**log: [Log](https://docs.apify.com/sdk/js/sdk/js/reference/class/Log.md)
