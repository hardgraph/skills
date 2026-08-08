---
title: DeviceMotion
description: A library that provides access to a device's motion and orientation sensors.
sourceCodeUrl: 'https://github.com/expo/expo/tree/sdk-57/packages/expo-sensors'
packageName: 'expo-sensors'
iconUrl: '/static/images/packages/expo-sensors.png'
platforms: ['android', 'ios', 'web', 'expo-go']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/devicemotion/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/devicemotion/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, use llms.txt to find the relevant page as Markdown (.md) instead of guessing.

You are here: Reference (v57.0.0) > Expo SDK (86 pages in this section)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo DeviceMotion

A library that provides access to a device's motion and orientation sensors.
Android, iOS, Web, Included in Expo Go

`DeviceMotion` from `expo-sensors` provides access to the device motion and orientation sensors. All data is presented in terms of three axes that run through a device. According to portrait orientation: X runs from left to right, Y from bottom to top and Z perpendicularly through the screen from back to front.

## Installation

```sh
# npm
npx expo install expo-sensors

# yarn
yarn expo install expo-sensors

# pnpm
pnpm expo install expo-sensors

# bun
bun expo install expo-sensors
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project.

## Configuration in app config

You can configure `DeviceMotion` from `expo-sensor` using its built-in [config plugin](/config-plugins/introduction.md) if you use config plugins in your project ([Continuous Native Generation (CNG)](/workflow/continuous-native-generation.md)). The plugin allows you to configure various properties that cannot be set at runtime and require building a new app binary to take effect. If your app does **not** use CNG, then you'll need to manually configure the library.

### Example app.json with config plugin

```json
{
  "expo": {
    "plugins": [
      [
        "expo-sensors",
        {
          "motionPermission": "Allow $(PRODUCT_NAME) to access your device motion."
        }
      ]
    ]
  }
}
```

### Configurable properties

| Name | Default | Description |
| --- | --- | --- |
| `motionPermission` | `"Allow $(PRODUCT_NAME) to access your device motion"` | Only for: iOS. A string to set the [`NSMotionUsageDescription`](#ios) permission message. |

#### Are you using this library in an existing React Native app?

If you're not using Continuous Native Generation ([CNG](/workflow/continuous-native-generation.md)) or you're using native **ios** project manually, then you need to configure `NSMotionUsageDescription` key in your native project to access `DeviceMotion` stats:

```xml
<key>NSMotionUsageDescription</key>
<string>Allow $(PRODUCT_NAME) to access your device motion</string>
```

## API

```js
import { DeviceMotion } from 'expo-sensors';
```

## Constants

### `Gravity`

Supported platforms: Android, iOS, Web.

Type: `number`

Constant value representing standard gravitational acceleration for Earth (`9.80665` m/s^2).

## Classes

### `DeviceMotion`

Supported platforms: Android, iOS, Web.

Type: Class extends [DeviceSensor](/versions/latest/sdk/sensors.md)<[DeviceMotionMeasurement](#devicemotionmeasurement)\>

A base class for subscribable sensors. The events emitted by this class are measurements specified by the parameter type `Measurement`.

DeviceMotion Properties

### `Gravity`

Supported platforms: Android, iOS, Web.

Type: `number` • Default: `ExponentDeviceMotion.Gravity`

Constant value representing standard gravitational acceleration for Earth (`9.80665` m/s^2).

DeviceMotion Methods

### `addListener(listener)`

Supported platforms: Android, iOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `listener` | Listener<[DeviceMotionMeasurement](#devicemotionmeasurement)\> | A callback that is invoked when a device motion sensor update is available. When invoked, the listener is provided a single argument that is a `DeviceMotionMeasurement` object. |

  

Subscribe for updates to the device motion sensor.

Returns: `EventSubscription`

A subscription that you can call `remove()` on when you would like to unsubscribe the listener.

### `getListenerCount()`

Supported platforms: Android, iOS, Web.

Returns the registered listeners count.

Returns: `number`

### `getPermissionsAsync()`

Supported platforms: Android, iOS, Web.

Checks user's permissions for accessing sensor.

Returns: `Promise<permissionresponse>`

### `hasListeners()`

Supported platforms: Android, iOS, Web.

Returns boolean which signifies if sensor has any listeners registered.

Returns: `boolean`

### `isAvailableAsync()`

Supported platforms: Android, iOS, Web.

> You should always check the sensor availability before attempting to use it.

Returns whether the accelerometer is enabled on the device.

On mobile web, you must first invoke `DeviceMotion.requestPermissionsAsync()` in a user interaction (i.e. touch event) before you can use this module. If the `status` is not equal to `granted` then you should inform the end user that they may have to open settings.

On **web** this starts a timer and waits to see if an event is fired. This should predict if the iOS device has the **device orientation** API disabled in **Settings > Safari > Motion & Orientation Access**. Some devices will also not fire if the site isn't hosted with **HTTPS** as `DeviceMotion` is now considered a secure API. There is no formal API for detecting the status of `DeviceMotion` so this API can sometimes be unreliable on web.

Returns: `Promise<boolean>`

A promise that resolves to a `boolean` denoting the availability of device motion sensor.

### `removeAllListeners()`

Supported platforms: Android, iOS, Web.

Removes all registered listeners.

Returns: `void`

> **Deprecated:** use subscription.remove() instead.

### `removeSubscription(subscription)`

Supported platforms: Android, iOS, Web.

| Parameter | Type |
| --- | --- |
| `subscription` | `EventSubscription` |

  

Returns: `void`

### `requestPermissionsAsync()`

Supported platforms: Android, iOS, Web.

Asks the user to grant permissions for accessing sensor.

Returns: `Promise<permissionresponse>`

### `setUpdateInterval(intervalMs)`

Supported platforms: Android, iOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `intervalMs` | `number` | Desired interval in milliseconds between sensor updates. Starting from Android 12 (API level 31), the system has a 200Hz limit for each sensor updates. |

  

Set the sensor update interval.

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

### `DeviceMotionMeasurement`

Supported platforms: Android, iOS, Web.

| Property | Type | Description |
| --- | --- | --- |
| acceleration | `null | { timestamp: number, x: number, y: number, z: number }` | Device acceleration on the three axis as an object with `x`, `y`, `z` keys. Expressed in meters per second squared (m/s^2). |
| accelerationIncludingGravity | `{ timestamp: number, x: number, y: number, z: number }` | Device acceleration with the effect of gravity on the three axis as an object with `x`, `y`, `z` keys. Expressed in meters per second squared (m/s^2). |
| interval | `number` | Interval at which data is obtained from the native platform. Expressed in **milliseconds** (ms). |
| orientation | [DeviceMotionOrientation](#devicemotionorientation) | Device orientation based on screen rotation. Value is one of:
-   `0` (portrait),
-   `90` (right landscape),
-   `180` (upside down),
-   `-90` (left landscape).

 |
| rotation | `{ alpha: number, beta: number, gamma: number, timestamp: number }` | Device's orientation in space as an object with alpha, beta, gamma keys where alpha is for rotation around Z axis, beta for X axis rotation and gamma for Y axis rotation. |
| rotationRate | `null | { alpha: number, beta: number, gamma: number, timestamp: number }` | Device's rate of rotation in space expressed in degrees per second (deg/s). |

### `PermissionExpiration`

Supported platforms: Android, iOS, Web.

Literal type: `union`

Permission expiration time. Currently, all permissions are granted permanently.

Acceptable values are: `'never'` | `number`

### `PermissionResponse`

Supported platforms: Android, iOS, Web.

An object obtained by permissions get and request functions.

| Property | Type | Description |
| --- | --- | --- |
| canAskAgain | `boolean` | Indicates if user can be asked again for specific permission. If not, one should be directed to the Settings app in order to enable/disable the permission. |
| expires | `PermissionExpiration` | Determines time when the permission expires. |
| granted | `boolean` | A convenience boolean that indicates if the permission is granted. |
| status | [PermissionStatus](/versions/latest/sdk/expo.md#permissionstatus) | Determines the status of the permission. |

## Enums

### `DeviceMotionOrientation`

Supported platforms: Android, iOS, Web.

#### `LeftLandscape`

`DeviceMotionOrientation.LeftLandscape = -90`

#### `Portrait`

`DeviceMotionOrientation.Portrait = 0`

#### `RightLandscape`

`DeviceMotionOrientation.RightLandscape = 90`

#### `UpsideDown`

`DeviceMotionOrientation.UpsideDown = 180`

### `PermissionStatus`

Supported platforms: Android, iOS, Web.

#### `DENIED`

`PermissionStatus.DENIED = "denied"`

User has denied the permission.

#### `GRANTED`

`PermissionStatus.GRANTED = "granted"`

User has granted the permission.

#### `UNDETERMINED`

`PermissionStatus.UNDETERMINED = "undetermined"`

User hasn't granted or denied the permission yet.

## Permissions

### iOS

The following usage description keys are used by this library:

| Info.plist key | Description |
| --- | --- |
| `NSMotionUsageDescription` | A message that tells the user why the app is requesting access to the device’s motion data. |
