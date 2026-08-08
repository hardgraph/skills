---
modificationDate: July 28, 2026
title: Migrate from Classic Updates
description: A guide to help migrate from Classic Updates to EAS Update.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas-update/migrate-from-classic-updates/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas-update/migrate-from-classic-updates/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Update > Reference
Pages in this section:
- [Code signing](https://docs.expo.dev/eas-update/code-signing.md)
- [Asset selection and exclusion](https://docs.expo.dev/eas-update/asset-selection.md)
- [Using without other EAS services](https://docs.expo.dev/eas-update/standalone-service.md)
- [Request proxying](https://docs.expo.dev/eas-update/request-proxying.md)
- [Migrate from CodePush](https://docs.expo.dev/eas-update/codepush.md)
- [Migrate from Classic Updates](https://docs.expo.dev/eas-update/migrate-from-classic-updates.md) (this page)
- [Trace update ID back to the EAS dashboard](https://docs.expo.dev/eas-update/trace-update-id-expo-dashboard.md)
- [Estimate bandwidth usage](https://docs.expo.dev/eas-update/estimate-bandwidth.md)
- [Integrate in existing native apps](https://docs.expo.dev/eas-update/integration-in-existing-native-apps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Migrate from Classic Updates

A guide to help migrate from Classic Updates to EAS Update.

> SDK 49 was the last version to support Classic Updates. To continue using the deprecated `expo publish` command, set `updates.useClassicUpdates` in your app config.

EAS Update is the next generation of Expo's updates service. If you're using Classic Updates, this guide will help you upgrade to EAS Update.

#### Prerequisites

##### Minimum versions for EAS Update

EAS Update requires the following versions or greater:

-   Expo SDK 45.0.0 and later
-   Expo CLI 5.3.0 and later
-   EAS CLI 0.50.0 and later
-   `expo-updates` 0.13.0 and later

## Install EAS CLI

Install EAS CLI:

```sh
# npm
npm install --global eas-cli

# yarn
yarn global add eas-cli

# pnpm
pnpm add --global eas-cli

# bun
bun add --global eas-cli
```

Then, log in with your expo account:

```sh
eas login
```

## Configure your project

You'll need to make the following changes to your project:

Initialize your project with EAS Update:

```sh
eas update:configure
```

After this command, you should have two new fields in your app config at `expo.updates.url` and `expo.runtimeVersion`.

To ensure that updates are compatible with the underlying native code inside a build, EAS Update uses a new field named `runtimeVersion` that replaces the `sdkVersion` field in your project's app config. Remove the `expo.sdkVersion` property from your app config.

To allow updates to apply to builds built with EAS, update your EAS Build profiles in **eas.json** to include `channel` properties. These channels replace `releaseChannel` properties. We find it convenient to name the `channel` after the profile's name. For instance, the `preview` profile has a `channel` named `"preview"` and the `production` profile has a `channel` named `"production"`.

```json
{
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal"
    },
    "preview": {
      "distribution": "internal",
      "channel": "preview"
    },
    "production": {
      "channel": "production"
    }
  }
}
```

**Optional**: If your project is an [existing React Native project](/bare/overview.md), see [Use EAS Update in an existing project](/eas-update/getting-started.md) for the extra configuration you may need.

## Create new builds

The changes above affect the native code layer inside builds, which means you'll need to make new builds to start sending updates. Once your builds are complete, you'll be ready to publish an update.

## Publish an update

After making a change to your project locally, you're ready to publish an update, run:

```sh
eas update --channel [channel-name] --message [message]
eas update --channel production --message "Fixes typo"
```

Once published, you can see the update in the [EAS dashboard](https://expo.dev/accounts/%5Baccount%5D/projects/%5Bproject%5D/updates).

## Additional migration steps

-   Replace instances of `expo publish` with `eas update` in scripts. You can view all the options for publishing with `eas update --help`.
-   If you have any code that references `Updates.releaseChannel` from the `expo-updates` library, replace them with `Updates.channel`.
-   Remove any code that references `Constants.manifest`. That will now always return `null`. You can access most properties you'll need with `Constants.expoConfig` from the `expo-constants` library.

## Learn more

The steps described above allow you to use a similar flow to Classic Updates. However, EAS Update is more flexible and has more features. It can be used to create more stable release flows. Learn [how EAS Update works](/eas-update/how-it-works.md) and how you can craft a more stable [deployment process](/eas-update/deployment-patterns.md) for your project and your team.

If you experience issues with migrating, check out our [debugging guide](/eas-update/debug.md). If you have feedback, join us on [Discord](https://chat.expo.dev/) in the #update channel.
