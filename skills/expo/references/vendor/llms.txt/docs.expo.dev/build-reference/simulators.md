---
modificationDate: July 01, 2024
title: Build for iOS Simulators
description: Learn how to configure and install build for iOS simulators when using EAS Build.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build-reference/simulators/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build-reference/simulators/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Build > Reference
Pages in this section:
- [Build lifecycle hooks](https://docs.expo.dev/build-reference/npm-hooks.md)
- [Using private npm packages](https://docs.expo.dev/build-reference/private-npm-packages.md)
- [Git submodules](https://docs.expo.dev/build-reference/git-submodules.md)
- [Using npm cache with Yarn 1 (Classic)](https://docs.expo.dev/build-reference/npm-cache-with-yarn.md)
- [Set up EAS Build with a monorepo](https://docs.expo.dev/build-reference/build-with-monorepos.md)
- [Build APKs for Android Emulators and devices](https://docs.expo.dev/build-reference/apk.md)
- [Build for iOS Simulators](https://docs.expo.dev/build-reference/simulators.md) (this page)
- [App version management](https://docs.expo.dev/build-reference/app-versions.md)
- [Troubleshoot build errors and crashes](https://docs.expo.dev/build-reference/troubleshooting.md)
- [Install app variants on the same device](https://docs.expo.dev/build-reference/variants.md)
- [iOS capabilities](https://docs.expo.dev/build-reference/ios-capabilities.md)
- [Run EAS Build locally](https://docs.expo.dev/build-reference/local-builds.md)
- [Cache dependencies](https://docs.expo.dev/build-reference/caching.md)
- [Android build process](https://docs.expo.dev/build-reference/android-builds.md)
- [iOS build process](https://docs.expo.dev/build-reference/ios-builds.md)
- [Configuration process](https://docs.expo.dev/build-reference/build-configuration.md)
- [Server infrastructure](https://docs.expo.dev/build-reference/infrastructure.md)
- [iOS App Extensions](https://docs.expo.dev/build-reference/app-extensions.md)
- [Ignore files via .easignore](https://docs.expo.dev/build-reference/easignore.md)
- [npx testflight](https://docs.expo.dev/build-reference/npx-testflight.md)
- [Repack app](https://docs.expo.dev/build-reference/repack.md)
- [Limitations](https://docs.expo.dev/build-reference/limitations.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Build for iOS Simulators

Learn how to configure and install build for iOS simulators when using EAS Build.

Running a build of your app on an iOS Simulator is useful. You can configure the build profile and install the build automatically on the simulator. This provides a standalone (independent of Expo Go) version of the app running without needing to deploy to TestFlight or even having an Apple Developer account.

## Configuring a profile to build for simulators

To install a build of your app on an iOS Simulator, modify the build profile in [**eas.json**](/build/eas-json.md) and set the `ios.simulator` value to `true`:

```json
{
  "build": {
    "preview": {
      "ios": {
        "simulator": true
      }
    },
    "production": {}
  }
}
```

Now, execute the command as shown below to run the build:

```sh
eas build -p ios --profile preview
```

Remember that a profile can be named whatever you like. In the above example, it is called `preview`. However, you can call it `local`, `simulator`, or whatever makes the most sense.

## Installing build on the simulator

> If you haven't installed or run the iOS Simulator before, follow the [iOS Simulator guide](/workflow/ios-simulator.md) before proceeding.

Once your build is completed, the CLI will prompt you to automatically download and install it on the iOS Simulator. When prompted, press Y to directly install it on the simulator.

In case you have multiple builds, you can also run the `eas build:run` command at any time to download a specific build and automatically install it on the iOS Simulator:

```sh
eas build:run -p ios
```

The command also shows a list of available builds of your project. You can select the build to install on the simulator from this list. Each build in the list has a build ID, the time elapsed since the build creation, the build number, the version number, and the git commit information. The list also displays invalid builds if a project has any.

For example, the image below lists two previous builds of a project:

When the build's installation is complete, it will appear on the home screen. If it's a development build, open a terminal window and start the development server by running the command `npx expo start`.

### Running the latest build

Pass the `--latest` flag to the `eas build:run` command to download and install the latest build on the iOS Simulator:

```sh
eas build:run -p ios --latest
```
