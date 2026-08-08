---
modificationDate: July 17, 2026
title: Using Google authentication
description: A guide on using react-native-nitro-google-signin or @react-native-google-signin/google-signin to integrate Google authentication in your Expo project.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/google-authentication/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/google-authentication/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Integrations > Authentication
Pages in this section:
- [Overview](https://docs.expo.dev/guides/using-authentication.md)
- [Using Clerk](https://docs.expo.dev/guides/using-clerk.md)
- [Using Facebook authentication](https://docs.expo.dev/guides/facebook-authentication.md)
- [Using Google authentication](https://docs.expo.dev/guides/google-authentication.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Using Google authentication

A guide on using react-native-nitro-google-signin or @react-native-google-signin/google-signin to integrate Google authentication in your Expo project.

You can integrate Google authentication in your Expo app using either of the following libraries:

-   [`react-native-nitro-google-signin`](https://react-native-nitro-google-sign-in.github.io): A library that uses modern native APIs.
-   [`@react-native-google-signin/google-signin`](https://github.com/react-native-google-signin/google-signin): A widely used library.

Both libraries provide native sign-in buttons and support authenticating the user (and obtaining authorization to use Google APIs). Because they require custom native code, you'll need to use a [config plugin](/config-plugins/introduction.md) in the [app config](/versions/latest/config/app.md) and build a development build.

## Choosing a library

When choosing between the two libraries, consider the following differences:

-   `react-native-nitro-google-signin` includes support for **Android Credential Manager**.
-   `@react-native-google-signin/google-signin` provides **Android Credential Manager APIs as part of their paid offering**.

> The legacy Google Sign-In SDK for Android (part of `com.google.android.gms:play-services-auth`) is deprecated and Google recommends migrating to **Android Credential Manager**. See [About the migration from legacy Google Sign-In](https://developer.android.com/identity/sign-in/legacy-gsi-migration).

This guide provides information on how to configure the library for your project.

#### Prerequisites

##### A development build

These libraries can't be used in Expo Go because they require custom native code. Learn more about [adding custom native code to your app](/workflow/customizing.md).

## Installation

Choose an installation guide based on the library you want to use:

[React Native Nitro Google Sign-In](https://react-native-nitro-google-sign-in.github.io)

[React Native Google Sign In: Expo installation instructions](https://react-native-google-signin.github.io/docs/setting-up/expo)

## Configure Google project for Android and iOS

Below are instructions on how to configure your Google project for Android and iOS.

### Upload app to Google Play Store

We recommend uploading the app to the Google Play Store if your app intends to run in production. You can submit your app to the stores for testing even if your project is still in development. This allows you to test Google Sign In when your app is signed by EAS for testing, and when it is signed by [Google Play App Signing](https://support.google.com/googleplay/android-developer/answer/9842756?hl=en) for store deployment. To learn more about the app submission process, see the guides below in the order they are specified:

[Create your first EAS Build](/build/setup.md)

[Build your project for app stores](/deploy/build-project.md)

[Submit to the Google Play Store with EAS Submit](/submit/android.md)

### Configure your Firebase or Google Cloud Console project

Refer to the library documentation for a more in-depth configuration guide:

-   [`react-native-nitro-google-signin`](https://react-native-nitro-google-sign-in.github.io/docs/setup/google-cloud)
-   [`@react-native-google-signin/google-signin`](https://react-native-google-signin.github.io/docs/setting-up/get-config-file)

For Android, once you have uploaded your app, you need to provide the SHA-1 certificate fingerprint values when asked while configuring the project in Firebase or Google Cloud Console. There are two types of values that you can provide:

-   Fingerprint of the **.apk** you built (on your machine or using EAS Build). You can find the SHA-1 certificate fingerprint in the Google Play Console under **Release** > **Setup** > **App Integrity** > **Upload key certificate**.
-   Fingerprint(s) of a **production app** downloaded from the play store. You can find the SHA-1 certificate fingerprint(s) in the Google Play Console under **Release** > **Setup** > **App Integrity** > **App signing key certificate**.

### With Firebase

For more instructions on how to configure your project for Android and iOS with Firebase:

[Firebase (Nitro Google Sign-In)](https://react-native-nitro-google-sign-in.github.io/docs/setup/expo#with-firebase--google-services-files-recommended)

[Firebase (@react-native-google-signin/google-signin)](https://react-native-google-signin.github.io/docs/setting-up/expo#expo-and-firebase-authentication)

#### Upload google-services.json and GoogleService-Info.plist to EAS

If you use the Firebase method for Android and iOS (as shared in sections above), you'll need to make sure **google-services.json** and **GoogleService-Info.plist** are available in EAS for building the app. You can check them into your repository because the files should not contain sensitive values, or you can treat the files as secrets, add them to **.gitignore** and use the guide below to make them available in EAS.

[Upload a secret file to EAS and use in the app config](/eas/environment-variables/usage.md#using-environment-variables-with-eas-build)

### With Google Cloud Console

This is an alternate method to configure a Google project when you are not using [Firebase](/guides/google-authentication.md#with-firebase).

For more instructions on how to configure your Google project Android and iOS with Google Cloud Console:

[Expo without Firebase (Nitro Google Sign-In)](https://react-native-nitro-google-sign-in.github.io/docs/setup/expo#without-firebase-manual-ios-url-scheme)

[Expo without Firebase (@react-native-google-signin/google-signin)](https://react-native-google-signin.github.io/docs/setting-up/expo#expo-without-firebase)
