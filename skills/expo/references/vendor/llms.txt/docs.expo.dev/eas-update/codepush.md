---
modificationDate: July 28, 2026
title: Migrate from CodePush
description: A guide to help migrate from CodePush to EAS Update.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas-update/codepush/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas-update/codepush/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Update > Reference
Pages in this section:
- [Code signing](https://docs.expo.dev/eas-update/code-signing.md)
- [Asset selection and exclusion](https://docs.expo.dev/eas-update/asset-selection.md)
- [Using without other EAS services](https://docs.expo.dev/eas-update/standalone-service.md)
- [Request proxying](https://docs.expo.dev/eas-update/request-proxying.md)
- [Migrate from CodePush](https://docs.expo.dev/eas-update/codepush.md) (this page)
- [Migrate from Classic Updates](https://docs.expo.dev/eas-update/migrate-from-classic-updates.md)
- [Trace update ID back to the EAS dashboard](https://docs.expo.dev/eas-update/trace-update-id-expo-dashboard.md)
- [Estimate bandwidth usage](https://docs.expo.dev/eas-update/estimate-bandwidth.md)
- [Integrate in existing native apps](https://docs.expo.dev/eas-update/integration-in-existing-native-apps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Migrate from CodePush

A guide to help migrate from CodePush to EAS Update.

This guide explains how to transition a React Native project that uses CodePush to use EAS Update which offers [many advantages](/eas-update/introduction.md#key-features). It assumes that you're using the default React Native project structure. For assistance with migrating brownfield native apps to EAS Update, see [Using EAS Update in an existing native app](/eas-update/integration-in-existing-native-apps.md).

To learn more about the differences between CodePush and EAS Update, see [Conceptual differences between CodePush and EAS Update](/eas-update/codepush.md#conceptual-differences-between-codepush-and-eas-update) and the [What to do without CodePush post on the Expo Blog](https://expo.dev/blog/what-to-do-without-codepush).

## Ensure your app is using the latest Expo SDK version

To migrate from CodePush to EAS Update, we recommend that you use the latest Expo SDK version. Instructions are not available for older Expo SDK and React Native version. While you may be able to migrate successfully by adapting the instructions as needed for the older Expo SDK and React Native version that your app uses, additional hands-on support for integrating with older versions can only be provided for enterprise customers ([contact us](https://expo.dev/contact)).

## Uninstall CodePush

To avoid conflicts and unexpected behavior, it's recommended to uninstall CodePush if you're using EAS Update. This is because your app could periodically fetch updates from both services, leading to issues, especially if you're using different configurations for each service.

Remove the CodePush SDK from your project by uninstalling the `react-native-code-push` package:

```sh
npm uninstall react-native-code-push
```

You'll also need to remove CodePush references from JS and native code. See this [GitHub comment](https://github.com/Microsoft/react-native-code-push/issues/1101#issuecomment-350204507) for more detailed instructions.

## Add an `expo` key to your `app.json`

Ensure that your project has an **app.json** file with an `expo` object. If you don't have anything specific to configure in your **app.json** file yet, you can create a minimal file with an empty `expo` object like this:

```json
{
  "expo": {
    //... any other existing keys you have
  }
}
```

## Follow the "Getting Started" guide

The instructions in the [EAS Update Getting Started guide](/eas-update/getting-started.md) will guide you through setting up EAS Update in your project.

## Resubmit your app

Since you have changed the update provider from CodePush to EAS Update, you will need to rebuild your app and submit the new build to the respective app stores (Google Play Store and Apple App Store) to ensure the update mechanism works as expected for your end-users.

Follow the respective store guidelines for submitting a new build of your application:

-   [Submitting to Google Play Store](/submit/android.md)
-   [Submitting to Apple App Store](/submit/ios.md)

After successfully submitting your app, users will be able to download and use the latest build with EAS Update integration. If your app is not updating as expected, [validate your configuration](/eas-update/debug.md).

## Common questions

#### How do I release mandatory/critical updates with EAS Update?

CodePush CLI has a `--mandatory` flag that allows you to release mandatory updates. You can build this functionality with EAS Update but there is no specific flag for it.

[Learn more about mandatory/critical updates](/eas-update/download-updates.md#criticalmandatory-updates).

#### How do I include a message in an update?

CodePush CLI has a `--description` flag that allows you to include a message in an update. You can build this functionality with EAS Update using the `extra` field in your app config.

Refer the `--message` flag in this example: [`expo/UpdatesAPIDemo`](https://github.com/expo/UpdatesAPIDemo).

#### How do I switch the 'deployment' that is being used at runtime, similar to the sync() function in CodePush?

This is possible using `Updates.setUpdateURLAndRequestHeadersOverride()`. Learn more in the [Override update configuration at runtime](/eas-update/override.md) guide.

#### How do I handle different environments (such as staging and production) with EAS Update?

With EAS Update, you can use channels and branches to manage different environments and rollouts. See [how to manage branches and channels with EAS CLI](/eas-update/eas-cli.md).

#### How do I roll back updates with EAS Update?

You can roll back updates using `eas update:rollback`. Learn more in the [Rollback to a previous update](/eas-update/rollbacks.md) guide.

#### How do I gradually roll out updates with EAS Update?

EAS Update supports various strategies for gradually rolling out updates, so you can pick which approach best fits your needs. See the [rollouts guide](/eas-update/rollouts.md).

#### How can I have direct control over when an update is downloaded and applied?

Learn more about different strategies for downloading and applying updates in the [Downloading updates](/eas-update/download-updates.md) guide, such as checking for updates while the app is running or even when backgrounded with `Updates.checkForUpdateAsync()`.

#### Does EAS Update support end-to-end code signing?

Yes, EAS Update supports end-to-end code signing. It is available for EAS Production and Enterprise plan subscribers. Learn more in the [Code signing](/eas-update/code-signing.md) guide.

#### What else should I know about?

-   Expo Orbit: The macOS, Windows, and Linux desktop launcher app. You can [launch updates](/review/with-orbit.md) directly from the website with it in one click, among other features.
-   You can monitor the adoption of updates from the EAS website. See [monitoring adoption of updates](/eas-update/download-updates.md#monitoring-adoption-of-updates). You can also roll out and roll back updates from the website.
-   You can use EAS Update to achieve a web-like preview workflow. See [how to preview updates](/eas-update/preview.md).
-   Each update and build created with EAS is associated with a [fingerprint](/versions/latest/sdk/fingerprint.md). You can diff these fingerprints through the website UI or with `eas fingerprint:compare` to see what changed in the native runtime of your app between your builds and updates, to understand build/update compatibility, and guide your decision about when to bump the [`runtimeVersion`](/eas-update/runtime-versions.md).

## Conceptual differences between CodePush and EAS Update

CodePush and EAS Update are both services that allow you to send hotfixes to the JavaScript code of your app, but they take slightly different approaches, and so you may need to adapt your release process when moving to EAS Update.

#### Differences in how updates are organized within streams

**CodePush has single streams of updates for deployments**. What this means is that you can point a build to a deployment, and it will pull updates from that. If you want to change the deployment that is targeted by a build, you can do this at runtime through a JavaScript API.

**EAS Update has multiple streams of updates** — one that corresponds to your source control branches (called branches), and another called channels, which point to branches. The mapping between channels and branches is handled on the server side, and a channel can point to different branches for each runtime version (additionally, more advanced logic may be expressed, such as to support incremental rollouts). Builds are not directly associated with branches, but rather with channels. Each build points to a single channel by default. The reason for this is that it ensures that certain branches (for example: development, staging) don't automatically go out to production, so your preview updates don't go to your production users. This helps you separate the two main uses of EAS Update: previews and production hotfixes.

#### Differences in how updates are selected at runtime

A key distinction between CodePush and EAS Update that can impact your release process is that **with CodePush, the client controls the target update deployment at runtime**, and **with EAS Update, the default path is controlled on the server side, by mapping channels to branches**. In the default EAS Update flow, a build requests updates for its configured channel, and EAS Update returns the latest compatible update from the branch mapped to that channel.

The ability to control the target deployment at runtime is commonly used with CodePush in staging environments to allow non-technical stakeholders to test features from a single build on Google Play Beta and TestFlight. With EAS Update, you can achieve the same with [channel surfing](/eas-update/channel-surfing.md). This allows you to switch update channels in non-development builds at runtime. Development builds can also load updates from any compatible channel and are a good fit for developer-focused testing.
