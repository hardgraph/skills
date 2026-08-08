---
modificationDate: July 16, 2026
title: Manage different app versions
description: Learn about developer-facing and user-facing app versions and how EAS Build automatically manages developer-facing versions.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/eas/manage-app-versions/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/eas/manage-app-versions/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Manage app versions](https://docs.expo.dev/tutorial/eas/manage-app-versions.md) (this page)
- [Android production build](https://docs.expo.dev/tutorial/eas/android-production-build.md)
- [iOS production build](https://docs.expo.dev/tutorial/eas/ios-production-build.md)
- [Share previews](https://docs.expo.dev/tutorial/eas/team-development.md)
- [Builds from GitHub](https://docs.expo.dev/tutorial/eas/using-github.md)
- [Next steps](https://docs.expo.dev/tutorial/eas/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Manage different app versions

Learn about developer-facing and user-facing app versions and how EAS Build automatically manages developer-facing versions.

In this chapter, we'll learn how EAS Build automatically manages the developer-facing app version for Android and iOS. Learning about it will be useful before we dive into production build in the next two chapters.

[Watch: Automating app version code](https://www.youtube.com/watch?v=C8x4N9UmzS8) — Understand developer-facing and user-facing app versions and how EAS Build automates version management for you.

## Understanding developer-facing and user-facing app versions

An app version is composed of two values:

-   Developer-facing value: Represented by [`versionCode`](/versions/latest/config/app.md#versioncode) for Android and [`buildNumber`](/versions/latest/config/app.md#buildnumber) for iOS.
-   User-facing value: Represented by [`version`](/versions/latest/config/app.md#version) **app.config.js**.

Both Google Play Store and Apple App Store rely on developer-facing values to identify each unique build. For example, if we upload an app with the app version `1.0.0 (1)` (which is a combination of user-facing and developer-facing values), we cannot submit another build to the app stores with the same app version. Submitting builds with duplicate app version numbers results in a failed submission.

An example demonstration of manually managing developer-facing values is shown below by `android.versionCode` and `ios.buildNumber` in **app.config.js**. **We don't have to add or manage these values manually since EAS Build automates this for us**.

```js
{
  ios: {
    buildNumber: 1
    ... 
  },
  android: {
    versionCode: 1
  }
  ... 
}
```

> **Note**: The [user-facing version number](/build-reference/app-versions.md#user-facing-version) is not handled by EAS. Instead, we define that in the app store developer portals before submitting our production app for review.

## Automatic app version management with EAS Build

By default, EAS Build assists in automating developer-facing values. It utilizes the [remote version source](/build-reference/app-versions.md#remote-version-source) to automatically increment developer-facing values whenever a new production release is made.

When we initialized the project with `eas init` command, the EAS CLI automatically added the following properties in **eas.json**:

-   `cli.appVersionSource` which is set to `remote`
-   [`build.production.autoIncrement`](/eas/json.md#autoincrement-1) which is set to `true`

You can view them in your project's **eas.json**:

```json
{
  "cli": {
    ... 
    "appVersionSource": "remote"
  },
  "build": {
    "production": {
      "autoIncrement": true
    }
  }
  ... 
}
```

When we create a new production build in the next two chapters, the `versionCode` for Android and `buildNumber` for iOS will increment automatically.

#### Syncing developer-facing app versions for already published apps to EAS

If your app is already published in the app stores, the developer-facing app versions are already set. When migrating this app to use EAS Build, follow the steps below to sync those app versions:

-   In the terminal window, run the `eas build:version:set` command:

```sh
eas build:version:set
```

-   Select the platform (Android or iOS) when prompted.
-   When prompted **Do you want to set app version source to remote now?**, select **yes**. This will set the `cli.appVersionSource` to `remote` in **eas.json**.
-   When prompted **What version would you like to initialize it with?**, enter the last version number that you have set in the app stores.

After these steps, the app versions will be synced to EAS Build remotely. You can set `build.production.autoIncrement` to `true` in **eas.json**. When you create a new production build, the `versionCode` and `buildNumber` will be automatically incremented from now on.

## Summary

Chapter 7: Manage different app versions

We successfully explored app versioning differences, addressed the importance of unique app versions to prevent store rejections, and enabled automated version updates in **eas.json** for production builds.

In the next chapter, learn about the process of creating a production build for Android.

[Next: Chapter 8: Create a production build for Android](/tutorial/eas/android-production-build.md)
