---
modificationDate: July 17, 2026
title: Using Facebook authentication
description: A guide on using react-native-fbsdk-next library to integrate Facebook authentication in your Expo project.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/facebook-authentication/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/facebook-authentication/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Integrations > Authentication
Pages in this section:
- [Overview](https://docs.expo.dev/guides/using-authentication.md)
- [Using Clerk](https://docs.expo.dev/guides/using-clerk.md)
- [Using Facebook authentication](https://docs.expo.dev/guides/facebook-authentication.md) (this page)
- [Using Google authentication](https://docs.expo.dev/guides/google-authentication.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Using Facebook authentication

A guide on using react-native-fbsdk-next library to integrate Facebook authentication in your Expo project.

The [`react-native-fbsdk-next`](https://github.com/thebergamo/react-native-fbsdk-next/) library provides a wrapper around Facebook's Android and iOS SDKs. It allows integrating Facebook authentication into your Expo project and provide access to native components.

This guide provides additional information on configuring the library with Expo for Android.

#### Prerequisites

##### A development build

The `react-native-fbsdk-next` library can't be used in Expo Go because it requires custom native code. Learn more about [adding custom native code to your app](/workflow/customizing.md).

## Installation

See `react-native-fbsdk-next` documentation for instructions on how to install and configure the library:

[React Native FBSDK Next: Expo installation instructions](https://github.com/thebergamo/react-native-fbsdk-next/#expo-installation)

## Configuration for Android

Adding Android as a platform in your Facebook project requires you to have your app approved by Google Play Store so that it has a valid Play Store URL, and the [`package`](/versions/latest/config/app.md#package) name associated with your app. Otherwise, you'll run into the following error:

See the following guides for more information on how to build your project for app stores:

[Build your project for app stores](/deploy/build-project.md)

[Submit to the Google Play Store with EAS Submit](/submit/android.md)

Once you have uploaded the app to the Play Store you can submit your app review. When it is approved the Facebook project will be able to access it at a Play Store URL.

After that, go to your Facebook project's **Settings** > **Basic** and add the **Android** platform. You'll need to provide the Key hash, Package name and Class name.

-   To add Key hash, go to your Play Store Console to obtain the SHA-1 certificate fingerprint from **Release** > **Setup** > **App Integrity** > **App signing key certificate**. Then, [convert the value of the Hex value of the certificate to Base64](https://base64.guru/converter/encode/hex) and add it under the **Android** > **Key hashes** in your Facebook project.
-   You can find the Package name in your [app config](/versions/latest/config/app.md) under the [`android.package`](/versions/latest/config/app.md#package) field.
-   The Class name is `MainActivity` by default, and you can use `package.MainActivity` where `package` is the `android.package` in your project's app config. For example, `com.myapp.example.MainActivity`, where `com.myapp.example` is the `package` name of your app.
-   Then, click **Save changes** to save the configuration.

Now, you can use your Facebook project for development or release builds and production apps.
