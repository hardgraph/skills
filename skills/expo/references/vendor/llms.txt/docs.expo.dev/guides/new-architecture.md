---
modificationDate: July 28, 2026
title: React Native's New Architecture
description: Learn about React Native's "New Architecture" and how and why to migrate to it.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/new-architecture/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/new-architecture/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [iOS Simulator](https://docs.expo.dev/workflow/ios-simulator.md)
- [New Architecture](https://docs.expo.dev/guides/new-architecture.md) (this page)
- [React Compiler](https://docs.expo.dev/guides/react-compiler.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# React Native's New Architecture

Learn about React Native's "New Architecture" and how and why to migrate to it.

> **SDK 55 and later run entirely on the New Architecture.** The New Architecture is always enabled and cannot be disabled. If you need to use the legacy architecture, use SDK 54 or earlier.

The New Architecture is a name that we use to describe a complete refactoring of the internals of React Native. It is also used to solve limitations of the original React Native architecture discovered over years of usage in production at Meta and other companies.

In this guide, we'll talk about how to use the New Architecture in Expo projects today.

[New Architecture is here](https://reactnative.dev/blog/2024/10/23/the-new-architecture-is-here) — A blog post from the React Native team at Meta that gives an overview of the features of the New Architecture and the motivations behind building it.

[React Native 0.82 - A New Era](https://reactnative.dev/blog/2025/10/08/react-native-0.82) — React Native 0.82 is the first version that runs entirely on the New Architecture. SDK 55 uses React Native 0.83, which inherits this behavior.

#### Why migrate to the New Architecture?

**The New Architecture is the present and future of React Native**. Starting with React Native 0.82, the New Architecture is always enabled and cannot be disabled. SDK 55 uses React Native 0.83, which inherits this behavior. The [legacy architecture was frozen](https://github.com/reactwg/react-native-new-architecture/discussions/290) in June 2025, meaning no new features or bugfixes are being developed for it.

**New React and React Native features are coming to the New Architecture only**. For example, the New Architecture includes [full support for Suspense](https://reactnative.dev/blog/2024/10/23/the-new-architecture-is-here#full-support-for-suspense) and [new styling capabilities](https://reactnative.dev/blog/2025/01/21/version-0.77#new-css-features-for-better-layouts-sizing-and-blending) that are not implemented in the legacy architecture. Many popular libraries now only support the New Architecture.

**If you are on SDK 54 or earlier**, you can still use the legacy architecture by setting `newArchEnabled` to `false`. However, you will need to migrate to the New Architecture before upgrading to SDK 55 or later.

## Expo tools and the New Architecture

As of SDK 53, all `expo-*` packages in the [Expo SDK](/versions/latest.md) support the New Architecture (including [bridgeless](https://github.com/reactwg/react-native-new-architecture/discussions/154)). [Learn more about known issues](/guides/new-architecture.md#known-issues-in-expo-libraries).

Additionally, all modules written using the [Expo Modules API](/modules/overview.md) support the New Architecture by default! So if you have built your own native modules using this API, no additional work is needed to use them with the New Architecture.

**As of January 2026, approximately 83% of SDK 54 projects built with [EAS Build](/build/introduction.md) use the New Architecture**.

## Third-party libraries and the New Architecture

The compatibility status of many of the most popular libraries is tracked on [React Native Directory](https://reactnative.directory/) ([learn more about known issues in third-party libraries](/guides/new-architecture.md#known-issues-in-third-party-libraries)). We've built tooling into Expo Doctor to integrate with React Native Directory to help you validate your dependencies, so you can quickly learn which libraries are unmaintained and which incompatible or untested with the New Architecture.

### Validate your dependencies with React Native Directory

Run `npx expo-doctor` to check your dependencies against the data in React Native Directory.

```sh
# npm
npx expo-doctor@latest

# yarn
yarn dlx expo-doctor@latest

# pnpm
pnpm dlx expo-doctor@latest

# bun
bunx expo-doctor@latest
```

You can configure the React Native Directory check in your **package.json** file. For example, if you would like to exclude a package from validation:

```json
{
  "expo": {
    "doctor": {
      "reactNativeDirectoryCheck": {
        "exclude": ["react-redux"]
      }
    }
  }
}
```

#### See all available options

-   **enabled**: If `true`, the check will warn if any packages are missing from React Native Directory. Set this to `false` to disable this behavior. In SDK 52 and later, this is set to `true` by default, otherwise it is `false` by default. You can also override this setting with the `EXPO_DOCTOR_ENABLE_DIRECTORY_CHECK` environment variable (0 is `false`, 1 is `true`).
-   **exclude**: List any packages you want to exclude from the check. Supports exact package names and regex patterns. For example, `["exact-package", "/or-a-regex-.*/"]`.
-   **listUnknownPackages**: By default, the check will warn if any packages are missing from React Native Directory. Set this to false to disable this behavior.

## Initialize a new project with the New Architecture

**As of SDK 52**, all new projects will be initialized with the New Architecture enabled by default.

```sh
# npm
npx create-expo-app@latest --template default@sdk-57

# yarn
yarn create expo-app --template default@sdk-57

# pnpm
pnpm create expo-app --template default@sdk-57

# bun
bun create expo --template default@sdk-57
```

## Enable the New Architecture in an existing project

#### SDK 55 and later

**The New Architecture is always enabled in SDK 55 and later**. There is no option to disable it. SDK 55 uses React Native 0.83. [React Native 0.82 was the first version to remove the option to disable the New Architecture](https://reactnative.dev/blog/2025/10/08/react-native-0.82), and this applies to all later versions.

If you were previously using `newArchEnabled: false` in your app config, this setting will be ignored. Remove it from your configuration to avoid confusion.

#### SDK 53 and SDK 54

**The New Architecture is enabled by default in SDK 53 and SDK 54**. If you have explicitly disabled it, remove that configuration to enable it.

> SDK 54 is the last SDK version where the New Architecture can be disabled.

#### SDK 52

We recommend upgrading to SDK 53 to ensure that your app can take advantage of all of the latest New Architecture related fixes and improvements with libraries and React Native itself. If you want to try it on SDK 52 first, follow the instructions below.

To enable it on both Android and iOS, use the `newArchEnabled` at the root of the `expo` object in your app config. You can selectively enable it for a single platform by setting, for example, `"android": { "newArchEnabled": true }`.

```json
{
  "expo": {
    "newArchEnabled": true
  }
}
```

Create a new build:

#### Android

```sh
# npm
npx expo prebuild --clean && npx expo run:android
eas build -p android

# yarn
yarn expo prebuild --clean && yarn expo run:android
eas build -p android

# pnpm
pnpm expo prebuild --clean && pnpm expo run:android
eas build -p android

# bun
bun expo prebuild --clean && bun expo run:android
eas build -p android
```

#### iOS

```sh
# npm
npx expo prebuild --clean && npx expo run:ios
eas build -p ios

# yarn
yarn expo prebuild --clean && yarn expo run:ios
eas build -p ios

# pnpm
pnpm expo prebuild --clean && pnpm expo run:ios
eas build -p ios

# bun
bun expo prebuild --clean && bun expo run:ios
eas build -p ios
```

If the build succeeds, you will now be running your app with the New Architecture! Depending on the native modules you use, your app may work properly immediately.

Now you can tap around your app and test it out. For most non-trivial apps, you're likely to encounter some issues, such as missing native views that haven't been implemented for the New Architecture yet. Many of the issues you encounter are actionable and can be resolved with some configuration or code changes. We recommend reading [Troubleshooting](/guides/new-architecture.md#troubleshooting) sections below for more information.

#### SDK 51 and earlier

We recommend upgrading to SDK 54 or SDK 55. SDK 51 is significantly outdated, and the [legacy architecture has been frozen](https://github.com/reactwg/react-native-new-architecture/discussions/290), meaning it will not receive new features or bugfixes. It's possible to enable the New Architecture in SDK 51, but you are likely to encounter a variety of issues that have been resolved in more recent releases. To enable it, you need to [install the `expo-build-properties` plugin](/versions/latest/sdk/build-properties.md#installation) and set `newArchEnabled` on target platforms.

#### Are you enabling the New Architecture in an existing React Native project?

If you are using Expo SDK 53 or later, the New Architecture is enabled by default. For SDK 55 and later, the New Architecture is always enabled and cannot be disabled. The following instructions apply to SDK 52 and earlier projects.

-   **Android**: Set `newArchEnabled=true` in the **gradle.properties** file.
-   **iOS**: If your project has a **Podfile.properties.json** file (which is created by `npx create-expo-app` or `npx expo prebuild`), you can enable the New Architecture by setting the `newArchEnabled` property to `"true"` in the **Podfile.properties.json** file. Otherwise, refer to the ["Enable the New Architecture for Apps"](https://github.com/reactwg/react-native-new-architecture/blob/main/docs/enable-apps.md) section of the React Native New Architecture working group.

## Disable the New Architecture in an existing project

> **SDK 55 and later do not support disabling the New Architecture.** SDK 55 uses React Native 0.83. Starting with [React Native 0.82, the option to disable the New Architecture was removed](https://reactnative.dev/blog/2025/10/08/react-native-0.82), so setting `newArchEnabled` to `false` has no effect. If you need to use the legacy architecture, use SDK 54 or earlier.

> Expo Go only supports the New Architecture.

On **SDK 54 and earlier**, you can opt out of the New Architecture by setting the `newArchEnabled` property to `false` in app config and create a [development build](/develop/development-builds/introduction.md).

```json
{
  "expo": {
    "newArchEnabled": false
  }
}
```

#### Are you disabling the New Architecture in an existing React Native project (SDK 54 and earlier)?

-   **Android**: Set `newArchEnabled=false` in the **gradle.properties** file.
-   **iOS**: If your project has a **Podfile.properties.json** file (which is created by `npx create-expo-app` or `npx expo prebuild`), you can disable the New Architecture by setting the `newArchEnabled` property to `"false"` in the **Podfile.properties.json** file. Otherwise, refer to the ["Enable the New Architecture for Apps"](https://github.com/reactwg/react-native-new-architecture/blob/main/docs/enable-apps.md) section of the React Native New Architecture working group.

## Troubleshooting

Meta and Expo are working toward making the New Architecture the default for all new apps and ensuring it is as easy as possible to migrate existing apps. However, the New Architecture isn't just a name — many of the internals of React Native has been re-architected and rebuilt from the ground up. As a result, you may encounter issues when enabling the New Architecture in your app. The following is some advice for troubleshooting these issues.

#### Can I still try the New Architecture even if some of the libraries I use aren't supported?

You may be able to try the New Architecture in your app even if some of the libraries you use aren't supported, but it will require temporarily removing those libraries. Create a new branch in your repository and remove any of the libraries that aren't compatible until your app is running. This will give you a good idea of what libraries still need work before you can fully migrate to the New Architecture. We recommend creating issues or pull requests on those libraries' repositories to help them become compatible with the New Architecture. Alternatively, you could switch to other libraries that are compatible with the New Architecture. Refer to [React Native Directory](https://reactnative.directory/) to find compatible libraries.

#### Known issues in React Native

Refer to the [issues labeled with "Type: New Architecture" on the React Native GitHub repository](https://github.com/facebook/react-native/issues?q=is%3Aopen+is%3Aissue+label%3A%22Type%3A+New+Architecture%22).

#### Known issues in Expo libraries

There are no known issues specific to the New Architecture in Expo libraries.

#### Known issues in third-party libraries

Since React Native 0.74, there are various Interop Layers enabled by default. This allows many libraries built for the old architecture to work on the New Architecture without any changes. However, the interop is not perfect and some libraries will need to be updated. The libraries that are most likely to require updates are those that ship or depend on third-party native code. [Learn more about library support in the New Architecture](https://github.com/reactwg/react-native-new-architecture/discussions/167).

Refer to [React Native Directory](https://reactnative.directory/) a more complete list of libraries and their compatibility with the New Architecture. The following libraries were found to be popular among Expo apps and are known to be incompatible:

The following are known issues with libraries that are popular among Expo apps.

-   **react-native-maps**: Version 1.20.x, which is the default for SDK 53, supports the New Architecture with the interop layer and works well for most features. A New Architecture-first version is available in version 1.21.0, which is still stabilizing. We encourage your to test it in your app, report issues that you find, and [follow along with the discussion on GitHub](https://github.com/react-native-maps/react-native-maps/discussions/5355). We are also investigating another approach that may provider a smoother migration path, by leaning on the [interop layer](https://github.com/reactwg/react-native-new-architecture/discussions/175) rather than rewriting the module. It's worth mentioning that if your app can force a minimum version of iOS 17, or does not need to support maps on iOS, then you can consider using [`expo-maps`](/versions/latest/sdk/maps.md) instead.
-   **@stripe/react-native**: The New Architecture is supported starting with version 0.45.0, which is the default for SDK 53.
-   **@react-native-community/masked-view**: Use `@react-native-masked-view/masked-view` instead.
-   **@react-native-community/clipboard**: Use `@react-native-clipboard/clipboard` instead.
-   **rn-fetch-blob**: Use `react-native-blob-util` instead.
-   **react-native-fs**: Use `expo-file-system` or [a fork of react-native-fs](https://github.com/birdofpreyru/react-native-fs) instead.
-   **react-native-geolocation-service**: Use `expo-location` instead.
-   **react-native-datepicker**: Use `react-native-date-picker` or `@react-native-community/datetimepicker` instead.

#### My build failed after enabling the New Architecture

This isn't entirely surprising! Not all libraries are compatible yet, and in some cases compatibility was only recently added and so you will want to ensure you update your libraries to their latest versions. Read the logs to determine which library is incompatible. Also, run `npx expo-doctor@latest` to check your dependencies against the data in React Native Directory.

When you are using the latest version of a library and it is not compatible, report any issues you encounter to the respective GitHub repository. Create a [minimal reproducible example](https://stackoverflow.com/help/minimal-reproducible-example) and report the issue to the library author. If you believe the issue originates in React Native itself, rather than a library, report it to the React Native team (again, with a minimal reproducible example).
