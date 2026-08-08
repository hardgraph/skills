---
modificationDate: April 28, 2026
title: 'Build locally: Overview'
description: An overview of how to build your app locally using your own machine for Expo projects.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/local-app-overview/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/local-app-overview/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Build locally
Pages in this section:
- [Overview](https://docs.expo.dev/guides/local-app-overview.md) (this page)
- [Development](https://docs.expo.dev/guides/local-app-development.md)
- [Release](https://docs.expo.dev/guides/local-app-production.md)
- [Cache builds remotely](https://docs.expo.dev/guides/cache-builds-remotely.md)
- [Precompiled Expo Modules](https://docs.expo.dev/guides/prebuilt-expo-modules.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Build locally: Overview

An overview of how to build your app locally using your own machine for Expo projects.

You can leverage your local development environment to build your app locally by utilizing Android Studio and Xcode. This build process can be done for both debug and release builds. This page provides an overview on different ways to build your app locally using your own machine and references to other guides that might be necessary in this workflow.

## When to build your app locally

There are different scenarios when you want to build your app on your developer machine:

-   You want to iterate quickly on native code changes or test platform-specific changes in your debug build
-   You want to manually generate native code to test your debug build
-   Any scenario where you are required to create builds inside an environment where access to a network is restricted.
-   You want to locally manage your own credentials (such as upload key, and so on)
-   You want to test or integrate your own custom build cache provider
-   You want to opt out of prebuilt Expo Modules for Android and compile them from source locally once

> **Note**: Building your app locally complements EAS Build. You can keep using the build service for cloud automation and fall back to local builds for development.

#### Prerequisites

##### Android Studio

[Set up Android Studio](/get-started/set-up-your-environment.md?platform=android&device=physical&mode=development-build&buildEnv=local#set-up-an-android-device-with-a-development-build) to compile and run Android projects on your local machine.

##### Xcode

[Set up Xcode](/get-started/set-up-your-environment.md?platform=ios&device=physical&mode=development-build&buildEnv=local#set-up-an-ios-device-with-a-development-build) to compile and run iOS projects on your local machine.

## Creating your debug build locally

To quickly build and iterate on a debug build, you can use Expo CLI's `npx expo run:[android|ios]` commands. These commands compile your project, using your locally installed Android SDK or Xcode, into a debug build of your app.

[Create a debug build locally](/guides/local-app-development.md) — Learn how to create a debug build for your Expo app locally.

## Creating your release build locally

To create a release build (also known as production build) of your app, you generate signing credentials by utilizing tools provided by Android Studio and Xcode. Then, you can generate a release build and follow the process of manually submitting your app to Google Play Store or Apple App Store.

[Create a release build locally](/guides/local-app-production.md) — Generate signed Android App Bundles, archive iOS builds in Xcode, and submit them manually to app stores.

## Reuse previous builds from a provider

You can accelerate your local development by caching and reusing builds from a provider. You can use EAS as a build provider or create your own custom provider.

[Use build cache providers](/guides/cache-builds-remotely.md) — Enable EAS build caching or ship a custom provider to shorten local build times.

## Prebuilt Expo Modules for Android

Expo ships prebuilt Expo Modules for Android that reduce the work Gradle performs on each build. You can continue using the defaults or selectively opt out when you need to modify a module's source code.

[Prebuilt Expo Modules for Android](/guides/prebuilt-expo-modules.md) — Understand how prebuilt modules work and learn how to opt out globally or per package.
