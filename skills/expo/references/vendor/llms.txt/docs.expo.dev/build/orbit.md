---
modificationDate: April 28, 2026
title: Expo Orbit
description: Accelerate your development workflow with one-click build and update launches and simulator management.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build/orbit/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build/orbit/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Build
Pages in this section:
- [Introduction](https://docs.expo.dev/build/introduction.md)
- [Create your first build](https://docs.expo.dev/build/setup.md)
- [Configure with eas.json](https://docs.expo.dev/build/eas-json.md)
- [Internal distribution](https://docs.expo.dev/build/internal-distribution.md)
- [Automate submissions](https://docs.expo.dev/build/automate-submissions.md)
- [Using EAS Update](https://docs.expo.dev/build/updates.md)
- [Trigger builds from CI](https://docs.expo.dev/build/building-on-ci.md)
- [Trigger builds from GitHub App](https://docs.expo.dev/build/building-from-github.md)
- [Expo Orbit](https://docs.expo.dev/build/orbit.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo Orbit

Accelerate your development workflow with one-click build and update launches and simulator management.

[Expo Orbit](https://expo.dev/orbit) for macOS, Windows, and Linux enables faster to install and launch builds or updates from EAS, local files, or run Snack projects, on simulators and physical devices.

## Why Orbit

Before Orbit, installing builds or updates from EAS (on Android and iOS physical devices or emulator/simulator) or running Snack projects on simulators was manual. You had to run `eas build:run` command and select a build for your chosen device or download the archive and then drag and drop it to the simulator (in the case of iOS). Also, for Snack projects, additional steps included installing Expo Go on the virtual device, logging in, and then selecting the Snack from the list. Orbit makes all of these steps as seamless as possible.

## Highlights

-   List and launch simulators, including running Android emulators without audio.
-   Install and launch builds from EAS on simulators and real devices in one click.
-   [Install and open updates from EAS](/review/with-orbit.md) on Android Emulators or iOS Simulators.
-   Launch Snack projects in your simulators in one click.
-   Install and launch apps from local files using Finder or drag and drop a file into the menu bar app. Orbit supports any Android **.apk**, iOS Simulator compatible **.app**, or ad hoc signed apps.
-   See pinned projects from your [EAS dashboard](https://expo.dev) and quickly launch your latest builds.

## Installation

> Orbit relies on the Android SDK on macOS, Windows, and Linux, and `xcrun` for device management only on macOS, which requires setting up both [Android Studio](/workflow/android-studio-emulator.md) and [Xcode](/workflow/ios-simulator.md).

#### macOS

You can download Orbit with Homebrew for macOS, or directly from the [GitHub releases](https://github.com/expo/orbit/releases).

```sh
brew install expo-orbit
```

If you want Orbit to start when you log in automatically, click on the Orbit icon in the menu bar, then **Settings** and select the **Launch on Login** option.

#### Windows

You can download Orbit for Windows directly from the [GitHub releases](https://github.com/expo/orbit/releases).

#### Linux

You can download Orbit for Linux directly from the [GitHub releases](https://github.com/expo/orbit/releases). Both `.deb` (Debian and Ubuntu) and `.rpm` (Fedora and RHEL) packages are available.
