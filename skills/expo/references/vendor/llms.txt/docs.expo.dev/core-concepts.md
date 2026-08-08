---
modificationDate: May 05, 2026
title: Core concepts
description: An overview of Expo tools, features and services.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/core-concepts/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/core-concepts/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > More
Pages in this section:
- [Core concepts](https://docs.expo.dev/core-concepts.md) (this page)
- [FAQ](https://docs.expo.dev/faq.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Core concepts

An overview of Expo tools, features and services.

Expo is an [open-source framework](https://github.com/expo/expo/) for apps that run natively on Android, iOS, and the web. Expo brings together the best of mobile and the web and enables many important features for building and scaling an app.

The `expo` npm package enables a suite of incredible features for React Native apps. The `expo` package can be installed in nearly **any React Native project**.

## Tools and features

[Expo SDK](/versions/latest.md) — Comprehensive suite of well-tested React Native modules that run on Android, iOS, and web.

[Develop an app with Expo](/workflow/overview.md) — An overview of the development process of building an Expo app to help build a mental model of the core development loop.

[Expo Modules API](/modules/overview.md) — Write highly performant native code with modern Swift and Kotlin API.

[Prebuild](/workflow/continuous-native-generation.md) — Separate React from Native to develop from any computer, upgrade easily, white label apps, and maintain larger projects.

[Expo CLI](/more/expo-cli.md) — Manage dependencies, compile native apps, develop for the web, and connect to any device with a powerful dev server.

[Expo Go](/get-started/set-up-your-environment.md) — A playground for students and learners to try React Native on a simulator or device.

> All features are free, optional, and can be used independently of each other. Unused features add no additional bloat to your app.

| Feature | With `expo` | Without `expo` (existing React Native) |
| --- | --- | --- |
| Develop complex apps **entirely** in JavaScript. | ✓ | ✗ |
| Write JSI native modules with Swift and Kotlin. | ✓ | ✗ |
| Develop apps without Xcode or Android Studio. | ✓ | ✗ |
| Create and share example apps in the browser with [Snack](https://snack.expo.dev/). | ✓ | ✗ |
| Major upgrades without native changes. | ✓ | ✗ |
| First-class TypeScript support. | ✓ | ✗ |
| Install natively compatible libraries from the command line. | ✓ | ✗ |
| Develop performant websites with the same codebase. | ✓ | ✗ |
| [Tunnel](/more/expo-cli.md#tunneling) your dev server to any device. | ✓ | ✗ |

## Services

The team behind Expo also provides **Expo Application Services (EAS)**, deeply integrated cloud services for building, submitting, and updating your React Native app. EAS can be used with **any React Native app**, regardless of whether it uses `expo` or not.

[Expo Application Services](/eas.md) — The easiest way to build, deploy, and update native apps.
