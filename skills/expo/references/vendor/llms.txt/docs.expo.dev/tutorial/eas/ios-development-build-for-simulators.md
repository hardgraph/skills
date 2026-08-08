---
modificationDate: July 08, 2026
title: Create and run a cloud build for iOS Simulator
description: Learn how to configure a development build for iOS Simulators using EAS Build.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/eas/ios-development-build-for-simulators/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/eas/ios-development-build-for-simulators/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > EAS tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/eas/introduction.md)
- [Configure development build](https://docs.expo.dev/tutorial/eas/configure-development-build.md)
- [Android development build](https://docs.expo.dev/tutorial/eas/android-development-build.md)
- [iOS development build for simulators](https://docs.expo.dev/tutorial/eas/ios-development-build-for-simulators.md) (this page)
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

# Create and run a cloud build for iOS Simulator

Learn how to configure a development build for iOS Simulators using EAS Build.

In this chapter, we'll create a development build that can run on an iOS Simulator with EAS Build.

Development builds for iOS Simulators are generated in the **.app** format which is different from iOS devices.

[Watch: Creating a development build for iOS Simulator](https://www.youtube.com/watch?v=SgL97PFZctg) — Learn how to create a simulator build profile in eas.json and run a development build on iOS Simulator.

## Create a simulator build profile in eas.json

In **eas.json**, add a new build profile called `ios-simulator` with the property [`ios.simulator`](/eas/json.md#simulator) property. Set its value `true`:

```json
{
  "build": {
    "development": {
      ... 
    },
    "ios-simulator": {
      "ios": {
        "simulator": true
      }
    }
  }
}
```

For a development build, it's necessary to have the `developmentClient` and `distribution` properties defined in the profile. To avoid redundancy, we can extend the `development` profile properties:

```json
{
  "ios-simulator": {
    "extends": "development",
    "ios": {
      "simulator": true
    }
  }
}
```

## Development build for iOS Simulator

### Create

Run the `eas build` command with `ios` as a platform and `ios-simulator` as the build profile:

```sh
eas build --platform ios --profile ios-simulator
```

This command prompts us with the following questions when we create the build for the first time:

-   **What would you like your iOS bundle identifier to be?** Press Return to select the default value provided for this prompt. This will add [`ios.bundleIdentifier`](/versions/latest/config/app.md#package) in **app.json**.
-   **iOS app only uses standard/exempt encryption?** Press Y to select the default value provided for this prompt. Since our app doesn't use encryption, it sets `ITSAppUsesNonExemptEncryption` in the **Info.plist** file to `NO` and manages the compliance check for the same when you are releasing your app to TestFlight/Apple App Store. When you are releasing your own app, and it uses encryption, you can select `N` to skip this prompt next time.

After responding to the prompts, our EAS Build is queued, and the EAS CLI provides a link to view build details and track progress on the EAS dashboard:

#### What does a build details page contain?

The build details page displays the build type, profile, Expo SDK version, app version, build number, last commit hash, and the identity of the developer or account owner who initiated the build.

In the above image, the current status of the **Build artifact** shows that the build is in progress. Upon completion, this section will offer an option to download the build. The **Logs** outlines every step taken during the iOS build process on EAS Build. For the sake of brevity, we won't explore each step in detail here. To learn more, see [iOS build process](/build-reference/ios-builds.md).

#### What is iOS bundle identifier?

The `ios.bundleIdentifier` is a unique name of our app. If we publish our app right now, the Apple App Store will use this property and its value to identify our app on the store.

This notation is defined as `host.owner.app-name`. For example, our example app has `com.owner.stickersmash` where `com.owner` is the domain and `stickersmash` is our app name.

### Install

In the terminal, once the build finishes, EAS CLI prompts us by asking whether we want to run the build on an iOS Simulator. Press Y.

#### Alternate: Use Expo Orbit

You can use [Expo Orbit](https://expo.dev/orbit) to install the development build. From **Build artifact** on the EAS dashboard, click **Open with Expo Orbit** to install the development build on the iOS Simulator.

### Run

Start the development server by running the `npx expo start` command from the project directory:

```sh
# npm
npx expo start

# yarn
yarn expo start

# pnpm
pnpm expo start

# bun
bun expo start
```

Press I in the terminal window to open the project on the iOS Simulator.

## Summary

Chapter 3: Create and run a cloud build for iOS Simulator

We successfully used EAS Build to create and run development builds on iOS Simulators.

In the next chapter, let's create a development build for iOS, install it on a device, and get it running.

[Next: Chapter 4: Create and run a cloud build for iOS device](/tutorial/eas/ios-development-build-for-devices.md)
