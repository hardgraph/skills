---
title: ScreenOrientation
description: A universal library for managing a device's screen orientation.
sourceCodeUrl: 'https://github.com/expo/expo/tree/sdk-57/packages/expo-screen-orientation'
packageName: 'expo-screen-orientation'
iconUrl: '/static/images/packages/expo-screen-orientation.png'
platforms: ['android', 'ios', 'web', 'expo-go']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/screen-orientation/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/screen-orientation/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, use llms.txt to find the relevant page as Markdown (.md) instead of guessing.

You are here: Reference (v57.0.0) > Expo SDK (86 pages in this section)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo ScreenOrientation

A universal library for managing a device's screen orientation.
Android, iOS, Web, Included in Expo Go

Screen Orientation is defined as the orientation in which graphics are painted on the device. For example, the figure below has a device in a vertical and horizontal physical orientation, but a portrait screen orientation. For physical device orientation, see the orientation section of [Device Motion](/versions/latest/sdk/devicemotion.md).

On both Android and iOS platforms, changes to the screen orientation will override any system settings or user preferences. On Android, it is possible to change the screen orientation while taking the user's preferred orientation into account. On iOS, user and system settings are not accessible by the application and any changes to the screen orientation will override existing settings.

> Web has [limited support](https://caniuse.com/#feat=deviceorientation).

## Installation

```sh
# npm
npx expo install expo-screen-orientation

# yarn
yarn expo install expo-screen-orientation

# pnpm
pnpm expo install expo-screen-orientation

# bun
bun expo install expo-screen-orientation
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project.

### Warning

Apple added support for _split view_ mode to iPads in iOS 9. This changed how the screen orientation is handled by the system. To put the matter shortly, for iOS, your iPad is always in landscape mode unless you open two applications side by side. To be able to lock screen orientation using this module you will need to disable support for this feature. For more information about the _split view_ mode, check out [the official Apple documentation](https://support.apple.com/en-us/HT207582).

## Configuration in app config

You can configure `expo-screen-orientation` using its built-in [config plugin](/config-plugins/introduction.md) if you use config plugins in your project ([Continuous Native Generation (CNG)](/workflow/continuous-native-generation.md)). The plugin allows you to configure various properties that cannot be set at runtime and require building a new app binary to take effect. If your app does **not** use CNG, then you'll need to manually configure the library.

### Example app.json with config plugin

```json
{
  "expo": {
    "ios": {
      "requireFullScreen": true
    },
    "plugins": [
      [
        "expo-screen-orientation",
        {
          "initialOrientation": "DEFAULT"
        }
      ]
    ]
  }
}
```

### Configurable properties

| Name | Default | Description |
| --- | --- | --- |
| `initialOrientation` | `undefined` | Only for: iOS. Sets the iOS initial screen orientation. Possible values: `DEFAULT`, `ALL`, `PORTRAIT`, `PORTRAIT_UP`, `PORTRAIT_DOWN`, `LANDSCAPE`, `LANDSCAPE_LEFT`, `LANDSCAPE_RIGHT` |

#### Are you using this library in an existing React Native app?

1.  Open the **ios** directory in Xcode with `xed ios`. If you don't have the directory, run `npx expo prebuild -p ios` to generate one.
2.  Tick the `Requires Full Screen` checkbox in Xcode. It should be located under **Project Target** > **General** > **Deployment Info**.

## Per-screen orientation with Expo Router

If you use [Expo Router](/router/introduction.md), you can set the orientation per screen using the `orientation` option on [`Stack.Screen`](/router/advanced/stack.md). This is powered by `react-native-screens` and is the recommended approach for per-screen orientation in stack navigators.

```tsx
import { Stack } from 'expo-router';

export default function Layout() {
  return (
    <Stack>
      <Stack.Screen name="index" options={{ orientation: 'portrait' }} />
      <Stack.Screen name="landscape" options={{ orientation: 'landscape' }} />
    </Stack>
  );
}
```

## API

```js
import * as ScreenOrientation from 'expo-screen-orientation';
```

## Methods

### `ScreenOrientation.getOrientationAsync()`

Supported platforms: Android, iOS, Web.

Gets the current screen orientation.

Returns: `Promise<orientation>`

Returns a promise that fulfils with an [`Orientation`](#orientation) value that reflects the current screen orientation.

### `ScreenOrientation.getOrientationLockAsync()`

Supported platforms: Android, iOS, Web.

Gets the current screen orientation lock type.

Returns: `Promise<orientationlock>`

Returns a promise which fulfils with an [`OrientationLock`](#orientationlock) value.

### `ScreenOrientation.getPlatformOrientationLockAsync()`

Supported platforms: Android, iOS, Web.

Gets the platform specific screen orientation lock type.

Returns: `Promise<platformorientationinfo>`

Returns a promise which fulfils with a [`PlatformOrientationInfo`](#platformorientationinfo) value.

### `ScreenOrientation.lockAsync(orientationLock)`

Supported platforms: Android, iOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `orientationLock` | [OrientationLock](#orientationlock) | The orientation lock to apply. See the [`OrientationLock`](#orientationlock) enum for possible values. |

  

Lock the screen orientation to a particular `OrientationLock`.

Returns: `Promise<void>`

Returns a promise with `void` value, which fulfils when the orientation is set.

Example

```ts
async function changeScreenOrientation() {
  await ScreenOrientation.lockAsync(ScreenOrientation.OrientationLock.LANDSCAPE_LEFT);
}
```

### `ScreenOrientation.lockPlatformAsync(options)`

Supported platforms: Android, iOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `options` | [PlatformOrientationInfo](#platformorientationinfo) | The platform specific lock to apply. See the [`PlatformOrientationInfo`](#platformorientationinfo) object type for the different platform formats. |

  

Returns: `Promise<void>`

Returns a promise with `void` value, resolving when the orientation is set and rejecting if an invalid option or value is passed.

### `ScreenOrientation.supportsOrientationLockAsync(orientationLock)`

Supported platforms: Android, iOS, Web.

| Parameter | Type |
| --- | --- |
| `orientationLock` | [OrientationLock](#orientationlock) |

  

Returns whether the [`OrientationLock`](#orientationlock) policy is supported on the device.

Returns: `Promise<boolean>`

Returns a promise that resolves to a `boolean` value that reflects whether or not the orientationLock is supported.

### `ScreenOrientation.unlockAsync()`

Supported platforms: Android, iOS, Web.

Sets the screen orientation back to the `OrientationLock.DEFAULT` policy.

Returns: `Promise<void>`

Returns a promise with `void` value, which fulfils when the orientation is set.

## Event subscriptions

### `ScreenOrientation.addOrientationChangeListener(listener)`

Supported platforms: Android, iOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `listener` | [OrientationChangeListener](/versions/latest/sdk/screen-orientation.md#orientationchangelistenerevent) | Each orientation update will pass an object with the new [`OrientationChangeEvent`](#orientationchangeevent) to the listener. |

  

Invokes the `listener` function when the screen orientation changes from `portrait` to `landscape` or from `landscape` to `portrait`. For example, it won't be invoked when screen orientation change from `portrait up` to `portrait down`, but it will be called when there was a change from `portrait up` to `landscape left`.

Returns: `EventSubscription`

> **Deprecated:** this function will be removed in a future version. Use `subscription.remove()` instead.

### `ScreenOrientation.removeOrientationChangeListener(subscription)`

Supported platforms: Android, iOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `subscription` | `EventSubscription` | A subscription object that manages the updates passed to a listener function on an orientation change. |

  

Unsubscribes the listener associated with the `Subscription` object from all orientation change updates.

Returns: `void`

> **Deprecated:** this function will be removed in future versions. Keep track of your own subscriptions.

### `ScreenOrientation.removeOrientationChangeListeners()`

Supported platforms: Android, iOS, Web.

Removes all listeners subscribed to orientation change updates.

Returns: `void`

## Interfaces

### `Subscription`

Supported platforms: Android, iOS, Web.

A subscription object that allows to conveniently remove an event listener from the emitter.

Subscription Methods

### `remove()`

Supported platforms: Android, iOS, Web.

Removes an event listener for which the subscription has been created. After calling this function, the listener will no longer receive any events from the emitter.

Returns: `void`

## Types

### `OrientationChangeEvent`

Supported platforms: Android, iOS, Web.

| Property | Type | Description |
| --- | --- | --- |
| orientationInfo | [ScreenOrientationInfo](#screenorientationinfo) | The current `ScreenOrientationInfo` of the device. |
| orientationLock | [OrientationLock](#orientationlock) | The current `OrientationLock` of the device. |

### `OrientationChangeListener(event)`

Supported platforms: Android, iOS, Web.

| Parameter | Type |
| --- | --- |
| `event` | [OrientationChangeEvent](#orientationchangeevent) |

Returns:

`void`

### `PlatformOrientationInfo`

Supported platforms: Android, iOS, Web.

| Property | Type | Description |
| --- | --- | --- |
| screenOrientationArrayIOS(optional) | [Orientation[]](#orientation) | Supported platforms: iOS. An array of orientations to allow on the iOS platform. |
| screenOrientationConstantAndroid(optional) | `number` | Supported platforms: Android. A constant to set using the Android native [API](https://developer.android.com/reference/android/R.attr#screenOrientation). For example, in order to set the lock policy to [unspecified](https://developer.android.com/reference/android/content/pm/ActivityInfo.html#SCREEN_ORIENTATION_UNSPECIFIED), `-1` should be passed in. |
| screenOrientationLockWeb(optional) | [WebOrientationLock](#weborientationlock) | Supported platforms: Web. A web orientation lock to apply in the browser. |

### `ScreenOrientationInfo`

Supported platforms: Android, iOS, Web.

| Property | Type | Description |
| --- | --- | --- |
| horizontalSizeClass(optional) | [SizeClassIOS](#sizeclassios) | Supported platforms: iOS. The [horizontal size class](https://developer.apple.com/documentation/uikit/uitraitcollection/1623508-horizontalsizeclass) of the device. |
| orientation | [Orientation](#orientation) | The current orientation of the device. |
| verticalSizeClass(optional) | [SizeClassIOS](#sizeclassios) | Supported platforms: iOS. The [vertical size class](https://developer.apple.com/documentation/uikit/uitraitcollection/1623513-verticalsizeclass) of the device. |

## Enums

### `Orientation`

Supported platforms: Android, iOS, Web.

#### `UNKNOWN`

`Orientation.UNKNOWN = 0`

An unknown screen orientation. For example, the device is flat, perhaps on a table.

#### `PORTRAIT_UP`

`Orientation.PORTRAIT_UP = 1`

Right-side up portrait interface orientation.

#### `PORTRAIT_DOWN`

`Orientation.PORTRAIT_DOWN = 2`

Upside down portrait interface orientation.

#### `LANDSCAPE_LEFT`

`Orientation.LANDSCAPE_LEFT = 3`

Left landscape interface orientation.

#### `LANDSCAPE_RIGHT`

`Orientation.LANDSCAPE_RIGHT = 4`

Right landscape interface orientation.

### `OrientationLock`

Supported platforms: Android, iOS, Web.

An enum whose values can be passed to the [`lockAsync`](#screenorientationlockasyncorientationlock) method.

> **Note:** `OrientationLock.ALL` and `OrientationLock.PORTRAIT` are invalid on devices which don't support `OrientationLock.PORTRAIT_DOWN`.

#### `DEFAULT`

`OrientationLock.DEFAULT = 0`

The default orientation. On iOS, this will allow all orientations except `Orientation.PORTRAIT_DOWN`. On Android, this lets the system decide the best orientation.

#### `ALL`

`OrientationLock.ALL = 1`

All four possible orientations

#### `PORTRAIT`

`OrientationLock.PORTRAIT = 2`

Any portrait orientation.

#### `PORTRAIT_UP`

`OrientationLock.PORTRAIT_UP = 3`

Right-side up portrait only.

#### `PORTRAIT_DOWN`

`OrientationLock.PORTRAIT_DOWN = 4`

Upside down portrait only.

#### `LANDSCAPE`

`OrientationLock.LANDSCAPE = 5`

Any landscape orientation.

#### `LANDSCAPE_LEFT`

`OrientationLock.LANDSCAPE_LEFT = 6`

Left landscape only.

#### `LANDSCAPE_RIGHT`

`OrientationLock.LANDSCAPE_RIGHT = 7`

Right landscape only.

#### `OTHER`

`OrientationLock.OTHER = 8`

A platform specific orientation. This is not a valid policy that can be applied in [`lockAsync`](#screenorientationlockasyncorientationlock).

#### `UNKNOWN`

`OrientationLock.UNKNOWN = 9`

An unknown screen orientation lock. This is not a valid policy that can be applied in [`lockAsync`](#screenorientationlockasyncorientationlock).

### `SizeClassIOS`

Supported platforms: Android, iOS, Web.

Each iOS device has a default set of [size classes](https://developer.apple.com/documentation/uikit/uiuserinterfacesizeclass) that you can use as a guide when designing your interface.

#### `UNKNOWN`

`SizeClassIOS.UNKNOWN = 0`

#### `COMPACT`

`SizeClassIOS.COMPACT = 1`

#### `REGULAR`

`SizeClassIOS.REGULAR = 2`

### `WebOrientation`

Supported platforms: Android, iOS, Web.

#### `LANDSCAPE_PRIMARY`

`WebOrientation.LANDSCAPE_PRIMARY = "landscape-primary"`

#### `LANDSCAPE_SECONDARY`

`WebOrientation.LANDSCAPE_SECONDARY = "landscape-secondary"`

#### `PORTRAIT_PRIMARY`

`WebOrientation.PORTRAIT_PRIMARY = "portrait-primary"`

#### `PORTRAIT_SECONDARY`

`WebOrientation.PORTRAIT_SECONDARY = "portrait-secondary"`

### `WebOrientationLock`

Supported platforms: Android, iOS, Web.

An enum representing the lock policies that can be applied on the web platform, modelled after the [W3C specification](https://w3c.github.io/screen-orientation/#dom-orientationlocktype). These values can be applied through the [`lockPlatformAsync`](#screenorientationlockplatformasyncoptions) method.

#### `ANY`

`WebOrientationLock.ANY = "any"`

#### `LANDSCAPE`

`WebOrientationLock.LANDSCAPE = "landscape"`

#### `LANDSCAPE_PRIMARY`

`WebOrientationLock.LANDSCAPE_PRIMARY = "landscape-primary"`

#### `LANDSCAPE_SECONDARY`

`WebOrientationLock.LANDSCAPE_SECONDARY = "landscape-secondary"`

#### `NATURAL`

`WebOrientationLock.NATURAL = "natural"`

#### `PORTRAIT`

`WebOrientationLock.PORTRAIT = "portrait"`

#### `PORTRAIT_PRIMARY`

`WebOrientationLock.PORTRAIT_PRIMARY = "portrait-primary"`

#### `PORTRAIT_SECONDARY`

`WebOrientationLock.PORTRAIT_SECONDARY = "portrait-secondary"`

#### `UNKNOWN`

`WebOrientationLock.UNKNOWN = "unknown"`
