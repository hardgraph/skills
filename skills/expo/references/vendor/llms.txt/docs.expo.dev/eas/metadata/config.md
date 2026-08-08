---
modificationDate: April 07, 2026
title: Configuring EAS Metadata
description: Learn about different ways to configure EAS Metadata.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/metadata/config/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/metadata/config/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Metadata > Reference
Pages in this section:
- [store.config.json](https://docs.expo.dev/eas/metadata/config.md) (this page)
- [Metadata schema](https://docs.expo.dev/eas/metadata/schema.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Configuring EAS Metadata

Learn about different ways to configure EAS Metadata.

> **EAS Metadata** is in [beta](/more/release-statuses.md#beta) and subject to breaking changes.

EAS Metadata is configured by a **store.config.json** file at the _root of your project_.

You can configure the path or name of the store config file with the **eas.json** [`metadataPath`](/eas/json.md#metadatapath) property. Besides the default JSON format, EAS Metadata also supports more dynamic config using JavaScript files.

## Static store config

The default store config type for EAS Metadata is a simple JSON file. The code snippet below shows an example store config with basic App Store information written in English (U.S.).

You can find all configuration options in the [store config schema](/eas/metadata/schema.md).

> If you have the [VS Code Expo Tools extension](https://github.com/expo/vscode-expo#readme) installed, you get auto-complete, suggestions, and warnings for **store.config.json** files.

```json
{
  "configVersion": 0,
  "apple": {
    "info": {
      "en-US": {
        "title": "Awesome App",
        "subtitle": "Your self-made awesome app",
        "description": "The most awesome app you have ever seen",
        "keywords": ["awesome", "app"],
        "marketingUrl": "https://example.com/en/promo",
        "supportUrl": "https://example.com/en/support",
        "privacyPolicyUrl": "https://example.com/en/privacy"
      }
    }
  }
}
```

## Dynamic store config

At times, Metadata properties can benefit from dynamic values. For example, the Metadata **copyright notice** should contain the current year. This can be automated with EAS Metadata.

To generate content dynamically, start by creating a JavaScript config file **store.config.js**. Then, use the [`metadataPath`](/eas/json.md#metadatapath) property in the **eas.json** file to pick the JS config file.

> `eas metadata:pull` can't update dynamic store config files. Instead, it creates a JSON file with the same name as the configured file. You can import the JSON file to reuse the data from `eas metadata:pull`.

```js
// Use the data from `eas metadata:pull`
const config = require('./store.config.json');

const year = new Date().getFullYear();
config.apple.copyright = `${year} Acme, Inc.`;

module.exports = config;
```

```json
{
  "submit": {
    "production": {
      "ios": {
        "metadataPath": "./store.config.js"
      }
    }
  }
}
```

## Store config with external content

When using external services for localizations, you have to fetch external content. EAS Metadata supports synchronous and asynchronous functions exported from dynamic store config files. The function results are awaited before validating and syncing with the stores.

> The **store.config.js** function is evaluated in Node.js. If you need special values, like secrets, use environment variables.

```js
// Use the data from `eas metadata:pull`
const config = require('./store.config.json');

module.exports = async () => {
  const year = new Date().getFullYear();
  const info = await fetchLocalizations('...').then(response => response.json());

  config.apple.copyright = `${year} Acme, Inc.`;
  config.apple.info = info;

  return config;
};
```

```json
{
  "submit": {
    "production": {
      "ios": {
        "metadataPath": "./store.config.js"
      }
    }
  }
}
```
