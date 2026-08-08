---
modificationDate: July 17, 2026
title: Create a release build locally
description: Learn how to create a release (production) build for your Expo app locally.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/local-app-production/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/local-app-production/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Build locally
Pages in this section:
- [Overview](https://docs.expo.dev/guides/local-app-overview.md)
- [Development](https://docs.expo.dev/guides/local-app-development.md)
- [Release](https://docs.expo.dev/guides/local-app-production.md) (this page)
- [Cache builds remotely](https://docs.expo.dev/guides/cache-builds-remotely.md)
- [Precompiled Expo Modules](https://docs.expo.dev/guides/prebuilt-expo-modules.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Create a release build locally

Learn how to create a release (production) build for your Expo app locally.

To create your app's release build (also known as production build) locally, you need to follow separate steps on your computer and use the tools required to create any native app. This guide provides the necessary steps for Android and iOS.

## Android

Creating a release build locally for Android requires signing it with an [upload key](https://developer.android.com/studio/publish/app-signing#certificates-keystores) and generating an Android Application Bundle (**.aab**). Follow the steps below:

#### Prerequisites

##### OpenJDK installed

Install an [OpenJDK distribution](/get-started/set-up-your-environment.md?mode=development-build&buildEnv=local#install-jdk) to access the `keytool` command.

##### android directory generated

If you are using [Continuous Native Generation (CNG)](/workflow/continuous-native-generation.md), run `npx expo prebuild` to generate it.

### Create an upload key

#### Already created a build with EAS Build? Download your credentials and skip to the next step.

If you've already created a build with EAS Build, follow the steps below to download the credentials, which contains the upload key and its password, key alias, and key password:

1.  In your terminal, run `eas credentials -p android` and select the build profile.
2.  Select **credentials.json** > **Download credentials from EAS to credentials.json**.
3.  Move the downloaded **keystore.jks** file to the **android/app** directory.
4.  Copy the values for the upload keystore password, key alias, and key password from the **credentials.json** as you will need them in the next step.

Inside your Expo project directory, run the following `keytool` command to create an upload key:

```sh
sudo keytool -genkey -v -keystore my-upload-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```

After running this command, you will be prompted to enter a password for the keystore. This password will protect the upload key. Remember the password you enter here, as you'll need it in the next step.

This command also generates the keystore file named **my-upload-key.keystore** in your project directory. Move it to the **android/app** directory.

> If you commit the **android** directory to a version control system like Git, don't commit this keystore file. It contains your upload key and should be kept private.

### Update gradle variables

Open **android/gradle.properties** file and add the following gradle variables at the end of the file. Replace the `*****` with the correct keystore and key password that you provided in the previous step.

These variables contain information about your upload key:

```ruby
# If you've downloaded the credentials from `eas credentials` command, see comments below for each value.

# Path to the "keystore" file
MYAPP_UPLOAD_STORE_FILE=my-upload-key.keystore
# Replace with value of the `keystore.keyAlias` field in the credentials.json file
MYAPP_UPLOAD_KEY_ALIAS=my-key-alias
# Replace with value of the `keystore.password` field in the credentials.json file
MYAPP_UPLOAD_STORE_PASSWORD=*****
# Replace with value of the `keystore.keyPassword` field in the credentials.json file
MYAPP_UPLOAD_KEY_PASSWORD=*****
```

> If you commit the **android** directory to a version control system like Git, don't commit the above information. Instead, create a **~/.gradle/gradle.properties** file on your computer and add the above variables to this file.

### Add signing config to build.gradle

Open **android/app/build.gradle** file and add the following configuration:

### Generate release Android Application Bundle (aab)

Navigate inside the **android** directory and create a release build in **.aab** format by running Gradle's `bundleRelease` command:

```sh
cd android
./gradlew app:bundleRelease
```

This command will generate app-release.aab inside the **android/app/build/outputs/bundle/release** directory.

### App submission to Google Play Console

To submit the **.aab** file, upload it manually through Google Play Console. Alternatively, [EAS Submit](/deploy/submit-to-app-stores.md#can-i-submit-builds-that-were-not) accepts locally built binaries with `eas submit --platform android --path ./my-app.aab`.

[Manual submission of an Android app](/submit/android-manual.md) — Follow the step-by-step guide on manually submitting your app to Google Play Store for the first time.

## iOS

To create an iOS release build locally, you use Xcode, which handles signing and uploading to App Store Connect. Follow the manual submission guide for the complete walkthrough, from configuring a release scheme to archiving and uploading your app:

[Manually submit an iOS app with Xcode](/submit/ios-manual.md) — Configure signing, archive your app with Xcode, and upload it to App Store Connect.
