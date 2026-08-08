---
modificationDate: July 21, 2026
title: Manually submit an iOS app to the Apple App Store
description: Archive and upload an iOS app to App Store Connect using Xcode on macOS.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/submit/ios-manual/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/submit/ios-manual/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Submit
Pages in this section:
- [Submit to Google Play Store](https://docs.expo.dev/submit/android.md)
- [Submit to Apple App Store](https://docs.expo.dev/submit/ios.md)
- [TestFlight](https://docs.expo.dev/submit/testflight.md)
- [Manual Android submission](https://docs.expo.dev/submit/android-manual.md)
- [Manual iOS submission](https://docs.expo.dev/submit/ios-manual.md) (this page)
- [Configure with eas.json](https://docs.expo.dev/submit/eas-json.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Manually submit an iOS app to the Apple App Store

Archive and upload an iOS app to App Store Connect using Xcode on macOS.

This guide walks through archiving an iOS app with Xcode and uploading it to [App Store Connect](https://appstoreconnect.apple.com/), which is useful if you aren't using EAS, or as a fallback if EAS Submit is temporarily unavailable.

If you already have a **.ipa** file, skip to [Upload an existing build with Transporter](/submit/ios-manual.md#upload-an-existing-build-with-transporter). For most teams, [EAS Submit](/submit/ios.md) is simpler and works from any OS.

## Prerequisites

#### Prerequisites

##### Sign up for an Apple Developer account

A paid Apple Developer membership is required to submit apps to the Apple App Store. Sign up on the [Apple Developer Portal](https://developer.apple.com/account/).

##### Install Xcode

Xcode must be installed on a Mac. See [Set up Xcode](/get-started/set-up-your-environment.md?platform=ios&device=physical&mode=development-build&buildEnv=local#set-up-xcode-and-watchman) for instructions.

##### Generate the ios directory

Your project needs an **ios** directory. If you use [CNG](/workflow/continuous-native-generation.md), run `npx expo prebuild` to generate it.

##### Create an app record in App Store Connect

An app record in [App Store Connect](https://appstoreconnect.apple.com/) with a matching bundle identifier is required. If you haven't created one, sign in to App Store Connect, click the blue **+** next to **Apps**, and choose **New App**.

## Open the iOS workspace in Xcode

From your project directory, open the iOS workspace in Xcode:

```sh
xed ios
```

Then:

1.  In the left sidebar, select your app's workspace.
2.  Go to **Signing & Capabilities** and select **All** or **Release**.
3.  Under **Signing** > **Team**, choose your Apple Developer team. Xcode will generate an automatically managed provisioning profile and signing certificate.

## Configure a release scheme

1.  From the menu bar, open **Product** > **Scheme** > **Edit Scheme**.
2.  Select **Run** in the sidebar and set **Build configuration** to **Release**.

## Build the app for release

From the menu bar, open **Product** > **Build**. Xcode will build the release binary.

## Archive and upload to App Store Connect

1.  From the menu bar, open **Product** > **Archive**.
2.  Under **Archives**, click **Distribute App** from the right sidebar.
3.  Click **App Store Connect** and follow the prompts. Xcode will upload your build to App Store Connect. To get a **.ipa** file instead and upload it later, choose **Export** in the prompts and use [Transporter](/submit/ios-manual.md#upload-an-existing-build-with-transporter).
4.  Sign in to [App Store Connect](https://appstoreconnect.apple.com/), select your app, and submit it to TestFlight or for App Review from the App Store Connect dashboard.

## Upload an existing build with Transporter

If you already have a **.ipa** file, for example from running `eas build --platform ios --local` or downloading a build from the [EAS dashboard](https://expo.dev/accounts/%5Baccount%5D/projects/%5Bproject%5D/builds), you don't need to archive the app in Xcode. Upload it with Apple's Transporter app instead:

1.  Download [**Transporter** from the App Store](https://apps.apple.com/app/transporter/id1450874784).
2.  Sign in with your Apple ID.
3.  Add the build either by dragging the **.ipa** file directly into the Transporter window or by selecting it from the file dialog opened with **+** or **Add App** button.
4.  Submit it by clicking the **Deliver** button.

This process can take a few minutes, then another 10-15 minutes of processing on Apple's servers. Afterward, you can check the status of your binary in App Store Connect and submit it to TestFlight or for App Review.

## Further reading

-   [Create store assets](/guides/store-assets.md) for your app's store page
-   [Submit to the Apple App Store with EAS Submit](/submit/ios.md)
