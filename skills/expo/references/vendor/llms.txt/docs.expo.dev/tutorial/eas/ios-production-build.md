---
modificationDate: July 22, 2026
title: Create a production build for iOS
description: Learn about the process of creating a production build for iOS and automating the release process.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/eas/ios-production-build/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/eas/ios-production-build/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > EAS tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/eas/introduction.md)
- [Configure development build](https://docs.expo.dev/tutorial/eas/configure-development-build.md)
- [Android development build](https://docs.expo.dev/tutorial/eas/android-development-build.md)
- [iOS development build for simulators](https://docs.expo.dev/tutorial/eas/ios-development-build-for-simulators.md)
- [iOS development build for devices](https://docs.expo.dev/tutorial/eas/ios-development-build-for-devices.md)
- [Multiple app variants](https://docs.expo.dev/tutorial/eas/multiple-app-variants.md)
- [Internal distribution build](https://docs.expo.dev/tutorial/eas/internal-distribution-builds.md)
- [Manage app versions](https://docs.expo.dev/tutorial/eas/manage-app-versions.md)
- [Android production build](https://docs.expo.dev/tutorial/eas/android-production-build.md)
- [iOS production build](https://docs.expo.dev/tutorial/eas/ios-production-build.md) (this page)
- [Share previews](https://docs.expo.dev/tutorial/eas/team-development.md)
- [Builds from GitHub](https://docs.expo.dev/tutorial/eas/using-github.md)
- [Next steps](https://docs.expo.dev/tutorial/eas/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Create a production build for iOS

Learn about the process of creating a production build for iOS and automating the release process.

In this chapter, we'll create our example app's production version and submit it for testing using TestFlight. After that, we'll submit them for App Store review to get it on the App Store.

[Watch: Creating and releasing a production build for iOS](https://www.youtube.com/watch?v=VZL_e0cEwo8) — Create a production build for iOS with EAS, test it using TestFlight, and submit it to the App Store.

#### Prerequisites

##### Apple Developer account

To create one, see the [Apple Developer Portal](https://developer.apple.com/account/).

##### A production build profile in eas.json

Ensure that a `production` build profile is present in your **eas.json**, which is added by default.

## Production build for iOS

A [production iOS build](/build/eas-json.md#production-builds) is optimized for Apple's App Store Connect, which allows distributing builds to testers with TestFlight and public end users through the App Store. This build type cannot be side-loaded on a simulator or device and can only be distributed through App Store Connect.

## Create a distribution provisioning profile

Run the `eas credentials` command in the terminal and then answer the following prompts by EAS CLI:

-   **Select platform** iOS.
-   **Which build profile do you want to configure?** Select production.
-   **Do you want to log in to your Apple account?** Press Y. This will log in to our Apple Developer account.
-   **What do you want to do?** Select **Build credentials** and choose **All: Set up all the required credentials to build your project**.
-   Now, it will prompt whether we want to re-use the previous Distribution Certificate. Press Y.
-   **Generate a new Apple Provisioning Profile?** Press Y. This will be the provisioning profile for the production app.
-   Once the profiles are created, press any Ctrl + C to exit the EAS CLI.

## Create a production build

To create an iOS production build using the default `production` profile, open your terminal and execute the following command. Since `production` is set as the default profile in the EAS configuration, there is no need to specify it explicitly with the `--profile` flag.

```sh
eas build --platform ios
```

The command will queue the build. Notice on the EAS dashboard that the **Build Number** is auto-incremented.

## Submit the app binary to the App Store

To submit the app binary created from our latest EAS Build, run the [`eas submit`](/submit/ios.md) command:

```sh
eas submit --platform ios
```

After running this command, we need to:

-   **Select a build from EAS.** Let's select the latest build ID.
-   **Follow the prompt to log in to our Apple account.** When it asks for **Reuse this App Store Connect API Key?** Press Y.

This will trigger the submission process.

## Release an internal testing version

After the submission process is complete, we'll need to log in to the Apple Developer account from the web browser.

-   Click **[Apps](https://appstoreconnect.apple.com/apps),** and see the app icon.
-   Click the app name, and from the navigation tab menu, click **TestFlight**. If the build was just submitted, it may take a few minutes for Apple to process the build before it is available to distribute with TestFlight.

> **Only if you have skipped [iOS development build for devices chapter](/tutorial/eas/ios-development-build-for-devices.md):** You'll be prompted **iOS app only uses standard/exempt encryption?** Press Y to select the default value provided for this prompt. Since our app doesn't use encryption, it sets `ITSAppUsesNonExemptEncryption` in the **Info.plist** file to `NO` and manages the compliance check for the same when you are releasing your app to TestFlight/Apple App Store. When you are releasing your own app, and it uses encryption, you can select `N` to skip this prompt next time.

-   In App Store Connect, under **Internal Testing**, and create a test group. This will allow us to invite test users.

-   Once the group is created, an email will be sent to all the test users.

-   In the email, click **View in TestFlight,** accept the invite, and then tap **Install**.

After that, the app will download on our device so that we can test it.

> **Note**: Similar to internal testing, we can also create a group for inviting external testers using TestFlight. Where internal testing has a limit of 100 users, TestFlight allows sharing a test release version externally with up to 10,000 testers and provides a publicly shareable link. For step-by-step instructions, see [Distribute an iOS app with TestFlight](/submit/testflight.md).

## Submit the app to the Apple App Store

To prepare our app for App Store submission, go to the **App Store** tab:

-   Provide metadata details, provide screenshots as per Apple's guidelines and also fill details under **General**.

-   Then, manually select the build.

> **Complete App Store listing**: To prepare the app for store listing, see [Create app store assets](/guides/store-assets.md) on how to create screenshots and previews.

-   Once our app is ready, click on **Submit to App Review**. After that, Apple will review our app, and if approved, the app will be available on the App Store.

## Automated submissions

For future releases, we can streamline the process by combining build creation and App Store submission into a single step by using the [`--auto-submit`](/build/automate-submissions.md) flag with `eas build`:

```sh
eas build --platform ios --auto-submit
```

> **Note:** This command will automatically upload your build to TestFlight for internal testing, but it will not automatically submit your app for App Store review. You'll still need to manually promote the build from TestFlight to the App Store when you're ready for public release. For more information, see [Default submission behavior for app stores](/build/automate-submissions.md#default-submission-behavior-for-app-stores).

## Summary

Chapter 9: Create a production build for iOS

We successfully created a production-ready iOS build, discussed distribution using TestFlight and Apple App Store using `eas submit`, and automated the release process with the `--auto-submit`.

In the next chapter, learn how to use the EAS Update to send OTA updates and share previews with our team.

[Next: Chapter 10: Share previews with your team](/tutorial/eas/team-development.md)
