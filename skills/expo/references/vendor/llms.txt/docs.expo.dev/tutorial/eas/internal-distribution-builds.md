---
modificationDate: July 28, 2026
title: Create and share internal distribution build
description: Learn about internal distribution builds, why we need them, and how to create them.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/eas/internal-distribution-builds/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/eas/internal-distribution-builds/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Internal distribution build](https://docs.expo.dev/tutorial/eas/internal-distribution-builds.md) (this page)
- [Manage app versions](https://docs.expo.dev/tutorial/eas/manage-app-versions.md)
- [Android production build](https://docs.expo.dev/tutorial/eas/android-production-build.md)
- [iOS production build](https://docs.expo.dev/tutorial/eas/ios-production-build.md)
- [Share previews](https://docs.expo.dev/tutorial/eas/team-development.md)
- [Builds from GitHub](https://docs.expo.dev/tutorial/eas/using-github.md)
- [Next steps](https://docs.expo.dev/tutorial/eas/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Create and share internal distribution build

Learn about internal distribution builds, why we need them, and how to create them.

In this chapter, we'll learn how to set up [internal distribution builds](/build/internal-distribution.md#using-internal-distribution).

[Watch: How to create and share an internal distribution build](https://www.youtube.com/watch?v=1fQuGLHxWks) — Create an internal distribution build with EAS and share it directly with your team for testing.

## Internal distribution build

Internal distribution builds are ideal for sharing updates with team members, allowing both technical and non-technical stakeholders to provide feedback directly. Unlike development builds, these do not require running a development server, simplifying the testing process.

### Ways to distribute an app internally

Both Google and Apple provide built-in mechanisms for sharing apps internally:

-   **Android**: Using Google Play beta
-   **iOS**: Using TestFlight

However, both of these traditional methods have their limitations. For example, TestFlight limits to one active build at a time.

### EAS Build for faster distribution

EAS Build speeds up the process. It creates shareable links for our builds and provides instructions on using them. It has a default configuration designed to facilitate internal distribution, offering a more efficient alternative to traditional methods.

## Create an internal distribution build

To create and distribute a build with EAS Build, we need to follow these steps:

### Configure preview build profile

From our initial setup in **eas.json**, we already have a default configuration that includes a `preview` build profile designed for internal distribution:

```json
{
  "build": {
    "preview": {
      "distribution": "internal"
    }
  }
}
```

This is all we need to create our first internal distribution build. The `preview` build profile from the above snippet has a `distribution` property whose value is set to `internal`. This value allows us to share our build URLs with anyone so they can install it on their device and do not require a development server to run the app.

As discussed in the previous chapters, for non-app store builds, Android requires **.apk** and iOS needs **.ipa** formats. This applies to internal distribution builds as well. The `distribution` when set to `internal`, automatically creates the app binary in these file formats for devices.

### Create

Creating an internal distribution build requires [app signing credentials](/app-signing/app-credentials.md).

Android app signing is non-restrictive and allows installing any compatible **.apk** file. When a development build was created, a new Android Keystore was generated for it. Hence, there is no need to generate a new keystore for preview builds.

On the other hand, Apple has stricter rules for app distribution on iOS devices. We need an ad hoc provisioning profile that explicitly lists the devices allowed to run the app. Some organizations whose apps meet specific requirements may be able to use the [Apple Developer Enterprise Program](https://developer.apple.com/programs/enterprise/) to distribute apps internally to a larger audience.

#### Android

-   Use the `preview` profile to initiate an Android build:

```sh
eas build --platform android --profile preview
```

-   This command triggers the EAS Build, and on the EAS dashboard, we can see the build's progress:

#### iOS

Apps signed with an ad hoc provisioning profile can be installed by an iOS device whose UDID is registered with the provisioning profile.

-   To register more devices, use `eas device:create`. This command registers an iOS device and gives us a URL or QR code to share for device registration:

```sh
eas device:create
```

-   This command registers an iOS device for app installation, generating a shareable URL (or QR code) for device registration.
    
    > **Tip**: This command enables device registration at any time. However, only builds created post-registration will work on the newly added device.
    
    > **On a new or recently renewed Apple Developer Program membership, a newly registered device may not be immediately installable.** Registering a device with Expo does not register it with Apple. The device is added to Apple only when it's first included in a build or re-sign. For such a membership, Apple can then take [up to 24–72 hours](https://developer.apple.com/help/account/reference/device-registration-updates/) to finish processing the device before it can be added to a provisioning profile. If a build fails right after you register a new device, wait for Apple to finish processing and then run the build again. Learn more about [managing devices](/build/internal-distribution.md#managing-devices).
    
-   To create the preview build, we need to use the `preview` profile with the `eas build` command:
    

```sh
eas build --platform ios --profile preview
```

-   This command triggers the EAS Build, and on the EAS dashboard, we can see the build's progress:

#### Alternative method to register devices using eas build:resign

[`eas build:resign`](/app-signing/app-credentials.md#re-signing-new-credentials) command can be used to re-sign an existing iOS **.ipa** with a new ad hoc provisioning profile, eliminating the need for a full rebuild.

#### Are you setting up enterprise provisioning?

Apple Enterprise Program membership costs $299 USD per year and [not all organizations will be eligible](https://developer.apple.com/programs/enterprise/), so you will likely be using ad hoc provisioning, which works with any normal paid Apple Developer account.

If you have an [Apple Developer Enterprise Program membership](https://developer.apple.com/programs/enterprise/) users can install your app to their device without pre-registering their UDID. They just need to install the profile to their device and they can then access existing builds. You will need to sign in using your Apple Developer Enterprise account during the `eas build` process to set up the correct provisioning.

If you distribute your app both through enterprise provisioning and the App Store, you will need to have a distinct bundle identifier for each context. We recommend either:

-   In projects generated with Expo CLI, use [**app.config.js** to dynamically switch identifiers](/tutorial/eas/multiple-app-variants.md).
-   In [existing React Native projects](/bare/overview.md), create a separate `scheme` for each bundle identifier and specify the scheme name in separate build profiles.

#### Are you using manual local credentials?

If so, make sure to point your **credentials.json** to an ad hoc or enterprise provisioning profile that you generate through the Apple Developer Portal (either update an existing **credentials.json** used for another type of distribution or replace it with a new one that points to the appropriate provisioning profile). Beware that EAS CLI does only a limited validation of your local credentials, and you will have to handle device UDID registration manually. Read more about [using local credentials](/app-signing/local-credentials.md).

### Install

Once the build finishes, the Build artifact section gets updated, indicating that the build is complete. This section provides the methods available for running the development build on an iOS device: Expo Orbit and Install button.

-   Open the build's detail page. If you are sharing the build with someone else, you can send them the link to the build. They'll be able to open the build's detail page or build artifact details which include Expo Orbit.
-   Connect the Android or iOS device to your machine using USB.
-   Open the Orbit menu bar app.
-   Select the **Device** in the Orbit app.
-   Under **Build artifact**, click the **Open with Orbit**.

#### Alternate: Use Install and QR code

-   Open the build's detail page. If you are sharing the build with someone else, you can send them the link to the build page. They'll be able to open it and see build artifact details which includes Expo Orbit.
-   Click **Install** under the Build artifact section to display the **Install on a test device** popup.
-   Copy the link from **Send a link to a device** section and send it to the test device.

### Run

Tap the app icon on your device to start the preview build. There is no need for a development server.

Since we have already set up multiple app variants, we can see both the development and preview variants installed separately on our devices. For example:

-   On Android:

-   On iOS:

## Summary

Chapter 6: Create and share internal distribution build

We successfully created internal distribution builds for Android and iOS, used ad hoc provisioning for iOS, and installed multiple app variants on the same device.

In the next chapter, learn about developer-facing and user-facing app versions and how to manage them automatically.

[Next: Chapter 7: Manage different app versions](/tutorial/eas/manage-app-versions.md)
