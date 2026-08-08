---
modificationDate: July 28, 2026
title: Permissions
description: Learn about configuring and adding permissions in an app config file.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/permissions/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/permissions/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process
Pages in this section:
- [Develop an app with Expo](https://docs.expo.dev/workflow/overview.md)
- [Configure with app config](https://docs.expo.dev/workflow/configuration.md)
- [Continuous Native Generation](https://docs.expo.dev/workflow/continuous-native-generation.md)
- [Using libraries](https://docs.expo.dev/workflow/using-libraries.md)
- [Privacy manifests](https://docs.expo.dev/guides/apple-privacy.md)
- [Permissions](https://docs.expo.dev/guides/permissions.md) (this page)
- [Environment variables](https://docs.expo.dev/guides/environment-variables.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Permissions

Learn about configuring and adding permissions in an app config file.

When developing a native app that requires access to potentially sensitive information on a user's device, such as their location or contacts, the app must request the user's permission first. For example, to access the user's media library, the app will need to run [`MediaLibrary.requestPermissionsAsync()`](/versions/latest/sdk/media-library.md#medialibraryrequestpermissionsasyncwriteonly-granularpermissions).

Permissions in standalone and [development builds](/develop/development-builds/introduction.md) require native build-time configuration before they can be requested using runtime JavaScript code. This is not required when testing projects in the [Expo Go](https://expo.dev/go) app.

> If you don't configure or explain the native permissions properly **it may result in your app getting rejected or pulled from the stores**.

## Android

Permissions are configured with the [`android.permissions`](/versions/latest/config/app.md#permissions) and [`android.blockedPermissions`](/versions/latest/config/app.md#blockedpermissions) keys in your [app config](/workflow/configuration.md).

Most permissions are added automatically by libraries that you use in your app either with [config plugins](/config-plugins/plugins.md#creating-a-config-plugin) or with a package-level **AndroidManifest.xml**. You only need to use `android.permissions` to add additional permissions that are not included by default in a library.

```json
{
  "android": {
    "permissions": ["android.permission.SCHEDULE_EXACT_ALARM"]
  }
}
```

The only way to remove permissions that are added by package-level **AndroidManifest.xml** files is to block them with the [`android.blockedPermissions`](/versions/latest/config/app.md#blockedpermissions) property. To do this, specify the **full permission name**. For example, if you want to remove the audio recording permissions added by `expo-camera`:

```json
{
  "android": {
    "blockedPermissions": ["android.permission.RECORD_AUDIO"]
  }
}
```

-   See [`android.permissions`](/versions/latest/config/app.md#permissions) to learn about which permissions are included in the default [prebuild template](/workflow/continuous-native-generation.md#templates).
-   Apps using _dangerous_ or _signature_ permissions without valid reasons **may be rejected by Google**. Ensure you follow the [Android permissions best practices](https://developer.android.com/training/permissions/usage-notes) when submitting your app.
-   [All available Android `Manifest.permissions`](https://developer.android.com/reference/android/Manifest.permission).

#### Are you using this library in an existing React Native app?

Modify **AndroidManifest.xml** to exclude specific permissions: add the `tools:node="remove"` attribute to a `<use-permission>` tag to ensure it is removed, even if it's included in a library's **AndroidManifest.xml**.

```xml
<manifest xmlns:tools="http://schemas.android.com/tools">
  <uses-permission tools:node="remove" android:name="android.permission.ACCESS_FINE_LOCATION" />
</manifest>
```

> You have to define the `xmlns:tools` attribute on `<manifest>` before you can use the `tools:node` attribute on permissions.

## iOS

Your iOS app can ask for system permissions from the user. For example, to use the device's camera or access photos, Apple requires an explanation for how your app makes use of that data. Most packages will automatically provide a boilerplate reason for a given permission with [config plugins](/config-plugins/introduction.md). These default messages will most likely need to be tailored to your specific use case for your app to be accepted by the App Store.

To set permission messages, use the [`ios.infoPlist`](/versions/latest/config/app.md#infoplist) key in your [app config](/workflow/configuration.md), for example:

```json
{
  "ios": {
    "infoPlist": {
      "NSCameraUsageDescription": "This app uses the camera to scan barcodes on event tickets."
    }
  }
}
```

Many of these properties are also directly configurable using the [config plugin](/config-plugins/introduction.md) properties associated with the library that adds them. For example, with [`expo-media-library`](/versions/latest/sdk/media-library.md) you can configure photo permission messages like this:

```json
{
  "plugins": [
    [
      "expo-media-library",
      {
        "photosPermission": "Allow $(PRODUCT_NAME) to access your photos.",
        "savePhotosPermission": "Allow $(PRODUCT_NAME) to save photos."
      }
    ]
  ]
}
```

-   Changes to the **Info.plist** cannot be updated over-the-air, they will only be deployed when you submit a new native binary. For example, with [`eas build`](/build/introduction.md).
-   Apple's official [permission message recommendations](https://developer.apple.com/design/human-interface-guidelines/privacy#Requesting-permission).
-   [All available **Info.plist** properties](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html).

#### Are you using this library in an existing React Native app?

Add and modify the permission message values in **Info.plist** file directly. We recommend doing this directly in Xcode for autocompletion.

## Web

On the web, permissions like the `Camera` and `Location` can only be requested from a [secure context](https://developer.mozilla.org/en-US/docs/Web/Security/Secure_Contexts#When_is_a_context_considered_secure). For example, using `https://` or `http://localhost`. This limitation is similar to Android's manifest permissions and iOS's **Info.plist** usage messages and is enforced to increase privacy.

## Resetting permissions

Often you want to be able to test what happens when a user rejects permissions, to ensure your app reacts gracefully. An operating-system level restriction on both Android and iOS prohibits an app from asking for the same permission more than once (you can imagine how this could be annoying for the user to be repeatedly prompted for permissions after rejecting them). To test different flows involving permissions in development, you may need to uninstall and reinstall the native app.

When testing in [Expo Go](https://expo.dev/go), you can delete the app and reinstall it by running `npx expo start` and pressing I or A in the [Expo CLI](/more/expo-cli.md) Terminal UI.
