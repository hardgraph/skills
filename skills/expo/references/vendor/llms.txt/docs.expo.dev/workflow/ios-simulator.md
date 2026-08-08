---
modificationDate: June 30, 2026
title: iOS Simulator
description: Learn how you can install the iOS Simulator on your Mac and use it to develop your app.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/workflow/ios-simulator/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/workflow/ios-simulator/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Reference
Pages in this section:
- [Work with monorepos](https://docs.expo.dev/guides/monorepos.md)
- [View logs](https://docs.expo.dev/workflow/logging.md)
- [Development and production modes](https://docs.expo.dev/workflow/development-mode.md)
- [Common development errors](https://docs.expo.dev/workflow/common-development-errors.md)
- [Android Studio Emulator](https://docs.expo.dev/workflow/android-studio-emulator.md)
- [iOS Simulator](https://docs.expo.dev/workflow/ios-simulator.md) (this page)
- [New Architecture](https://docs.expo.dev/guides/new-architecture.md)
- [React Compiler](https://docs.expo.dev/guides/react-compiler.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# iOS Simulator

Learn how you can install the iOS Simulator on your Mac and use it to develop your app.

Developing your app directly on a computer can be more convenient than constantly interacting with an iPhone or iPad, especially when network conditions are slow or when a [tunnel connection](/more/expo-cli.md#tunneling) is required due to LAN limitations.

This guide explains how to install the iOS Simulator on your Mac for app development. Note that the iOS Simulator can only be installed on macOS. If you are developing an iOS app from a Windows or a Linux machine, you will need a physical iOS device.

## Setup Xcode and Watchman

### Install Xcode

Open up the Mac App Store, search for [Xcode](https://apps.apple.com/us/app/xcode/id497799835), and click **Install** (or **Update** if you have it already).

### Install Xcode Command Line Tools

Open Xcode, choose **Settings...** from the Xcode menu (or press cmd ⌘ + ,). Go to the **Locations** and install the tools by selecting the most recent version in the **Command Line Tools** dropdown.

### Install an iOS Simulator in Xcode

To install an iOS Simulator, open **Xcode > Settings... > Components**, and under **Platform Support > iOS ...**, click **Get**.

### Install Watchman

> Installing Watchman is only required for projects on SDK 55 and earlier.

[Watchman](https://facebook.github.io/watchman/docs/install#macos) is a tool for watching changes in the filesystem. Installing it will result in better performance. You can install it with:

```sh
brew update
brew install watchman
```

### Try it out

Run your app with `npx expo start` and press I from the command line.

You may get a warning about needing to accept the Xcode license. Run the command that it suggests. Open your app again to see if it was successful. If not, check the [troubleshooting](/workflow/ios-simulator.md#troubleshooting) tips below.

You can also press Shift + I in the Expo CLI to interactively select a simulator to open.

## Expo Orbit

You can use the Expo Orbit app which allows launching builds and simulator management with one click from the menu bar on macOS.

[Use Expo Orbit](/build/orbit.md) — Learn more about how to use Expo Orbit.

## Limitations

Although the iOS Simulator is great for rapid development, it does come with a few limitations. We'll list out a few of the main differences that affect Expo APIs below. However, see [Apple's documentation](https://help.apple.com/simulator/mac/current/#/devb0244142d) for more details.

The following hardware is unavailable in the Simulator:

-   Audio Input
-   Barometer
-   Camera
-   Motion Support (accelerometer and gyroscope)

The Simulator also suspends background apps and processes on iOS 11 and later.

## Troubleshooting

### The CLI seems to be stuck when opening a Simulator

Sometimes the iOS Simulator doesn't respond to the open command. If it seems stuck on this prompt, you can open the iOS Simulator manually (`open -a Simulator`) and then in the macOS toolbar, choose **File** > **Open Simulator**, and select an iOS version and device that you'd like to open.

You can use this menu to open any version of the simulator. You can also open multiple simulators at the same time, however, Expo CLI will always target the most recently opened simulator.

### Simulator opened but the Expo Go app isn't opening inside of it

The first time you install the app in the simulator, iOS will ask if you'd like to open the Expo Go app. You may need to interact with the simulator (click around, drag something) for this prompt to show up, then press **OK**.

### How do I install a specific version of Expo Go?

You can create a project with the desired SDK version and open it in a simulator to install the matching version of Expo Go.

```sh
# npm
npx create-expo-app --template blank@57
npx expo start --ios

# yarn
yarn create expo-app --template blank@57
yarn expo start --ios

# pnpm
pnpm create expo-app --template blank@57
pnpm expo start --ios

# bun
bun create expo --template blank@57
bun expo start --ios
```

Alternatively, you can download a specific version of Expo Go with the [`expo-go` CLI](https://www.npmjs.com/package/expo-go) by passing an SDK version, or use `latest` for the latest SDK version. This command downloads the Expo Go app to the current directory and caches it under **~/.expo**.

```sh
# npm
npx expo-go download ios latest

# yarn
yarn dlx expo-go download ios latest

# pnpm
pnpm dlx expo-go download ios latest

# bun
bunx expo-go download ios latest
```

### Expo CLI is printing an error message about `xcrun`, what do I do?

For miscellaneous errors, try the following:

-   Manually uninstall Expo Go on your simulator and reinstall by pressing Shift + I in the Expo CLI Terminal UI and selecting the desired simulator.
-   If that doesn't help, focus the simulator window and in the Mac toolbar choose **Device** > **Erase All Content and Settings...**  
    This will reinitialize your simulator from a blank image. This is sometimes useful for cases where your computer is low on memory and the simulator fails to store some internal files, leaving the device in a corrupt state.
