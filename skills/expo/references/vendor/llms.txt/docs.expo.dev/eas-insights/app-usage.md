---
modificationDate: June 22, 2026
title: App usage
description: View your app's usage across platforms, app store versions, and time in the App usage tab of EAS Insights.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas-insights/app-usage/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas-insights/app-usage/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Insights
Pages in this section:
- [Introduction](https://docs.expo.dev/eas-insights/introduction.md)
- [App usage](https://docs.expo.dev/eas-insights/app-usage.md) (this page)
- [EAS Workflows](https://docs.expo.dev/eas-insights/workflows.md)
- [Maestro](https://docs.expo.dev/eas-insights/maestro.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# App usage

View your app's usage across platforms, app store versions, and time in the App usage tab of EAS Insights.

> **App usage** is in [preview](/more/release-statuses.md#preview) and subject to breaking changes. While in preview, it is free to use.

The **App usage** tab offers a view into a project's performance, usage, and reach. It allows you to see the state of your app, providing information about usage across platforms, app store versions, and timeframes.

## Integration with EAS Update

If you're already using [EAS Update](/eas-update/introduction.md), you get certain high-level usage insights without any additional client-side changes. This works by aggregating the update-check requests your app already makes into a limited Insights view of usage over time and by platform.

## Use the `expo-insights` library

Developers can add the `expo-insights` library to their projects and gain more precise usage metrics (than those provided by just aggregating update requests) and additional breakdowns by app store version. Currently, the library is limited to sending client events only pertaining to cold starts of the app, but in the future, `expo-insights` will expand to offer new types of events and payloads, to support more advanced functionality.

### Installation

To use `expo-insights`, make sure your app is linked to your EAS project in [app config](/workflow/configuration.md) by running `eas init`, then install the library.

```sh
# npm
npm install --global eas-cli
eas init
npx expo install expo-insights

# yarn
yarn global add eas-cli
eas init
yarn expo install expo-insights

# pnpm
pnpm add --global eas-cli
eas init
pnpm expo install expo-insights

# bun
bun add --global eas-cli
eas init
bun expo install expo-insights
```

After installing the library, create a build either with [EAS](/build/setup.md) or [locally](/guides/local-app-development.md). The library automatically sends events to EAS Insights when the app launches.

### View insights

To view usage data of your app, open your project in the EAS dashboard, select **Insights** from the navigation menu, and then select the **App usage** tab.
