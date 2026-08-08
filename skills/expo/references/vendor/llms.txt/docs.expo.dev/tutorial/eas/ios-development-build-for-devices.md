---
modificationDate: July 09, 2026
title: Create and run a cloud build for iOS device
description: Learn how to configure a development build for iOS devices using EAS Build.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/eas/ios-development-build-for-devices/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/eas/ios-development-build-for-devices/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > EAS tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/eas/introduction.md)
- [Configure development build](https://docs.expo.dev/tutorial/eas/configure-development-build.md)
- [Android development build](https://docs.expo.dev/tutorial/eas/android-development-build.md)
- [iOS development build for simulators](https://docs.expo.dev/tutorial/eas/ios-development-build-for-simulators.md)
- [iOS development build for devices](https://docs.expo.dev/tutorial/eas/ios-development-build-for-devices.md) (this page)
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

# Create and run a cloud build for iOS device

Learn how to configure a development build for iOS devices using EAS Build.

In this chapter, we'll create a development build that can run on an iOS device with EAS Build.

Development builds for iOS devices are generated in the **.ipa** format, which is standard for iOS app installations.

[Watch: Creating a development build for iOS physical device](https://www.youtube.com/watch?v=HbfWU7_o4cU) — Learn how to build and run a development build on a physical iOS device using EAS Build, including setting up code signing.

#### Prerequisites

##### Apple Developer account

Required to access [necessary credentials](/app-signing/app-credentials.md#ios) for signing our app, as each build needs to be signed to verify that the app comes from a trusted source. EAS Build helps manage these credentials.

##### Developer Mode activated on iOS 16 and higher

Installing development builds on your device requires Developer Mode to be enabled. If this is your first time or if it's currently disabled, see [activate Developer Mode](/guides/ios-developer-mode.md).

## Provisioning profile

To initiate development on an iOS device, we have to:

-   Register the device by creating a new [provisioning profile](/app-signing/app-credentials.md#provisioning-profiles).
-   Download and install this profile onto the device.

### Register an iOS device

With EAS CLI, run the command to register a new Apple device:

```sh
eas device:create
```

This command prompts us with the following questions:

-   **You're inside the project directory. Would you like to use the** **your-account-name** **account?** Press Y.
-   **Apple ID.** For this step, enter your Apple ID. It will then log in to our Apple Developer account. Follow the steps in the terminal window.
-   **How would you like to register your devices?** Select **Website** that generates a registration URL that can be opened on the iOS device.

> **Tip**: If you or your team have multiple devices, you can share the provisioning profile link with those devices for downloading and installing the profile.

### Download and install profile

On a device's web browser, open the link provided in the previous step and tap the **Download Profile button**.

Open the **Settings** app, which prompts us to register our device.

Tap **Install** to register the iOS device.

After the provisioning profile is installed, our device redirects us back to the web browser, displaying a success message indicating the completion of the process.

## Development build for iOS device

### Create

To create a development build on an iOS device, make sure that under the `build.development` profile:

-   The `developmentClient` is set to `true` in **eas.json**, which is done by the default configuration.
-   Then, run the `eas build` command with `ios` as the platform and `development` as the build profile:

```sh
eas build --platform ios --profile development
```

> **Tip**: Next time you run `eas build` command, you can also use `-p` to specify the platform. It is short for `--platform`.

This command prompts us with the following questions when we create the build for the first time:

-   **What would you like your iOS bundle identifier to be?** Press Return to select the default value provided for this prompt. This will add [`ios.bundleIdentifier`](/versions/latest/config/app.md#package) in **app.json** if it isn't already defined.
-   **Do you want to log in to your Apple account?**. Since we are creating a development build for the first time, it will ask us to **Generate a new Apple Distribution Certificate**. Press Y both times.
-   **Select a device for ad hoc build**. This is the key part, which is why we had to register a provisioning profile before. We can select one or all of our registered devices here and then press return to install that build on those devices later.

> **On a new or recently renewed Apple Developer Program membership, a newly registered device may not be immediately installable.** Registering a device with Expo does not register it with Apple. The device is added to Apple only when it's first included in a build. For such a membership, Apple can then take [up to 24–72 hours](https://developer.apple.com/help/account/reference/device-registration-updates/) to finish processing the device before it can be added to a provisioning profile, so a build may fail the first time you include a newly registered device. If that happens, wait for Apple to finish processing and then run the build again.

> **Only if you have skipped [iOS Simulator chapter](/tutorial/eas/ios-development-build-for-simulators.md):** You'll be prompted **iOS app only uses standard/exempt encryption?** Press Y to select the default value provided for this prompt. Since our app doesn't use encryption, it sets `ITSAppUsesNonExemptEncryption` in the **Info.plist** file to `NO` and manages the compliance check for the same when you are releasing your app to TestFlight/Apple App Store. When you are releasing your own app, and it uses encryption, you can select `N` to skip this prompt next time.

After responding, the build will queue up, and we can track its progress via a provided link by the EAS CLI in the EAS dashboard:

#### What does a build details page contain?

The build details page displays the build type, profile, Expo SDK version, app version, build number, last commit hash, and the identity of the developer or account owner who initiated the build.

In the above image, the current status of the **Build artifact** shows that the build is in progress. Upon completion, this section will offer an option to download the build. The **Logs** outlines every step taken during the iOS build process on EAS Build. For the sake of brevity, we won't explore each step in detail here. To learn more, see [iOS build process](/build-reference/ios-builds.md).

#### What is iOS bundle identifier?

The `ios.bundleIdentifier` is a unique name of our app. If we publish our app right now, the Apple App Store will use this property and its value to identify our app on the store.

This notation is defined as `host.owner.app-name`. For example, our example app has `com.owner.stickersmash` where `com.owner` is the domain and `stickersmash` is our app name.

### Install

Once the build finishes, the Build artifact section gets updated, indicating that the build is complete:

This section provides the methods available for running the development build on an iOS device: Expo Orbit and Install button.

[Expo Orbit](https://expo.dev/orbit) allows for seamless installation of the development build on an iOS device. To use this method:

-   Connect our iOS device to our developer machine using USB.
-   Open the Orbit menu bar app.
-   Select the **Device** in the Orbit app.

-   On the EAS dashboard, under **Build artifact**, click the **Open with Orbit**.

After the build is installed, the Orbit app launches the development build on the device.

#### Alternate: Use the Install button and QR code

The **Install** button in the **Build artifact** section generates a QR code for easy installation:

-   Click **Install** to display a popup with the QR code.

-   Scan the QR code with our iOS device's camera to open and tap the link to download the development build on the device.

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

-   On the device, tap the app icon to open the development build.

-   Use the account syncing feature by ensuring we're logged into both the EAS CLI and development build. As we're already logged into the EAS CLI, the next step is to log in through the UI of your development build.

-   Tap **Fetch development servers** and select the server running from the list under Development servers.

## Summary

Chapter 4: Create and run a cloud build for iOS device

We successfully used EAS Build to create and run development builds on iOS devices.

In the next chapter, learn how to configure our app config to install multiple app variants on a single device.

[Next: Chapter 5: Configure multiple app variants](/tutorial/eas/multiple-app-variants.md)
