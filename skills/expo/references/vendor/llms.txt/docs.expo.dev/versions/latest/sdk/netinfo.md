---
title: '@react-native-community/netinfo'
description: A cross-platform API that provides access to network information.
sourceCodeUrl: 'https://github.com/react-native-community/react-native-netinfo'
packageName: '@react-native-community/netinfo'
platforms: ['android', 'ios', 'tvos', 'web', 'expo-go']
inExpoGo: true
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/netinfo/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/netinfo/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Reference (v57.0.0) > Third-party libraries
Pages in this section:
- [Overview](https://docs.expo.dev/versions/latest/sdk/third-party-overview.md)
- [@react-native-async-storage/async-storage](https://docs.expo.dev/versions/latest/sdk/async-storage.md)
- [@react-native-community/datetimepicker](https://docs.expo.dev/versions/latest/sdk/date-time-picker.md)
- [@react-native-community/netinfo](https://docs.expo.dev/versions/latest/sdk/netinfo.md) (this page)
- [@react-native-community/slider](https://docs.expo.dev/versions/latest/sdk/slider.md)
- [@react-native-masked-view/masked-view](https://docs.expo.dev/versions/latest/sdk/masked-view.md)
- [@react-native-picker/picker](https://docs.expo.dev/versions/latest/sdk/picker.md)
- [@react-native-segmented-control/segmented-control](https://docs.expo.dev/versions/latest/sdk/segmented-control.md)
- [@shopify/flash-list](https://docs.expo.dev/versions/latest/sdk/flash-list.md)
- [@shopify/react-native-skia](https://docs.expo.dev/versions/latest/sdk/skia.md)
- [@stripe/stripe-react-native](https://docs.expo.dev/versions/latest/sdk/stripe.md)
- [react-native-gesture-handler](https://docs.expo.dev/versions/latest/sdk/gesture-handler.md)
- [react-native-keyboard-controller](https://docs.expo.dev/versions/latest/sdk/keyboard-controller.md)
- [react-native-maps](https://docs.expo.dev/versions/latest/sdk/map-view.md)
- [react-native-pager-view](https://docs.expo.dev/versions/latest/sdk/view-pager.md)
- [react-native-reanimated](https://docs.expo.dev/versions/latest/sdk/reanimated.md)
- [react-native-safe-area-context](https://docs.expo.dev/versions/latest/sdk/safe-area-context.md)
- [react-native-screens](https://docs.expo.dev/versions/latest/sdk/screens.md)
- [react-native-svg](https://docs.expo.dev/versions/latest/sdk/svg.md)
- [react-native-view-shot](https://docs.expo.dev/versions/latest/sdk/captureRef.md)
- [react-native-webview](https://docs.expo.dev/versions/latest/sdk/webview.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# @react-native-community/netinfo

A cross-platform API that provides access to network information.
Android, iOS, tvOS, Web, Included in Expo Go

`@react-native-community/netinfo` allows you to get information about connection type and connection quality.

## Installation

```sh
# npm
npx expo install @react-native-community/netinfo

# yarn
yarn expo install @react-native-community/netinfo

# pnpm
pnpm expo install @react-native-community/netinfo

# bun
bun expo install @react-native-community/netinfo
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project. Then, follow the [installation instructions](https://github.com/react-native-community/react-native-netinfo#getting-started) provided in the library's README or documentation.

## API

To import this library, use:

```js
import NetInfo from '@react-native-community/netinfo';
```

If you want to grab information about the network connection just once, you can use:

```js
NetInfo.fetch().then(state => {
  console.log('Connection type', state.type);
  console.log('Is connected?', state.isConnected);
});
```

Or, if you'd rather subscribe to updates about the network state (which then allows you to run code/perform actions anytime the network state changes) use:

```js
const unsubscribe = NetInfo.addEventListener(state => {
  console.log('Connection type', state.type);
  console.log('Is connected?', state.isConnected);
});

// To unsubscribe to these update, just use:
unsubscribe();
```

## Accessing the SSID

To access the `ssid` property (available under `state.details.ssid`), there are a few additional configuration steps:

-   Request location permissions with [`Location.requestForegroundPermissionsAsync()`](/versions/latest/sdk/location.md#locationrequestforegroundpermissionsasync) or [`Location.requestBackgroundPermissionsAsync()`](/versions/latest/sdk/location.md#locationrequestbackgroundpermissionsasync).

### iOS only

-   Add the `com.apple.developer.networking.wifi-info` entitlement to your **app.json** under `ios.entitlements`:
    
    ```json
    "ios": {
        "entitlements": {
          "com.apple.developer.networking.wifi-info": true
        }
      }
    ```
    
-   Check the **Access Wi-Fi Information** box in your app's App Identifier, [which can be found here](https://developer.apple.com/account/resources/identifiers/list).
    
-   Rebuild your app with [`eas build --platform ios`](/build/setup.md#run-a-build) or [`npx expo run:ios`](/more/expo-cli.md#compiling).
    

## Learn more

[Visit official documentation](https://github.com/react-native-netinfo/react-native-netinfo) — Get full information on API and its usage.
