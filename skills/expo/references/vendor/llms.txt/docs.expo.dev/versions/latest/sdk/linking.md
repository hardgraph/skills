---
title: Linking
description: An API that provides methods to create and open deep links universally.
sourceCodeUrl: 'https://github.com/expo/expo/tree/sdk-57/packages/expo-linking'
packageName: 'expo-linking'
platforms: ['android', 'ios', 'web', 'tvos', 'expo-go']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/linking/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/linking/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, use llms.txt to find the relevant page as Markdown (.md) instead of guessing.

You are here: Reference (v57.0.0) > Expo SDK (86 pages in this section)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo Linking

An API that provides methods to create and open deep links universally.
Android, iOS, tvOS, Web, Included in Expo Go

`expo-linking` provides utilities for your app to interact with other installed apps using deep links. It also provides helper methods for constructing and parsing deep links into your app. This library is an extension of the React Native [`Linking`](https://reactnative.dev/docs/linking).

For a more comprehensive explanation of how to use `expo-linking`, refer to the [Linking into other apps](/linking/into-other-apps.md).

## Installation

```sh
# npm
npx expo install expo-linking

# yarn
yarn expo install expo-linking

# pnpm
pnpm expo install expo-linking

# bun
bun expo install expo-linking
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project.

## API

```js
import * as Linking from 'expo-linking';
```

## Hooks

### `useLinkingURL()`

Supported platforms: Android, iOS, tvOS, Web.

Returns the linking URL followed by any subsequent changes to the URL. Always returns the initial URL immediately on reload.

Returns: `string | null`

Returns the initial URL or `null`.

> **Deprecated:** Use `useLinkingURL` hook instead.

### `useURL()`

Supported platforms: Android, iOS, tvOS, Web.

Returns the initial URL followed by any subsequent changes to the URL.

Returns: `string | null`

Returns the initial URL or `null`.

## Methods

### `Linking.canOpenURL(url)`

Supported platforms: Android, iOS, tvOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `url` | `string` | The URL that you want to test can be opened. |

  

Determine whether or not an installed app can handle a given URL. On web this always returns `true` because there is no API for detecting what URLs can be opened.

Returns: `Promise<boolean>`

A `Promise` object that is fulfilled with `true` if the URL can be handled, otherwise it `false` if not. The `Promise` will reject on Android if it was impossible to check if the URL can be opened, and on iOS if you didn't [add the specific scheme in the `LSApplicationQueriesSchemes` key inside **Info.plist**](/linking/into-other-apps.md#custom-url-schemes).

### `Linking.clearInitialURL()`

Supported platforms: Android, iOS.

Clears the cached initial URL used to launch the app, subsequent calls to [`getLinkingURL()`] return `null` until a new deep link is received.

On web this is a no-op.

Returns: `void`

### `Linking.collectManifestSchemes()`

Supported platforms: Android, iOS, tvOS, Web.

Collect a list of platform schemes from the manifest.

This method is based on the `Scheme` modules from `@expo/config-plugins` which are used for collecting the schemes before prebuilding a native app.

-   Android: `scheme` -> `android.scheme` -> `android.package`
-   iOS: `scheme` -> `ios.scheme` -> `ios.bundleIdentifier`

Returns: `string[]`

### `Linking.createURL(path, namedParameters)`

Supported platforms: Android, iOS, tvOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `path` | `string` | Addition path components to append to the base URL. |
| `namedParameters`(optional) | [CreateURLOptions](/versions/latest/sdk/linking.md#createurloptions) | Additional options object. Default: `{}` |

  

Helper method for constructing a deep link into your app, given an optional path and set of query parameters. Creates a URI scheme with two slashes by default.

The scheme must be defined in the [app config](/versions/latest/config/app.md) under `expo.scheme` or `expo.{android,ios}.scheme`. Platform-specific schemes defined under `expo.{android,ios}.scheme` take precedence over universal schemes defined under `expo.scheme`.

#### Examples

-   Development and production builds: `<scheme>://path` - uses the optional `scheme` property if provided, and otherwise uses the first scheme defined by your app config
-   Web (dev): `https://localhost:19006/path`
-   Web (prod): `https://myapp.com/path`
-   Expo Go (dev): `exp://128.0.0.1:8081/--/path`

The behavior of this method in Expo Go for published updates is undefined and should not be relied upon. The created URL in this case is neither stable nor predictable during the lifetime of the app. If a stable URL is needed, for example in authorization callbacks, a build (or development build) of your application should be used and the scheme provided.

Returns: `string`

A URL string which points to your app with the given deep link information.

### `Linking.getInitialURL()`

Supported platforms: Android, iOS, tvOS, Web.

Get the URL that was used to launch the app if it was launched by a link.

Returns: `Promise<string>`

The URL string that launched your app, or `null`.

### `Linking.getLinkingURL()`

Supported platforms: Android, iOS, tvOS, Web.

Get the URL that was used to launch the app if it was launched by a link.

Returns: `string | null`

The URL string that launched your app, or `null`.

### `Linking.hasConstantsManifest()`

Supported platforms: Android, iOS, tvOS, Web.

Ensure the user has linked the expo-constants manifest in an existing React Native project.

Returns: `boolean`

### `Linking.hasCustomScheme()`

Supported platforms: Android, iOS, tvOS, Web.

Returns: `boolean`

### `Linking.openSettings()`

Supported platforms: Android, iOS, tvOS, Web.

Open the operating system settings app and displays the app’s custom settings, if it has any.

Returns: `Promise<void>`

### `Linking.openURL(url)`

Supported platforms: Android, iOS, tvOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `url` | `string` | A URL for the operating system to open. For example: `tel:5555555`, `exp://`. |

  

Attempt to open the given URL with an installed app. See the [Linking guide](/linking/overview.md) for more information.

Returns: `Promise<true>`

A `Promise` that is fulfilled with `true` if the link is opened operating system automatically or the user confirms the prompt to open the link. The `Promise` rejects if there are no applications registered for the URL or the user cancels the dialog.

### `Linking.parse(url)`

Supported platforms: Android, iOS, tvOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `url` | `string` | A URL that points to the currently running experience (for example, an output of `Linking.createURL()`). |

  

Helper method for parsing out deep link information from a URL.

Returns: `ParsedURL`

A `ParsedURL` object.

### `Linking.parseInitialURLAsync()`

Supported platforms: Android, iOS, tvOS, Web.

Helper method which wraps React Native's `Linking.getInitialURL()` in `Linking.parse()`. Parses the deep link information out of the URL used to open the experience initially. If no link opened the app, all the fields will be `null`.

> On the web it parses the current window URL.

Returns: `Promise<parsedurl>`

A promise that resolves with `ParsedURL` object.

### `Linking.resolveScheme(options)`

Supported platforms: Android, iOS, tvOS, Web.

| Parameter | Type |
| --- | --- |
| `options` | `{ isSilent: boolean, scheme: string }` |

  

Returns: `string`

### `Linking.sendIntent(action, extras)`

Supported platforms: Android.

| Parameter | Type |
| --- | --- |
| `action` | `string` |
| `extras`(optional) | [SendIntentExtras[]](#sendintentextras) |

  

Launch an Android intent with extras.

> Use [`expo-intent-launcher`](/versions/latest/sdk/intent-launcher.md) instead. `sendIntent` is only included in `Linking` for API compatibility with React Native's Linking API.

Returns: `Promise<void>`

## Event subscriptions

### `Linking.addEventListener(type, handler)`

Supported platforms: Android, iOS, tvOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `type` | `'url'` | The only valid type is `'url'`. |
| `handler` | [URLListener](/versions/latest/sdk/linking.md#urllistenerevent) | An [`URLListener`](#urllistenerevent) function that takes an `event` object of the type [`EventType`](#eventtype). |

  

Add a handler to `Linking` changes by listening to the `url` event type and providing the handler. It is recommended to use the [`useLinkingURL()`](#uselinkingurl) hook instead.

Returns: `EmitterSubscription`

An EmitterSubscription that has the remove method from EventSubscription

> **See:** [React Native documentation on Linking](https://reactnative.dev/docs/linking#addeventlistener).

## Types

### `CreateURLOptions`

Supported platforms: Android, iOS, tvOS, Web.

| Property | Type | Description |
| --- | --- | --- |
| isTripleSlashed(optional) | `boolean` | Should the URI be triple slashed `scheme:///path` or double slashed `scheme://path`. |
| queryParams(optional) | [QueryParams](#queryparams) | An object of parameters that will be converted into a query string. |
| scheme(optional) | `string` | URI protocol `<scheme>://` that must be built into your native app. |

### `EventType`

Supported platforms: Android, iOS, tvOS, Web.

| Property | Type | Description |
| --- | --- | --- |
| nativeEvent(optional) | [MessageEvent](https://developer.mozilla.org/en-US/docs/Web/API/MessageEvent) | - |
| url | `string` | - |

### `NativeURLListener(nativeEvent)`

Supported platforms: Android, iOS, tvOS, Web.

| Parameter | Type |
| --- | --- |
| `nativeEvent` | [MessageEvent](https://developer.mozilla.org/en-US/docs/Web/API/MessageEvent) |

Returns:

`void`

### `ParsedURL`

Supported platforms: Android, iOS, tvOS, Web.

| Property | Type | Description |
| --- | --- | --- |
| hostname | `string | null` | - |
| path | `string | null` | The path into the app specified by the URL. |
| queryParams | [QueryParams](#queryparams) | null | The set of query parameters specified by the query string of the url used to open the app. |
| scheme | `string | null` | - |

### `QueryParams`

Supported platforms: Android, iOS, tvOS, Web.

Type: `Record<string, undefined | string | string[]>`

### `SendIntentExtras`

Supported platforms: Android, iOS, tvOS, Web.

| Property | Type | Description |
| --- | --- | --- |
| key | `string` | - |
| value | `string | number | boolean` | - |

### `URLListener(event)`

Supported platforms: Android, iOS, tvOS, Web.

| Parameter | Type |
| --- | --- |
| `event` | `EventType` |

Returns:

`void`
