---
modificationDate: July 17, 2026
title: 'EAS Tutorial: Introduction'
description: An introduction to the tutorial for building apps for Android and iOS using Expo Application Services (EAS) that covers the Build, Update, and Submit workflows.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/eas/introduction/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/eas/introduction/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > EAS tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/eas/introduction.md) (this page)
- [Configure development build](https://docs.expo.dev/tutorial/eas/configure-development-build.md)
- [Android development build](https://docs.expo.dev/tutorial/eas/android-development-build.md)
- [iOS development build for simulators](https://docs.expo.dev/tutorial/eas/ios-development-build-for-simulators.md)
- [iOS development build for devices](https://docs.expo.dev/tutorial/eas/ios-development-build-for-devices.md)
- [Multiple app variants](https://docs.expo.dev/tutorial/eas/multiple-app-variants.md)
- [Internal distribution build](https://docs.expo.dev/tutorial/eas/internal-distribution-builds.md)
- [Manage app versions](https://docs.expo.dev/tutorial/eas/manage-app-versions.md)
- [Android production build](https://docs.expo.dev/tutorial/eas/android-production-build.md)
- [iOS production build](https://docs.expo.dev/tutorial/eas/ios-production-build.md)
- [Share previews](https://docs.expo.dev/tutorial/eas/team-development.md)
- [Builds from GitHub](https://docs.expo.dev/tutorial/eas/using-github.md)
- [Next steps](https://docs.expo.dev/tutorial/eas/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# EAS Tutorial: Introduction

An introduction to the tutorial for building apps for Android and iOS using Expo Application Services (EAS) that covers the Build, Update, and Submit workflows.

## About this tutorial

This tutorial will give you proficiency with [Expo Application Services (EAS)](https://expo.dev/eas) core services: [Build](/build/introduction.md), [Submit](/deploy/submit-to-app-stores.md), and [Update](/eas-update/introduction.md). When you complete the tutorial, you will know how to set up a professional mobile Continuous Integration (CI)/Continuous Development (CD) pipeline for your individual and team projects.

This tutorial covers the following topics:

-   Use EAS Build to create and install a development build, then run it on a device, emulator, or simulator.
-   Experience the benefits of using a development build instead of Expo Go.
-   Implement workflows for sharing development builds with a team or external stakeholders.
-   Automatically increment app build versions.
-   Simultaneously install different app variants, like development and preview, on one device.
-   Utilize EAS Update to create and deploy updates swiftly during the development phase.
-   Automate build processes by integrating with a GitHub repository.

These topics will give us the foundation needed to use EAS effectively and to approach more advanced topics when needed.

This tutorial is hands-on and designed to be completed in about two hours.

#### Prerequisites

##### An existing Expo project set up locally

Pick one of the following options to follow along:

-   Continue with the Sticker Smash app from the previous tutorial. If new, download it from [GitHub](https://github.com/expo/examples/tree/master/stickersmash).
-   Start a new project with [`npx create-expo-app`](/get-started/create-a-project.md).
-   Use an [existing React Native project](/bare/overview.md). Ensure the `expo` package is installed, which you can do [automatically](/bare/installing-expo-modules.md) or [manually](/bare/installing-expo-modules.md#manual-installation).

## Tools

[Expo Orbit](https://expo.dev/orbit) to manage and launch builds with one click on macOS, Windows, and Linux.

If you want to install and run the build locally on your machine simultaneously, you can use Android Emulator or iOS Simulator. To set them up, see the following:

-   [Android Emulator](/workflow/android-studio-emulator.md)
-   [iOS Simulator](/workflow/ios-simulator.md) (available only on macOS)

## Next step

We're ready for this journey after setting up an Expo project locally. In the next chapter, let's learn how to create your first build with EAS Build.

[Start](/tutorial/eas/configure-development-build.md) — Let's start by configuring a development build.
