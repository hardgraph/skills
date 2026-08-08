---
modificationDate: July 17, 2026
title: Development builds FAQ
description: A collection of common questions about development builds, Expo Go, and EAS Build.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/develop/development-builds/faq/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/develop/development-builds/faq/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > Develop > Development builds
Pages in this section:
- [Introduction and setup](https://docs.expo.dev/develop/development-builds/introduction.md)
- [Use a build](https://docs.expo.dev/develop/development-builds/use-development-builds.md)
- [Share with your team](https://docs.expo.dev/develop/development-builds/share-with-your-team.md)
- [Tools, workflows and extensions](https://docs.expo.dev/develop/development-builds/development-workflows.md)
- [FAQ](https://docs.expo.dev/develop/development-builds/faq.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Development builds FAQ

A collection of common questions about development builds, Expo Go, and EAS Build.

This page answers common questions about development builds, how they compare to Expo Go, and where to learn more about EAS Build.

## Learn more about development builds and Expo Go

#### Difference between Expo Go and development builds

[Expo Go](https://expo.dev/go) is a playground app for students and learners to get started quickly. It comes with a fixed set of native libraries built in, so you can write JavaScript code and see changes instantly without building a native app yourself. A development build is a fully featured development environment for working on your production-grade Expo apps.

#### Native app and JavaScript bundle

The **native app** is what you install on your device. Expo Go is a pre-built native app that works like a playground — it can't be changed after you install it. To add new native libraries or change things like your app name and icon, you need to build your own native app (a development build).

The **JavaScript bundler (`expo start`)** is where your app's UI code and business logic are. In production apps, there is one **main.js** bundle that is shipped with the app itself. In development, this JS bundle is live reloaded from your local machine. The main role of React Native is to provide a way for the JavaScript code to access the native APIs (Image, Camera, Notifications, and more). However, only APIs and libraries that were bundled in the **native app** can be used.

## Why use a development build (a.k.a what _can't_ you do in Expo Go and why)

Expo Go is a playground for students and learners to understand the basics of React Native. It's limited and not useful for building production-grade projects, so most apps will convert to using development builds. It helps to know exactly what is _impossible_ in Expo Go and _why_, so you can make an informed decision on when and why to make this move.

#### Use libraries with native code that aren't in Expo Go

Consider [`react-native-webview`](/versions/latest/sdk/webview.md) as an example, a library that contains native code, but [is included in Expo Go](https://github.com/expo/expo/blob/main/apps/expo-go/package.json#L23). When you run `npx expo install react-native-webview` command in your project, it will install the library in your **node_modules** directory, which includes both the JS code and the native code. But the JS bundle you are building _only_ uses the JS code. Then, your JS bundle gets uploaded to Expo Go, and it interacts with the native code that was already bundled with the app.

Instead, when you try to use a library that is not included, for example, [`react-native-firebase`](/guides/using-firebase.md#using-react-native-firebase), then you can use the JS code and hot reload the new bundle into Expo Go but it will immediately error because the JS code tries to call the native code from the React Native Firebase package that does not exist in Expo Go. There is no way to get the native code into the Expo Go app unless it was already included in the bundle that was uploaded to the app stores.

#### Test changes in app icon, name, splash screen

If you're developing your app in Expo Go only, you can build a store version that will use your provided values and images; it just won't be possible to test it in Expo Go.

These native assets are shipped with the native bundle and are immutable once the app is installed. The Expo Go app does show a splash screen, which is your app icon on a solid color background. This is a dev-only emulation to view how the splash screen will probably look. However, it is limited, for example, you cannot test `SplashScreen.setOptions` to animate the splash screen.

#### Remote push notifications

While [in-app notifications](/versions/latest/sdk/notifications.md) are available in Expo Go, remote push notifications (that is, sending a push notification from a server to the app) are not. This is because a push notification service should be tied to your own push notification certificates, and while it is possible to make it work in Expo Go, it often causes confusion for production builds. It is recommended to test remote push notifications in development builds so you can ensure parity in behavior between development and production.

#### Implementing App/Universal links

Both [Android App Links](/linking/android-app-links.md) and [iOS Universal Links](/linking/ios-universal-links.md) require a two-way association between the native app and the website. In particular, it requires the native app to include the linked website's URL. This is impossible with Expo Go due to the aforementioned native code immutability.

#### Open projects using other SDK versions (iOS device only)

Expo Go can only support one SDK version at a time, and only that version is available to install from the app stores. The store version of Expo Go currently supports SDK 54.

If you're developing on an Android device, Android Emulator, or iOS Simulator, a compatible version of Expo Go can be [downloaded and installed](https://expo.dev/go). You can also use the [`expo-go` CLI](/develop/tools.md#expo-go-cli) to download a specific version. The only platform where this is impossible is iPhone devices because Apple does not support side-loading other versions of apps.

## Learn more about EAS Build

#### How do I configure EAS Build with eas.json?

See [Configuring EAS Build with eas.json](/build/eas-json.md) to learn how a project using EAS services is configured.

#### How do I use environment variables in my project?

See [Environment variables](/guides/environment-variables.md) to learn about different ways to use environment variables in an Expo project.

#### How is an Android project built on EAS Build?

See [Android build process](/build-reference/android-builds.md) to learn how an Android project is built on EAS Build.

#### How is an iOS project built on EAS Build?

See [iOS build process](/build-reference/ios-builds.md) to learn how an iOS project is built on EAS Build.

#### How do I set up EAS Build with a monorepo?

See [Set up EAS Build with a monorepo](/build-reference/build-with-monorepos.md) for a step-by-step guide.

## Video walkthroughs

[EAS Tutorial Series](https://www.youtube.com/playlist?list=PLsXDmrmFV_AS14tZCBin6m9NIS_VCUKe2) — A course on YouTube: learn how to speed up your development with Expo Application Services.

[Async Office Hours: How to make a development build with EAS Build](https://www.youtube.com/watch?v=LUFHXsBcW6w) — Learn how to make a development build with EAS Build in this video tutorial hosted by Developer Success Engineer: Keith Kurak.
