---
modificationDate: July 28, 2026
title: iOS capabilities
description: Learn about built-in iOS capabilities supported in EAS Build and how to enable or disable them.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build-reference/ios-capabilities/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build-reference/ios-capabilities/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Build > Reference
Pages in this section:
- [Build lifecycle hooks](https://docs.expo.dev/build-reference/npm-hooks.md)
- [Using private npm packages](https://docs.expo.dev/build-reference/private-npm-packages.md)
- [Git submodules](https://docs.expo.dev/build-reference/git-submodules.md)
- [Using npm cache with Yarn 1 (Classic)](https://docs.expo.dev/build-reference/npm-cache-with-yarn.md)
- [Set up EAS Build with a monorepo](https://docs.expo.dev/build-reference/build-with-monorepos.md)
- [Build APKs for Android Emulators and devices](https://docs.expo.dev/build-reference/apk.md)
- [Build for iOS Simulators](https://docs.expo.dev/build-reference/simulators.md)
- [App version management](https://docs.expo.dev/build-reference/app-versions.md)
- [Troubleshoot build errors and crashes](https://docs.expo.dev/build-reference/troubleshooting.md)
- [Install app variants on the same device](https://docs.expo.dev/build-reference/variants.md)
- [iOS capabilities](https://docs.expo.dev/build-reference/ios-capabilities.md) (this page)
- [Run EAS Build locally](https://docs.expo.dev/build-reference/local-builds.md)
- [Cache dependencies](https://docs.expo.dev/build-reference/caching.md)
- [Android build process](https://docs.expo.dev/build-reference/android-builds.md)
- [iOS build process](https://docs.expo.dev/build-reference/ios-builds.md)
- [Configuration process](https://docs.expo.dev/build-reference/build-configuration.md)
- [Server infrastructure](https://docs.expo.dev/build-reference/infrastructure.md)
- [iOS App Extensions](https://docs.expo.dev/build-reference/app-extensions.md)
- [Ignore files via .easignore](https://docs.expo.dev/build-reference/easignore.md)
- [npx testflight](https://docs.expo.dev/build-reference/npx-testflight.md)
- [Repack app](https://docs.expo.dev/build-reference/repack.md)
- [Limitations](https://docs.expo.dev/build-reference/limitations.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# iOS capabilities

Learn about built-in iOS capabilities supported in EAS Build and how to enable or disable them.

When you make a change to your iOS entitlements, this change needs to be updated remotely on Apple's servers before making a production build. EAS Build automatically synchronizes capabilities on the Apple Developer Console with your local entitlements configuration when you run `eas build`. Capabilities are web services provided by Apple, think of them like AWS or Firebase services.

> This feature can be disabled with `EXPO_NO_CAPABILITY_SYNC=1 eas build`

## Entitlements

In an Expo app, the entitlements are read from the introspected app config. To edit them, see the [`ios.entitlements`](/versions/latest/config/app.md#entitlements) field in your app config file. You can see your introspected config by running `npx expo config --type introspect` in your project and then look for the `ios.entitlements` object for the results.

In an [existing React Native project](/bare/overview.md), the entitlements are read from your **ios/\*\*/\*.entitlements** file.

## Enabling

If a supported entitlement is present in the entitlements file, then running `eas build` will enable it on Apple Developer Console. If the capability is already enabled, then EAS Build will skip it.

## Disabling

If a capability is enabled for your app remotely, but not present in the native entitlements file, then running `eas build` will automatically disable it.

## Supported capabilities

EAS Build will only enable capabilities that it has built-in support for, any unsupported entitlements must be manually enabled via [Apple Developer Console](https://developer.apple.com/account/resources/identifiers/list).

| Support | Capability | Entitlement string |
| --- | --- | --- |
| ✓ | Access Wi-Fi Information | `com.apple.developer.networking.wifi-info` |
| ✓ | App Attest | `com.apple.developer.devicecheck.appattest-environment` |
| ✓ | App Groups | `com.apple.security.application-groups` |
| ✓ | Apple Pay Later Merchandising | `com.apple.developer.pay-later-merchandising` |
| ✓ | Apple Pay Payment Processing | `com.apple.developer.in-app-payments` |
| ✓ | Associated Domains | `com.apple.developer.associated-domains` |
| ✓ | AutoFill Credential Provider | `com.apple.developer.authentication-services.autofill-credential-provider` |
| ✓ | ClassKit | `com.apple.developer.ClassKit-environment` |
| ✓ | Communicates with Drivers | `com.apple.developer.driverkit.communicates-with-drivers` |
| ✓ | Communication Notifications | `com.apple.developer.usernotifications.communication` |
| ✓ | Custom Network Protocol | `com.apple.developer.networking.custom-protocol` |
| ✓ | Data Protection | `com.apple.developer.default-data-protection` |
| ✓ | DriverKit Allow Third Party UserClients | `com.apple.developer.driverkit.allow-third-party-userclients` |
| ✓ | DriverKit Family Audio (development) | `com.apple.developer.driverkit.family.audio` |
| ✓ | DriverKit Family HID Device (development) | `com.apple.developer.driverkit.family.hid.device` |
| ✓ | DriverKit Family HID EventService (development) | `com.apple.developer.driverkit.family.hid.eventservice` |
| ✓ | DriverKit Family Networking (development) | `com.apple.developer.driverkit.family.networking` |
| ✓ | DriverKit Family SCSIController (development) | `com.apple.developer.driverkit.family.scsicontroller` |
| ✓ | DriverKit Family Serial (development) | `com.apple.developer.driverkit.family.serial` |
| ✓ | DriverKit Transport HID (development) | `com.apple.developer.driverkit.transport.hid` |
| ✓ | DriverKit USB Transport (development) | `com.apple.developer.driverkit.transport.usb` |
| ✓ | DriverKit for Development | `com.apple.developer.driverkit` |
| ✓ | Extended Virtual Address Space | `com.apple.developer.kernel.extended-virtual-addressing` |
| ✓ | Family Controls | `com.apple.developer.family-controls` |
| ✓ | FileProvider TestingMode | `com.apple.developer.fileprovider.testing-mode` |
| ✓ | Fonts | `com.apple.developer.user-fonts` |
| ✓ | Group Activities | `com.apple.developer.group-session` |
| ✓ | HealthKit | `com.apple.developer.healthkit` |
| ✓ | HomeKit | `com.apple.developer.homekit` |
| ✓ | Hotspot | `com.apple.developer.networking.HotspotConfiguration` |
| ✓ | Increased Memory Limit | `com.apple.developer.kernel.increased-memory-limit` |
| ✓ | Inter-App Audio | `inter-app-audio` |
| ✓ | Journaling Suggestions | `com.apple.developer.journal.allow` |
| ✓ | Low Latency HLS | `com.apple.developer.low-latency-streaming` |
| ✓ | MDM Managed Associated Domains | `com.apple.developer.associated-domains.mdm-managed` |
| ✓ | Managed App Installation UI | `com.apple.developer.managed-app-distribution.install-ui` |
| ✓ | Maps | `com.apple.developer.maps` |
| ✓ | Matter Allow Setup Payload | `com.apple.developer.matter.allow-setup-payload` |
| ✓ | Media Device Discovery | `com.apple.developer.media-device-discovery-extension` |
| ✓ | Messages Collaboration | `com.apple.developer.shared-with-you.collaboration` |
| ✓ | Multipath | `com.apple.developer.networking.multipath` |
| ✓ | NFC Tag Reading | `com.apple.developer.nfc.readersession.formats` |
| ✓ | Network Extensions | `com.apple.developer.networking.networkextension` |
| ✓ | 5G Network Slicing | `com.apple.developer.networking.slicing.appcategory` or `com.apple.developer.networking.slicing.trafficcategory` |
| ✓ | On Demand Install Capable for App Clip Extensions | `com.apple.developer.on-demand-install-capable` |
| ✓ | Personal VPN | `com.apple.developer.networking.vpn.api` |
| ✓ | Push Notifications | `aps-environment` |
| ✓ | Push to Talk | `com.apple.developer.push-to-talk` |
| ✓ | Recalibrate Estimates | `com.apple.developer.healthkit.recalibrate-estimates` |
| ✓ | Sensitive Content Analysis | `com.apple.developer.sensitivecontentanalysis.client` |
| ✓ | Shallow Depth and Pressure | `com.apple.developer.submerged-shallow-depth-and-pressure` |
| ✓ | Shared with You | `com.apple.developer.shared-with-you` |
| ✓ | Sign In with Apple | `com.apple.developer.applesignin` |
| ✓ | SiriKit | `com.apple.developer.siri` |
| ✓ | System Extension | `com.apple.developer.system-extension.install` |
| ✓ | Tap to Pay on iPhone | `com.apple.developer.proximity-reader.payment.acceptance` |
| ✓ | Tap to Present ID on iPhone (Display Only) | `com.apple.developer.proximity-reader.identity.display` |
| ✓ | TV Services | `com.apple.developer.user-management` |
| ✓ | Time Sensitive Notifications | `com.apple.developer.usernotifications.time-sensitive` |
| ✓ | Wallet | `com.apple.developer.pass-type-identifiers` |
| ✓ | WeatherKit | `com.apple.developer.weatherkit` |
| ✓ | Wireless Accessory Configuration | `com.apple.external-accessory.wireless-configuration` |
| ✓ | iCloud | `com.apple.developer.icloud-container-identifiers` |
| ✗ | HLS Interstitial Previews | Unknown |

The unsupported capabilities either don't support iOS, or they don't have a corresponding entitlement value. Here is a list of all of the [official Apple capabilities](https://developer.apple.com/help/account/reference/supported-capabilities-ios).

### Broadcast push notifications

Apple's Broadcast option lives inside the Push Notifications capability and has no entitlement of its own. To turn it on, set [`ios.usesBroadcastPushNotifications`](/versions/latest/config/app.md#usesbroadcastpushnotifications) to `true` in your [app config](/workflow/configuration.md):

```json
{
  "expo": {
    "ios": {
      "usesBroadcastPushNotifications": true
    }
  }
}
```

EAS Build sets the Broadcast option only when it enables the Push Notifications capability, so your entitlements also need `aps-environment`. Apple limits [broadcast push notifications](https://developer.apple.com/documentation/usernotifications/setting-up-broadcast-push-notifications) to Live Activities on iOS 18 and later.

If you enable Broadcast only in [Apple Developer Console](https://developer.apple.com/account/resources/identifiers/list), the next `eas build` resets the Push Notifications capability to its default options and turns Broadcast off. Apple Push Notification service (APNs) then rejects your channel management requests with a `FeatureNotEnabled` error.

## Capability identifiers

Merchant IDs, App Groups, and CloudKit Containers can all be automatically registered and assigned to your app. These assignments require Apple cookies authentication (running locally) as the official App Store Connect API does not support these operations.

## Debugging iOS capabilities

You can run `EXPO_DEBUG=1 eas build` to get more detailed logs regarding the capability syncing.

If you have trouble using this feature, you can disable it with the environment variable `EXPO_NO_CAPABILITY_SYNC=1`.

To see all of the currently enabled capabilities, visit [Apple Developer Console](https://developer.apple.com/account/resources/identifiers/list), and find the bundle identifier matching your app, if you click on it you should see a list of all the currently enabled capabilities.

## Manual setup

There are two ways to manually enable Apple capabilities, both systems will require any existing Apple provisioning profiles to be regenerated.

### Xcode

> Preferred method for projects that do **not** use [Expo Prebuild](/more/glossary-of-terms.md#prebuild) to continuously generate the native **android** and **ios** directories.

1.  Open the **ios** directory in Xcode with `xed ios`. If you don't have an **ios** directory, run `npx expo prebuild -p ios` to generate one.
2.  Then follow the steps mentioned in [Add a capability](https://help.apple.com/xcode/mac/current/#/dev88ff319e7).

### Apple Developer Console

First step is to add the respective key/value pairs to your **ios/[app]/[app].entitlements** (or more specific entitlements file for multi-target apps). You can refer to [Supported Capabilities](/build-reference/ios-capabilities.md#supported-capabilities) to determine which entitlements keys should be added.

1.  Log into [Apple Developer Console](https://developer.apple.com/account/resources/identifiers/list). Click on "Certificates, IDs & Profiles", then navigate to "Identifiers" page.
2.  Choose the bundle identifier that matches your app's bundle identifier.
3.  Scroll down and enable a capability, some capabilities may require extra setup.
4.  Scroll to the top and press "Save". You will see a dialog that says "Modify App Capabilities", press "Confirm" to continue. You will need to regenerate any provisioning profiles that use this bundle identifier before they'll be valid for building a code signed production **.ipa**.

If adding capabilities process has not been done correctly then your iOS native build will fail with an error similar to:

```text
❌  error: Provisioning profile "*[expo] app.bacon.hello AppStore ..." doesn't support the Associated Domains capability.

❌  error: Provisioning profile "*[expo] app.bacon.hello AppStore ..." doesn't include the com.apple.developer.associated-domains entitlement.
```
