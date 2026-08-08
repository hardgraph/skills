---
modificationDate: April 27, 2026
title: App stores metadata
description: A brief overview of how to use EAS Metadata to automate and maintain your app store presence.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/deploy/app-stores-metadata/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/deploy/app-stores-metadata/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > Deploy
Pages in this section:
- [Build project for app stores](https://docs.expo.dev/deploy/build-project.md)
- [Submit to app stores](https://docs.expo.dev/deploy/submit-to-app-stores.md)
- [App stores metadata](https://docs.expo.dev/deploy/app-stores-metadata.md) (this page)
- [Send over-the-air updates](https://docs.expo.dev/deploy/send-over-the-air-updates.md)
- [Deploy web apps](https://docs.expo.dev/deploy/web.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# App stores metadata

A brief overview of how to use EAS Metadata to automate and maintain your app store presence.

> **EAS Metadata** is in [beta](/more/release-statuses.md#beta) and subject to breaking changes. The service currently **only supports the Apple App Store**.

When submitting your app to app stores, you need to provide metadata. This process is lengthy and is often about complex topics that don't apply to your app. After the information you provide gets reviewed and if there is any issue with it, you need to restart this process.

[**EAS Metadata**](/eas/metadata.md) enables you to automate and maintain this information from the command line instead of going through multiple forms in the app store dashboards. It can also instantly identify well-known app store restrictions that could trigger a rejection after a lengthy review queue. This guide shows how to use EAS Metadata to automate and maintain your app store presence.

> **Tip:** Using VS Code? Install the [Expo Tools extension](https://github.com/expo/vscode-expo#readme) for auto-complete, suggestions, and warnings in your **store.config.json** files.

## Create a store config

EAS Metadata uses [store.config.json](/eas/metadata/config.md) file to hold all the information you want to upload to the app stores. This file is located at the root of your Expo project.

Create a new **store.config.json** file at the root of your project directory as shown in the example below:

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

The above example file contains JSON schema. Replace the example values with your own. It is usually contains your app's `title`, `subtitle` , `description`, `keywords`, and `marketingUrl` and so on.

**An important thing to remember from the above example is the `configVersion` property.** It helps with versioning changes that are not backward compatible.

> For more information on properties that can be defined in **store.config.json**, see [Schema for EAS Metadata](/eas/metadata/schema.md#config-schema).

## Upload the store config

> Before pushing the **store.config.json** to the app stores, you must upload a new binary of your app. See [App Store submissions](/deploy/submit-to-app-stores.md) for more information. After the binary is submitted and processed, you can continue with the step below.

After you have created the **store.config.json** file and added the necessary information related to your app, you can push the store config to the app stores by running the command:

```sh
eas metadata:push
```

If EAS Metadata runs into any issues with your store config, it will warn you when running this command. When there are no errors, or you confirm to push it with possible issues, it will try to upload as much as possible.

You can also re-use this command when you modify the **store.config.json** file and want to push the latest changes to the app stores.

## Next steps

[EAS Metadata schema](/eas/metadata/schema.md) — A reference of store config in EAS Metadata.

[Static and dynamic configurations with EAS Metadata](/eas/metadata/config.md) — Learn about different ways to configure EAS Metadata.
