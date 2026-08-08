---
modificationDate: July 28, 2026
title: Internal distribution
description: Learn how EAS Build provides shareable URLs for your builds with your team for internal distribution.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build/internal-distribution/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build/internal-distribution/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Build
Pages in this section:
- [Introduction](https://docs.expo.dev/build/introduction.md)
- [Create your first build](https://docs.expo.dev/build/setup.md)
- [Configure with eas.json](https://docs.expo.dev/build/eas-json.md)
- [Internal distribution](https://docs.expo.dev/build/internal-distribution.md) (this page)
- [Automate submissions](https://docs.expo.dev/build/automate-submissions.md)
- [Using EAS Update](https://docs.expo.dev/build/updates.md)
- [Trigger builds from CI](https://docs.expo.dev/build/building-on-ci.md)
- [Trigger builds from GitHub App](https://docs.expo.dev/build/building-from-github.md)
- [Expo Orbit](https://docs.expo.dev/build/orbit.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Internal distribution

Learn how EAS Build provides shareable URLs for your builds with your team for internal distribution.

Setting up an internal distribution build only takes a few minutes with EAS Build and provides a streamlined way to share your app with your team and other testers for feedback. It does this by providing a URL that allows them to install the app directly to their device. If you are not sure yet if you want to use this approach and want to learn about all of the options available for distributing your app internally, refer to the [overview of distribution apps for review](/review/overview.md) guide.

## Using internal distribution

To configure a build profile for internal distribution, set `"distribution": "internal"` on it. When you set this configuration, it has the following effects on the build profile:

-   **Android**: The default behavior for the `gradleCommand` will change to generate an APK instead of an AAB. If you have specified a custom `gradleCommand`, then make sure that it [produces an APK](/build-reference/apk.md#configuring-a-profile-to-build-apks), or it won't be directly installable on an Android device. Additionally, EAS Build will generate a new Android keystore for signing the APK, or it will use an existing one if the package name is the same as your [development build](/develop/development-builds/introduction.md).
-   **iOS**: Builds using this profile will use either [ad hoc or enterprise provisioning](/build/internal-distribution.md#overview-of-distribution-mechanisms). When using ad hoc provisioning, EAS Build will generate a provisioning profile containing an allow-list of device UDIDs, and only those devices in the list at build time will be able to install it. You can add a device by running `eas device:create` and creating a new build.
-   By default, internal distribution build URLs are available to anybody with the URL, and each is identified by a 32 character UUID. If you would like to require sign-in to an authorized Expo account to access these builds, you can disable the **Unauthenticated access to internal builds** option in your [project settings](https://expo.dev/accounts/%5Baccount%5D/projects/%5Bproject%5D/settings).

See the tutorial on Internal distribution with EAS Build below for more information on how to configure, create, and install a build:

[Create and share internal distribution build](/tutorial/eas/internal-distribution-builds.md) — Complete step-by-step guide to setting up and sharing internal distribution builds with EAS Build.

### Automation on CI (optional)

You can run internal distribution builds non-interactively in CI with the [`--non-interactive`](/eas/cli.md#eas-build) flag. [Learn more about triggering builds from CI](/build/building-on-ci.md).

For iOS ad hoc builds, `eas build --non-interactive` reuses a valid provisioning profile without updating its device list. The build can succeed, but the app may not install on registered devices added after the profile was last updated.

Pass `--refresh-ad-hoc-provisioning-profile` with `--non-interactive` to update the Expo-managed ad hoc provisioning profile on the Apple Developer Portal before the build.

> `--refresh-ad-hoc-provisioning-profile` requires EAS CLI 19.1.0 or later.

EAS authenticates with an App Store Connect API key. It reads devices registered on EAS for your Apple team. It registers any missing UDIDs on the portal. Then it refreshes the profile device list.

When you use this flag, EAS selects all matching devices for the build target's Apple platform: iPhone and iPad for iOS, Mac for macOS.

```sh
eas build --platform ios --profile preview --non-interactive --refresh-ad-hoc-provisioning-profile
```

For [EAS Workflows](/eas/workflows/get-started.md), set `refresh_ad_hoc_provisioning_profile: true` in the build job's `params` with the same profile requirements. See the [build job parameters](/eas/workflows/pre-packaged-jobs.md#build).

The profile must set [`"distribution": "internal"`](/eas/json.md#distribution) and use [credentials managed by EAS](/app-signing/app-credentials.md). You need at least one device from [`eas device:create`](/eas/cli.md#eas-device-create) and an App Store Connect API key in CI through [environment variables](/build/building-on-ci.md#optional-provide-an-asc-api-token-for-your-apple-team) (`EXPO_ASC_API_KEY_PATH`, `EXPO_ASC_KEY_ID`, and `EXPO_ASC_ISSUER_ID`) or a key stored in EAS for submissions on the project.

Otherwise, after registering a device with [`eas device:create`](/eas/cli.md#eas-device-create), run [`eas build`](/eas/cli.md#eas-build) interactively and sign in with your Apple account so EAS can update the ad hoc provisioning profile.

### Managing devices

With [EAS Workflows](/eas/workflows/get-started.md), you can pause a workflow until a tester enrolls an iOS device and a team member approves it using the [`apple-device-registration-request`](/eas/workflows/pre-packaged-jobs.md#apple-device-registration-request) job. Pair it with a `build` job that sets `refresh_ad_hoc_provisioning_profile: true` to include the new device in an internal distribution build.

You can see any devices registered via `eas device:create` by running:

```sh
eas device:list
```

Devices registered with Expo for ad hoc provisioning will appear on your Apple Developer Portal after they are used to generate a provisioning profile for a new internal build with EAS Build or to [re-sign an existing build](/app-signing/app-credentials.md#re-signing-new-credentials) with `eas build:resign`.

> **On new or recently renewed Apple Developer Program memberships, a newly registered device may not be immediately installable.** Registering a device with Expo does not register it with Apple. The device is added to your Apple Developer Portal only when it's first included in a provisioning profile, which happens when you create a new internal build or re-sign an existing one. For these memberships, Apple can take [up to 24–72 hours](https://developer.apple.com/help/account/reference/device-registration-updates/) to finish processing a newly registered device, and during that time the device can't be added to provisioning profiles. As a result, the first build or re-sign that includes a new device may fail. Wait for Apple to finish processing, then create a new build or re-sign again to produce an installable build that includes the device.

#### Remove devices

If a device is no longer in use, it can be removed from this list by running:

```sh
eas device:delete
```

This command will also prompt you to disable the device on the Apple Developer Portal. Disabled devices still count against [Apple's limit of 100 devices](https://developer.apple.com/support/account/#:~:text=Resetting%20your%20device%20list%20annually) for ad hoc distribution per app.

#### Rename devices

Devices added via the website URL/QR code will default to displaying their UDID when selecting them for an EAS Build. You can assign friendly names to your devices with the following command:

```sh
eas device:rename
```

## Overview of distribution mechanisms

The following are the different mechanisms for distributing your app to devices supported by internal distribution.

#### Android: Build and distribute an APK

To share your app to Android devices, you must build an APK (Android application package file) of your project. APKs can be installed directly to an Android device over USB, by downloading the file over the web or through an email or chat app, once the user accepts the security warning for installing an app that has not gone through Play Store review. AAB (Android app bundle) binaries of your app must be distributed through the Play Store.

#### iOS: Ad Hoc distribution

Apple offers [ad hoc provisioning profiles](https://help.apple.com/xcode/mac/current/#/dev7ccaf4d3c) to distribute your app to test devices once they have been registered to your Apple Developer account. This method requires a paid Apple Developer account and that account will only be able to use this method to distribute to at most 100 iPhones per year.

You will need to know the UDID (Unique Device Identifier) of each device that will install your app, which may be challenging if you try to share with someone who is not a developer. Adding a new device will require a rebuild of your app or [re-signing the build with new credentials](/app-signing/app-credentials.md#re-signing-new-credentials).

Setting up Ad Hoc certificates correctly can be intimidating if you haven't done it before and tedious even if you have. If you're using [EAS Build](/build/internal-distribution.md#using-internal-distribution), which is optimized for Expo and React Native projects, we'll handle the time-consuming parts of setting up Ad Hoc credentials for you.

#### iOS: Enterprise distribution

If your app is only intended for internal use by employees of a large organization and cannot be distributed through the App Store, you should use Enterprise distribution. Unlike with Ad Hoc Distribution, the number of devices that can install your app is unlimited, and you do not need to manage each device's UDID. Often these apps will be distributed to end users through a mobile device management (MDM) solution. Enterprise Distribution requires membership in the [Apple Developer Enterprise Program](https://developer.apple.com/programs/enterprise/). Organizations joining the Enterprise Program must meet additional requirements beyond what is required for App Store distribution.
