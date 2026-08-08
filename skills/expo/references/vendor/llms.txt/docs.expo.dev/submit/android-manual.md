---
modificationDate: July 17, 2026
title: Manually submit an Android app to the Google Play Store
description: Step-by-step guide for uploading an Android app to Google Play Console for the first time.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/submit/android-manual/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/submit/android-manual/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Submit
Pages in this section:
- [Submit to Google Play Store](https://docs.expo.dev/submit/android.md)
- [Submit to Apple App Store](https://docs.expo.dev/submit/ios.md)
- [TestFlight](https://docs.expo.dev/submit/testflight.md)
- [Manual Android submission](https://docs.expo.dev/submit/android-manual.md) (this page)
- [Manual iOS submission](https://docs.expo.dev/submit/ios-manual.md)
- [Configure with eas.json](https://docs.expo.dev/submit/eas-json.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Manually submit an Android app to the Google Play Store

Step-by-step guide for uploading an Android app to Google Play Console for the first time.

If you are submitting your Android app to the Google Play Store **for the first time**, this guide walks through the steps to upload it manually and create your first release in Google Play Console, a web UI.

> **A manual upload is optional**. The [`eas submit`](/submit/android.md) command can create your app's first release directly and by default on the internal testing track. Use this guide if you prefer to create the first release yourself in Play Console.

> The Google Play Console dashboard changes over time. If a screenshot or label in this guide does not match what you see, [open an issue on GitHub](https://github.com/expo/expo/issues/new/choose) to let us know.

## Steps to create your first release

Open [Google Play Console](https://play.google.com/apps/publish/) and click **Create app**.

Enter **App name**, select **Default language**, **App or game**, **Free or paid**, and click **Create app**.

You will be redirected to the **Dashboard**, where you provide all the information about your Android app.

Go through the steps to fill out the app details. Start by preparing your app's internal testing version. Click **View tasks** under **Start testing now**.

> This step is important. Otherwise, you will see errors related to the app information when you try to publish.

On the **Internal testing** page, under **Testers**, click **Create email list** and add users to share the internal test release with. After creating (or selecting an existing) testers list, click **Save**.

Click **Create new release**.

You will be redirected to the App integrity screen. Select **Choose signing key** > **Google-generated key**. Using a Google-generated key lets you upload your app even if you lose your Android keystore.

Under **App bundles**, click **Upload** and choose the **.aab** file from your computer. If you haven't created your build yet, [create one with `eas build`](/tutorial/eas/android-production-build.md#create-a-production-build).

Once the upload completes, you will see the archive type and the **Version code**. The **Version code** identifies each release. Every new release must have a unique value. In an Expo project, set this with `expo.android.versionCode` in your app config, or use [remote version source](/build-reference/app-versions.md) to increment it automatically.

Enter the **Release name** and click **Next**. On the **Preview and confirm** screen, you may see a warning about _"... no deobfuscation file associated with this App Bundle ..."_. You can skip this for now, or [enable ProGuard rules with `expo-build-properties`](/versions/latest/sdk/build-properties.md#pluginconfigtypeandroid). Click **Save and publish**.

You will land on the **Internal testing** > **Releases** summary. Click **Promote release** to make your app available to testers, or to promote it to production.

To share the release with the testers list you created, click **Testers** > **Copy link** and share the link. Testers can install the app on their device from that link.

## Next steps

Go back to the **Dashboard** and complete the remaining tasks for your app: privacy policy, store assets, and other details. This is required before the app can go to production.

### Further reading

-   [Missing privacy policy](https://expo.fyi/missing-privacy-policy) error in Play Console
-   [Create store assets](/guides/store-assets.md) for your app's store page
-   [Creating a Google Service Account key](https://expo.fyi/creating-google-service-account): required to use `eas submit --platform android`
-   [Submit to the Google Play Store with EAS Submit](/submit/android.md)
