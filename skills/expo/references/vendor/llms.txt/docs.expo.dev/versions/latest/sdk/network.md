---
title: Network
description: A library that provides access to the device's network such as its IP address, MAC address, and airplane mode status.
sourceCodeUrl: 'https://github.com/expo/expo/tree/sdk-57/packages/expo-network'
packageName: 'expo-network'
iconUrl: '/static/images/packages/expo-network.png'
platforms: ['android', 'ios', 'web', 'tvos', 'expo-go']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/network/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/network/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, use llms.txt to find the relevant page as Markdown (.md) instead of guessing.

You are here: Reference (v57.0.0) > Expo SDK (86 pages in this section)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo Network

A library that provides access to the device's network such as its IP address, MAC address, and airplane mode status.
Android, iOS, tvOS, Web, Included in Expo Go

`expo-network` provides useful information about the device's network such as its IP address, MAC address, and airplane mode status.

## Installation

```sh
# npm
npx expo install expo-network

# yarn
yarn expo install expo-network

# pnpm
pnpm expo install expo-network

# bun
bun expo install expo-network
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project.

## Configuration

On Android, this module requires permissions to access the network and Wi-Fi state. The permissions `ACCESS_NETWORK_STATE` and `ACCESS_WIFI_STATE` are added automatically.

## API

```js
import * as Network from 'expo-network';
```

## Hooks

### `useNetworkState()`

Supported platforms: Android, iOS, tvOS, Web.

Returns the current network state of the device. This method initiates a listener for network state changes and cleans up before unmounting.

Returns: `NetworkState`

The current network state of the device, including connectivity and type.

Example

```ts
const networkState = useNetworkState();
console.log(`Current network type: ${networkState.type}`);
```

## Methods

### `Network.getIpAddressAsync()`

Supported platforms: Android, iOS, tvOS, Web.

Gets the device's current IPv4 address. Returns `0.0.0.0` if the IP address could not be retrieved.

On web, this method uses the third-party [`ipify service`](https://www.ipify.org/) to get the public IP address of the current device.

Returns: `Promise<string>`

A `Promise` that fulfils with a `string` of the current IP address of the device's main network interface. Can only be IPv4 address.

Example

```ts
await Network.getIpAddressAsync();
// "92.168.32.44"
```

### `Network.getNetworkStateAsync()`

Supported platforms: Android, iOS, tvOS, Web.

Gets the device's current network connection state.

On web, `navigator.connection.type` is not available on browsers. So if there is an active network connection, the field `type` returns `NetworkStateType.UNKNOWN`. Otherwise, it returns `NetworkStateType.NONE`.

Returns: `Promise<networkstate>`

A `Promise` that fulfils with a `NetworkState` object.

Example

```ts
await Network.getNetworkStateAsync();
// {
//   type: NetworkStateType.CELLULAR,
//   isConnected: true,
//   isInternetReachable: true,
// }
```

### `Network.isAirplaneModeEnabledAsync()`

Supported platforms: Android.

Tells if the device is in airplane mode.

Returns: `Promise<boolean>`

Returns a `Promise` that fulfils with a `boolean` value for whether the device is in airplane mode or not.

Example

```ts
await Network.isAirplaneModeEnabledAsync();
// false
```

## Event subscriptions

### `Network.addNetworkStateListener(listener)`

Supported platforms: Android, iOS, tvOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `listener` | (event: [NetworkState](#networkstate)) => void | Callback to execute when the network state changes. The callback is provided with a single argument that is an object containing information about the network state. |

  

Adds a listener that will fire whenever the network state changes.

Returns: `EventSubscription`

A subscription object with a remove function to unregister the listener.

Example

```ts
const subscription = addNetworkStateListener(({ type, isConnected, isInternetReachable }) => {
  console.log(`Network type: ${type}, Connected: ${isConnected}, Internet Reachable: ${isInternetReachable}`);
});
```

## Types

### `NetworkState`

Supported platforms: Android, iOS, tvOS, Web.

| Property | Type | Description |
| --- | --- | --- |
| isConnected(optional) | `boolean` | If there is an active network connection. Note that this does not mean that internet is reachable. This field is `false` if the type is either `Network.NetworkStateType.NONE` or `Network.NetworkStateType.UNKNOWN`, `true` otherwise. |
| isInternetReachable(optional) | `boolean` | If the internet is reachable with the currently active network connection. On Android, this requires the active network to have internet capability (`NET_CAPABILITY_INTERNET`), confirmed internet access (`NET_CAPABILITY_VALIDATED`), and a usable connection state. VPN connections also require non-zero downstream bandwidth. On iOS, this value will always be the same as `isConnected`. |
| type(optional) | [NetworkStateType](#networkstatetype) | A [`NetworkStateType`](#networkstatetype) enum value that represents the current network connection type. |

### `NetworkStateEvent`

Supported platforms: Android, iOS, tvOS, Web.

Type: [NetworkState](#networkstate)

Represents an event that provides the updated network state when there is a change in the network status. This is passed as the argument to listeners registered with [`addNetworkStateListener()`](#networkaddnetworkstatelistenerlistener).

## Enums

### `NetworkStateType`

Supported platforms: Android, iOS, tvOS, Web.

An enum of the different types of devices supported by Expo.

#### `BLUETOOTH`

Supported platforms: Android.

`NetworkStateType.BLUETOOTH = "BLUETOOTH"`

Active network connection over Bluetooth.

#### `CELLULAR`

Supported platforms: Android, iOS.

`NetworkStateType.CELLULAR = "CELLULAR"`

Active network connection over mobile data or [`DUN-specific`](https://developer.android.com/reference/android/net/ConnectivityManager#TYPE_MOBILE_DUN) mobile connection when setting an upstream connection for tethering.

#### `ETHERNET`

Supported platforms: Android, iOS.

`NetworkStateType.ETHERNET = "ETHERNET"`

Active network connection over Ethernet.

#### `NONE`

`NetworkStateType.NONE = "NONE"`

No active network connection detected.

#### `OTHER`

Supported platforms: Android.

`NetworkStateType.OTHER = "OTHER"`

Active network connection over other network connection types.

#### `UNKNOWN`

`NetworkStateType.UNKNOWN = "UNKNOWN"`

The connection type could not be determined.

#### `VPN`

Supported platforms: Android.

`NetworkStateType.VPN = "VPN"`

Active network connection over VPN.

#### `WIFI`

Supported platforms: Android, iOS.

`NetworkStateType.WIFI = "WIFI"`

Active network connection over Wi-Fi.

#### `WIMAX`

Supported platforms: Android.

`NetworkStateType.WIMAX = "WIMAX"`

Active network connection over WiMAX.

## Error codes

| Code | Description |
| --- | --- |
| ERR_NETWORK_IP_ADDRESS | On Android, there may be an unknown Wi-Fi host when trying to access `WifiManager` in `getIpAddressAsync`. On iOS, no network interfaces could be retrieved. |
| ERR_NETWORK_UNDEFINED_INTERFACE | An undefined `interfaceName` was passed as an argument in `getMacAddressAsync`. |
| ERR_NETWORK_SOCKET_EXCEPTION | An error was encountered in creating or accessing the socket in `getMacAddressAsync`. |
| ERR_NETWORK_INVALID_PERMISSION_INTERNET | There are invalid permissions for [`android.permission.ACCESS_WIFI_STATE`](https://developer.android.com/reference/android/Manifest.permission#ACCESS_WIFI_STATE) in `getMacAddressAsync`. |
| ERR_NETWORK_NO_ACCESS_NETWORKINFO | Unable to access network information |
