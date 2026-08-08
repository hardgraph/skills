---
modificationDate: July 22, 2026
title: Create and run a cloud build for Android
description: Learn how to configure a development build for Android devices and emulators using EAS Build.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/eas/android-development-build/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/eas/android-development-build/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > EAS tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/eas/introduction.md)
- [Configure development build](https://docs.expo.dev/tutorial/eas/configure-development-build.md)
- [Android development build](https://docs.expo.dev/tutorial/eas/android-development-build.md) (this page)
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

# Create and run a cloud build for Android

Learn how to configure a development build for Android devices and emulators using EAS Build.

In this chapter, we'll create a development build that can run on Android with EAS Build.

The process for creating and running a build on Android devices or emulators is identical, with differences only in the installation of the development build.

[Watch: How to create and run a cloud build for Android](https://www.youtube.com/watch?v=D612BUtvvl8) — Learn how to create an Android development build with EAS Build and install it on a device or emulator.

## Create a build for the development profile

For Android, the development build must be in the **.apk**. While the default Android format is **.aab**, which is ideal for Google Play Store distribution, it cannot be installed on devices or emulators.

To create a **.apk**:

-   In **eas.json**, make sure that `developmentClient` is set to `true` under `build.development` profile.
    
-   Then, run the `eas build` command with `android` as the platform and `development` as the build profile:
    
    ```sh
    eas build --platform android --profile development
    ```
    
    > **Tip**: Next time you run `eas build` command, you can also use `-p` to specify the platform. It is short for `--platform`.
    

This command prompts us with the following questions:

-   **What would you like your Android application id to be?** Press Return to select the default value provided for this prompt. This will add [`android.package`](/versions/latest/config/app.md#package) in **app.json**.
-   **Generate a new Android Keystore**? Press Y.

After responding, the build will queue up, and we can track its progress via a provided link by the EAS CLI in the EAS dashboard:

#### What information does a build details page contain?

The build details page displays the build type, profile, Expo SDK version, app version, version code, last commit hash, and the identity of the developer or account owner who initiated the build.

In the above image, the current status of the **Build artifact** shows that the build is in progress. Upon completion, this section will offer an option to download the build. The **Logs** outlines every step taken during the Android build process on EAS Build. For the sake of brevity, we won't explore each step in detail here. To learn more, see [Android build process](/build-reference/android-builds.md).

#### What is an Android application ID?

Also known as the package name of our Android app, it stores the value in DNS reverse notation format (`com.owner.appname`). Each component of this notation should start with a lowercase letter.

For example, our example app has `com.owner.stickersmash` where `com.owner` is the domain and `stickersmash` is our app name.

## Android device

### Install development build

Once the build finishes, the **Build artifact** section gets updated, indicating that the build is complete:

This section provides the methods available for running the development build on an Android device: Expo Orbit and Install button.

[Expo Orbit](https://expo.dev/orbit) allows for seamless installation of the development build on an Android device. To use this method:

-   Connect our Android device to our local machine using USB.
-   Open the Orbit app.
-   Select the **Device** in the Orbit app.

-   On the EAS dashboard, under **Build artifact**, click the **Open with Orbit**.

After the build is installed, the Orbit app launches the development build on the device.

#### Alternate: Use the Install button and QR code

The **Install** button in the **Build artifact** generates a QR code for installation:

-   Click **Install** to display a popup with the QR code.

-   Scan the QR code with our Android device's camera to open the build link in the default web browser.
-   Tap the **Install** button on the webpage to download the **.apk** file.
-   Once downloaded, open the **.apk** to start the installation process.
-   If an **Unsafe app blocked message** appears, select **Install anyway**. This warning can safely be ignored as the source of the **.apk** (which we generated) is trusted.

### Run development build

Start the development server by running `npx expo start` from the project directory. Once the server is running, press A in the terminal window to open the project:

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

## Android Emulator

### Install the development build

In the terminal, once the build finishes, EAS CLI prompts us by asking whether we want to run the build on an Android Emulator. Press Y.

#### Alternate: Use Expo Orbit

Alternatively, [Expo Orbit](/build/orbit.md) can be used for installation. From **Build artifact** on the EAS dashboard, click **Open with Expo Orbit** to install the development build on the Android Emulator.

### Run the development build

Start the development server by running `npx expo start` from the project directory. Once the server is running, press A in the terminal window to open the project:

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

## Summary

Chapter 2: Create and run a cloud build for Android

We successfully used EAS Build to create and run development builds on Android devices and emulators, and learned about **.apk** and **.aab** file formats.

In the next chapter, learn how to configure a development build for iOS Simulators using EAS Build and get it running.

[Next: Chapter 3: Create and run a cloud build for iOS Simulator](/tutorial/eas/ios-development-build-for-simulators.md)
