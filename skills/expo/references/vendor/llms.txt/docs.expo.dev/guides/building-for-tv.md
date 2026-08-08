---
modificationDate: July 20, 2026
title: Build Expo apps for TV
description: A guide for building an Expo app for an Android TV or Apple TV target.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/building-for-tv/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/building-for-tv/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Integrations > TV apps
Pages in this section:
- [Build apps for TV](https://docs.expo.dev/guides/building-for-tv.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Build Expo apps for TV

A guide for building an Expo app for an Android TV or Apple TV target.

> Not all Expo features and SDK libraries are available on TV. For more details, check the [See which libraries are supported](/guides/building-for-tv.md#see-which-libraries-are-supported).

React Native is supported on Android TV and Apple TV through the [React Native TV project](https://github.com/react-native-tvos/react-native-tvos). This technology extends beyond TV, offering a comprehensive core repo fork with support for phone and TV targets, including Hermes and Fabric.

Using the React Native TV library as the `react-native` dependency in an Expo project, it becomes capable of targeting both mobile (Android, iOS) and TV (Android TV, Apple TV) devices.

## Native project changes required

The necessary changes to the native Android and iOS files are minimal and can be automated with a [config plugin](https://github.com/react-native-tvos/config-tv/tree/main/packages/config-tv) if you use [prebuild](/more/glossary-of-terms.md#prebuild). Below is a list of changes made by the config plugins, which you can alternatively apply manually:

### Android

-   **AndroidManifest.xml** is modified:
    -   The default phone portrait orientation is removed
    -   The required intent for TV apps is added
-   **MainApplication.kt** is modified to remove unsupported Flipper invocations

### iOS

-   **ios/Podfile** is modified to target tvOS instead of iOS
-   The Xcode project is modified to target tvOS instead of iOS
-   The splash screen (**SplashScreen.storyboard**) is modified to work on tvOS

## System requirements for TV development

### Android TV

#### Prerequisites

##### Node.js (LTS)

Install [Node.js (LTS)](https://nodejs.org/en/) on macOS or Linux.

##### Android Studio (Iguana or later)

Install Android Studio Iguana or later.

##### Android TV system image

In the Android Studio SDK manager, select the dropdown for the Android SDK you are using (API version 31 or later), and make sure an Android TV system image is selected for installation. For Apple silicon, choose the ARM 64 image. Otherwise, choose the Intel x86_64 image.

##### Android TV emulator

After installing the Android TV system image, create an Android TV emulator using that image (the process is the same as creating an Android phone emulator).

### Apple TV

#### Prerequisites

##### Node.js (LTS)

Install [Node.js (LTS)](https://nodejs.org/en/) on macOS.

##### Xcode 16 or later

Install Xcode 16 or later.

##### tvOS SDK 17 or later

tvOS SDK 17 or later is not installed automatically with Xcode. You can install it later with `xcodebuild -downloadAllPlatforms`.

## Quick start

The fastest way to generate a new project is described in the [TV example](https://github.com/expo/examples/tree/master/with-tv) within the Expo examples repository:

```sh
# npm
npx create-expo-app MyTVProject -e with-tv

# yarn
yarn create expo-app MyTVProject -e with-tv

# pnpm
pnpm create expo-app MyTVProject -e with-tv

# bun
bun create expo MyTVProject -e with-tv
```

You can start with the [TV Router example](https://github.com/expo/examples/tree/master/with-router-tv):

```sh
# npm
npx create-expo-app MyTVProject -e with-router-tv

# yarn
yarn create expo-app MyTVProject -e with-router-tv

# pnpm
pnpm create expo-app MyTVProject -e with-router-tv

# bun
bun create expo MyTVProject -e with-router-tv
```

This creates a new project that uses [Expo Router](/router/introduction.md) for file-based navigation, modeled after the [**create-expo-app** default template](/get-started/create-a-project.md).

#### See which libraries are supported

At this time, TV applications work with the following libraries and APIs listed below:

-   [AppleAuthentication](/versions/latest/sdk/apple-authentication.md)
-   [Application](/versions/latest/sdk/application.md)
-   [Audio](/versions/latest/sdk/audio.md)
-   [Asset](/versions/latest/sdk/asset.md)
-   [AsyncStorage](/versions/latest/sdk/async-storage.md)
-   [AV](/versions/v54.0.0/sdk/av.md)
-   [BackgroundTask](/versions/latest/sdk/background-task.md)
-   [BlurView](/versions/latest/sdk/blur-view.md)
-   [BuildProperties](/versions/latest/sdk/build-properties.md)
-   [Constants](/versions/latest/sdk/constants.md)
-   [Crypto](/versions/latest/sdk/crypto.md)
-   [DevClient](/versions/latest/sdk/dev-client.md)
-   [Device](/versions/latest/sdk/device.md)
-   [Expo UI](/versions/latest/sdk/ui.md)
-   [FileSystem](/versions/latest/sdk/filesystem.md)
-   [FlashList](/versions/latest/sdk/flash-list.md)
-   [Font](/versions/latest/sdk/font.md)
-   [GlassEffect](/versions/latest/sdk/glass-effect.md)
-   [Image](/versions/latest/sdk/image.md)
-   [ImageManipulator](/versions/latest/sdk/imagemanipulator.md)
-   [KeepAwake](/versions/latest/sdk/keep-awake.md)
-   [LinearGradient](/versions/latest/sdk/linear-gradient.md)
-   [Localization](/versions/latest/sdk/localization.md)
-   [Manifests](/versions/latest/sdk/manifests.md)
-   [MediaLibrary](/versions/latest/sdk/media-library.md)
-   [NetInfo](/versions/latest/sdk/netinfo.md)
-   [Network](/versions/latest/sdk/network.md)
-   [Reanimated](/versions/latest/sdk/reanimated.md)
-   [SafeAreaContext](/versions/latest/sdk/safe-area-context.md)
-   [SecureStore](/versions/latest/sdk/securestore.md)
-   [Skia](/versions/latest/sdk/skia.md)
-   [SplashScreen](/versions/latest/sdk/splash-screen.md)
-   [SQLite](/versions/latest/sdk/sqlite.md)
-   [Svg](/versions/latest/sdk/svg.md)
-   [SystemUI](/versions/latest/sdk/system-ui.md)
-   [TaskManager](/versions/latest/sdk/task-manager.md)
-   [TrackingTransparency](/versions/latest/sdk/tracking-transparency.md)
-   [Updates](/versions/latest/sdk/updates.md)
-   [Video](/versions/latest/sdk/video.md)
-   [VideoThumbnails](/versions/latest/sdk/video-thumbnails.md)

TV also works with [React Navigation](https://reactnavigation.org/), [React Native Skia](https://shopify.github.io/react-native-skia/), and many other commonly used third-party React Native libraries. See [React Native directory](https://reactnative.directory/?tvos=true) to learn more about supported third-party libraries.

### Limitations

-   The [Expo DevClient](/versions/latest/sdk/dev-client.md) library is only supported in SDK 54 and later:
    -   **Android TV**: All operations are supported, similar to an Android phone.
    -   **Apple TV**: Basic operations with a local or tunneled packager are supported. Authentication to EAS and listing of EAS builds and updates is not yet supported.

## Integration with an existing Expo project

The following walkthrough describes the steps required to modify an Expo project for TV.

### Modify dependencies for TV

In **package.json**, modify the `react-native` dependency to use the TV repo.

#### SDK 56 and later

```json
{
  ... 
  "dependencies": {
    ... 
    "react-native": "npm:react-native-tvos@0.85-stable"
    ... 
  }
}
```

The `react-native-tvos` version must match the Expo SDK you are using. For example, Expo SDK 56 uses React Native 0.85, so you should use `react-native-tvos@0.85-stable` (the latest 0.85 version) as shown above. See the [SDK compatibility table](/versions/latest.md#each-expo-sdk-version-depends-on-a-react-native-version) for the correct version to use.

In SDK 56 and later, [upgrading your project to a newer SDK version](/workflow/upgrading-expo-sdk-walkthrough.md#upgrade-the-expo-sdk) also upgrades the TV repo dependency.

#### SDK 55 and earlier

You will need to exclude the `react-native` dependency from [`npx expo install` version validation](/more/expo-cli.md#configuring-dependency-validation).

```json
{
  ... 
  "dependencies": {
    ... 
    "react-native": "npm:react-native-tvos@0.83-stable"
    ... 
  },
  "expo": {
    "install": {
      "exclude": ["react-native"]
    }
  }
}
```

The `react-native-tvos` version must match the Expo SDK you are using. For example, Expo SDK 55 uses React Native 0.83, so you should use `react-native-tvos@0.83-stable` (the latest 0.83 version) as shown above. See the [SDK compatibility table](/versions/latest.md#each-expo-sdk-version-depends-on-a-react-native-version) for the correct version to use.

> If upgrading an Expo TV project to SDK 55 or earlier, the `react-native-tvos` version **must be upgraded manually**. It will not be automatically updated by `npx expo install expo@latest --fix`.

> If you have more than one Expo project in a monorepo, and one of them is modified for TV, then all of them should be modified to use the React Native TV package as described here, even if some of the projects are not configured to target TV. This avoids possible conflicts between the project dependencies, while still supporting mobile development fully on all the projects.

### Add the TV config plugin

```sh
# npm
npx expo install @react-native-tvos/config-tv -- --dev

# yarn
yarn expo install @react-native-tvos/config-tv -- --dev

# pnpm
pnpm expo install @react-native-tvos/config-tv -- --dev

# bun
bun expo install @react-native-tvos/config-tv -- --dev
```

When installed, the plugin will modify the project for TV when either:

-   The environment variable `EXPO_TV` is set to `1`
-   The plugin parameter `isTV` is set to `true`

Verify that this plugin appears in **app.json**:

```json
{
  "plugins": ["@react-native-tvos/config-tv"]
}
```

To see additional information on the plugin's actions during prebuild, you can set [debug environment variables](https://github.com/debug-js/debug#conventions) before running prebuild. (See also our documentation on [Expo CLI environment variables](/more/expo-cli.md#environment-variables).)

```sh
export DEBUG=expo:*
export DEBUG=expo:react-native-tvos:config-tv
```

### Run prebuild

Set the `EXPO_TV` environment variable, and run prebuild to make the TV modifications to the project.

```sh
export EXPO_TV=1
npx expo prebuild --clean
```

> **Note**: The `--clean` argument is recommended, and is required if you have existing Android and iOS directories in the project.

### Build for Android TV

Start an Android TV emulator and use the following command to start the app on the emulator:

```sh
# npm
npx expo run:android

# yarn
yarn expo run:android

# pnpm
pnpm expo run:android

# bun
bun expo run:android
```

### Build for Apple TV

Run the following command to build and run the app on an Apple TV simulator:

```sh
# npm
npx expo run:ios

# yarn
yarn expo run:ios

# pnpm
pnpm expo run:ios

# bun
bun expo run:ios
```

### Revert TV changes and build for phone

You can revert the changes for TV and go back to phone development by unsetting `EXPO_TV` and running prebuild again:

```sh
unset EXPO_TV
npx expo prebuild --clean
```

### Create EAS Build profiles for both TV and phone

Since the TV build can be driven by the value of an environment variable, it is easy to set up EAS Build profiles that build from the same source but target TV instead of phone.

The following example **eas.json** shows how to extend existing profiles (`development` and `preview`) to create TV profiles (`development_tv` and `preview_tv`).

```json
{
  "cli": {
    "version": ">= 5.2.0"
  },
  "build": {
    "base": {
      "distribution": "internal",
      "ios": {
        "simulator": true
      },
      "android": {
        "buildType": "apk",
        "withoutCredentials": true
      },
      "channel": "base"
    },
    "development": {
      "extends": "base",
      "android": {
        "gradleCommand": ":app:assembleDebug"
      },
      "ios": {
        "buildConfiguration": "Debug"
      },
      "channel": "development"
    },
    "development_tv": {
      "extends": "development",
      "env": {
        "EXPO_TV": "1"
      },
      "channel": "development"
    },
    "preview": {
      "extends": "base",
      "channel": "preview"
    },
    "preview_tv": {
      "extends": "preview",
      "env": {
        "EXPO_TV": "1"
      },
      "channel": "preview"
    }
  },
  "submit": {}
}
```

### Credentials for tvOS App Store builds

tvOS uses the same bundle IDs and distribution certificates as iOS and other Apple platforms. However, tvOS does not use the same provisioning profiles as iOS.

Since the EAS tooling on the web and in EAS CLI only creates iOS provisioning profiles, some extra steps are required for tvOS projects. There are two ways for you to proceed:

1.  If a project is targeting only tvOS, you can have your credentials stored in EAS. Create the bundle ID, and upload the distribution certificate on the project's website, as you would for an iOS project. Then, you should go to Apple's developer web site and create a tvOS provisioning profile there, using the bundle ID that EAS created. Then download that profile to your computer, and upload it to the project at [your EAS project website](https://expo.dev/accounts/%5Baccount%5D/projects/%5Bproject%5D/credentials/). See the [app credentials guide](/app-signing/app-credentials.md).
    
2.  If a project is targeting both tvOS and iOS, you should use [local credentials](/app-signing/local-credentials.md) for tvOS, as the EAS-stored credentials will be needed by your iOS builds.
    

## Examples and demonstration projects

[IgniteTV](https://github.com/react-native-tvos/IgniteTV) — A project generated with the Ignite CLI that can be built for mobile or TV.

[SkiaMultiplatform](https://github.com/react-native-tvos/SkiaMultiplatform) — Demonstrates React Native Skia on mobile, TV, and web.

[NativewindMultiplatform](https://github.com/react-native-tvos/NativewindMultiplatform) — Demonstrates using TailwindCSS styling on mobile, TV, and web.
