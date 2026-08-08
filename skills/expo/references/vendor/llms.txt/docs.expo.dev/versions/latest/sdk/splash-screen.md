---
title: SplashScreen
description: A library that provides access to controlling the visibility behavior of native splash screen.
sourceCodeUrl: 'https://github.com/expo/expo/tree/sdk-57/packages/expo-splash-screen'
packageName: 'expo-splash-screen'
exampleName: 'with-splash-screen'
iconUrl: '/static/images/packages/expo-splash-screen.png'
platforms: ['android', 'ios', 'tvos']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/splash-screen/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/splash-screen/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, use llms.txt to find the relevant page as Markdown (.md) instead of guessing.

You are here: Reference (v57.0.0) > Expo SDK (86 pages in this section)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo SplashScreen

A library that provides access to controlling the visibility behavior of native splash screen.
Android, iOS, tvOS

The `SplashScreen` module from the `expo-splash-screen` library provides control over the native splash screen behavior. By default, the splash screen will automatically hide when your app is ready, but you can also manually control its visibility for advanced use cases.

> From **SDK 52**, due to changes supporting the latest Android splash screen API, Expo Go and development builds cannot fully replicate the splash screen experience your users will see in your [standalone app](/more/glossary-of-terms.md#standalone-app). Expo Go will show your app icon instead of the splash screen, and the splash screen on development builds will not reflect all properties set in the config plugin. **It is highly recommended that you test your splash screen on a release build to ensure it looks as expected.**

Also, see the guide on [creating a splash screen image](/develop/user-interface/splash-screen-and-app-icon.md#splash-screen), or [quickly generate an icon and splash screen using your browser](https://buildicon.netlify.app/).

## Installation

#### Install library

```sh
# npm
npx expo install expo-splash-screen

# yarn
yarn expo install expo-splash-screen

# pnpm
pnpm expo install expo-splash-screen

# bun
bun expo install expo-splash-screen
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project.

#### Start with an example

The [`with-splash-screen`](https://github.com/expo/examples/tree/master/with-splash-screen) example comes with `expo-splash-screen` already installed and configured:

```sh
# npm
npx create-expo-app --example with-splash-screen

# yarn
yarn create expo-app --example with-splash-screen

# pnpm
pnpm create expo-app --example with-splash-screen

# bun
bun create expo --example with-splash-screen
```

## Usage

For most apps, you don't need to do anything special with the splash screen. It will automatically hide when your app is ready. You can optionally configure animation options:

#### With Expo Router

```tsx
import { Stack } from 'expo-router';
import * as SplashScreen from 'expo-splash-screen';

// Set the animation options. This is optional.
SplashScreen.setOptions({
  duration: 1000,
  fade: true,
});

export default function RootLayout() {
  return <Stack />;
}
```

#### Without Expo Router

```tsx
import { Text, View } from 'react-native';
import Entypo from '@expo/vector-icons/Entypo';
import * as SplashScreen from 'expo-splash-screen';

// Set the animation options. This is optional.
SplashScreen.setOptions({
  duration: 1000,
  fade: true,
});

export default function App() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>SplashScreen Demo! 👋</Text>
      <Entypo name="rocket" size={30} />
    </View>
  );
}
```

### Delay hiding the splash screen

In some cases, it may be necessary to delay hiding the splash screen until certain resources are loaded. For example, if you need to load API data before displaying the app content, you can use `preventAutoHideAsync()` to manually control when the splash screen hides. The goal should be to hide the splash screen as soon as possible.

#### With Expo Router

```tsx
import { Stack } from 'expo-router';
import * as SplashScreen from 'expo-splash-screen';
import { useEffect, useState } from 'react';

// Keep the splash screen visible while we fetch resources
SplashScreen.preventAutoHideAsync();

export default function RootLayout() {
  const [isReady, setIsReady] = useState(false);

  useEffect(() => {
    async function doAsyncStuff() {
      try {
        // do something async here
      } catch (e) {
        console.warn(e);
      } finally {
        setIsReady(true);
      }
    }

    doAsyncStuff();
  }, []);

  useEffect(() => {
    if (isReady) {
      SplashScreen.hide();
    }
  }, [isReady]);

  if (!isReady) {
    return null;
  }

  return <Stack />;
}
```

#### Without Expo Router

```tsx
import { useCallback, useEffect, useState } from 'react';
import { Text, View } from 'react-native';
import Entypo from '@expo/vector-icons/Entypo';
import * as SplashScreen from 'expo-splash-screen';

// Keep the splash screen visible while we fetch resources
SplashScreen.preventAutoHideAsync();

export default function App() {
  const [isReady, setIsReady] = useState(false);

  useEffect(() => {
    async function doAsyncStuff() {
      try {
        // do something async here
      } catch (e) {
        console.warn(e);
      } finally {
        setIsReady(true);
      }
    }

    doAsyncStuff();
  }, []);

  useEffect(() => {
    if (isReady) {
      SplashScreen.hideAsync();
    }
  }, [isReady]);

  if (!isReady) {
    return null;
  }

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>SplashScreen Demo! 👋</Text>
      <Entypo name="rocket" size={30} />
    </View>
  );
}
```

## Configuration

You can configure `expo-splash-screen` using its built-in [config plugin](/config-plugins/introduction.md) if you use config plugins in your project ([Continuous Native Generation (CNG)](/workflow/continuous-native-generation.md)). The plugin allows you to configure various properties that cannot be set at runtime and require building a new app binary to take effect. If your app does **not** use CNG, then you'll need to manually configure the library.

**Using the config plugin, as shown below, is the recommended method for configuring the splash screen.** The other methods are now considered legacy and will be removed in the future.

### Example app.json with config plugin

```json
{
  "expo": {
    "plugins": [
      [
        "expo-splash-screen",
        {
          "backgroundColor": "#232323",
          "image": "./assets/splash-icon.png",
          "dark": {
            "image": "./assets/splash-icon-dark.png",
            "backgroundColor": "#000000"
          },
          "imageWidth": 200
        }
      ]
    ],
  }
}
```

### Configurable properties

| Name | Default | Description |
| --- | --- | --- |
| `backgroundColor` | `#ffffff` | A hex color string representing the background color of the splash screen. |
| `image` | `undefined` | The path to the image file that will be displayed on the splash screen. This should be your app icon or logo. |
| `enableFullScreenImage_legacy` | `false` | Only for: iOS. Enabling this property allows using a full screen image as the splashscreen. This is to help with the transition from the legacy splash screen configuration and will be removed in the future. |
| `dark` | `undefined` | An object containing properties for configuring the splash screen when the device is in dark mode. |
| `imageWidth` | `100` | The width to make the image. |
| `android` | `undefined` | An object containing properties for configuring the splash screen on Android. |
| `ios` | `undefined` | An object containing properties for configuring the splash screen on iOS. |
| `resizeMode` | `undefined` | Determines how the image is scaled to fit the container defined by `imageWidth`. Possible values: `contain`, `cover`, or `native`. |

#### Are you using this library in an existing React Native app?

See how to configure the native projects in the [installation instructions in the `expo-splash-screen` repository](https://github.com/expo/expo/tree/main/packages/expo-splash-screen#-installation-in-bare-react-native-projects).

### Animating the splash screen

`SplashScreen` provides an out-of-the-box fade animation. It can be configured using the `setOptions` method.

```tsx
SplashScreen.setOptions({
  duration: 1000,
  fade: true,
});
```

If you prefer to use custom animation, see the [`with-splash-screen`](https://github.com/expo/examples/tree/master/with-splash-screen) example on how to apply any arbitrary animations to your splash screen. You can initialize a new project from this example by running `npx create-expo-app --example with-splash-screen`.

## API

```tsx
import * as SplashScreen from 'expo-splash-screen';
```

## Props

### `android`

Supported platforms: Android.

Optional • Type: [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<AndroidSplashConfig\>

Properties for configuring the splash screen on Android.

### `backgroundColor`

Supported platforms: Android, iOS, tvOS.

Optional • Type: `string` • Default: `"#ffffff"`

Hex color for the splash screen background.

### `dark`

Supported platforms: Android, iOS, tvOS.

Optional • Type: `{ backgroundColor: string, image: string }`

Properties for configuring the splash screen in dark mode.

### `enableFullScreenImage_legacy`

Supported platforms: Android, iOS, tvOS.

Optional • Type: `boolean` • Default: `false`

Whether to use a full screen image as the splash screen. Legacy transition helper, will be removed.

### `image`

Supported platforms: Android, iOS, tvOS.

Optional • Type: `string`

Path to the image displayed on the splash screen.

### `imageWidth`

Supported platforms: Android, iOS, tvOS.

Optional • Type: `number` • Default: `100`

The width to make the image.

### `ios`

Supported platforms: iOS.

Optional • Type: [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<IOSSplashConfig\>

Properties for configuring the splash screen on iOS.

### `resizeMode`

Supported platforms: Android, iOS, tvOS.

Optional • Literal type: `string` • Default: `"contain"`

How the image is scaled. Accepts `contain`, `cover`, `native`.

Acceptable values are: `'contain'` | `'cover'` | `'native'`

## Methods

### `SplashScreen.hide()`

Supported platforms: Android, iOS, tvOS.

Hides the native splash screen immediately. Be careful to ensure that your app has content ready to display when you hide the splash screen, or you may see a blank screen briefly. See the ["Usage"](#usage) section for an example.

Returns: `void`

### `SplashScreen.hideAsync()`

Supported platforms: Android, iOS, tvOS.

Hides the native splash screen immediately. This method is provided for backwards compatability. See the ["Usage"](#usage) section for an example.

Returns: `Promise<void>`

### `SplashScreen.preventAutoHideAsync()`

Supported platforms: Android, iOS, tvOS.

Makes the native splash screen (configured in `app.json`) remain visible until `hideAsync` is called.

> **Important note**: It is recommended to call this in global scope without awaiting, rather than inside React components or hooks, because otherwise this might be called too late, when the splash screen is already hidden.

Returns: `Promise<boolean>`

Example

```ts
import * as SplashScreen from 'expo-splash-screen';

SplashScreen.preventAutoHideAsync();

export default function App() {
 // ...
}
```

### `SplashScreen.setOptions(options)`

Supported platforms: Android, iOS, tvOS.

| Parameter | Type |
| --- | --- |
| `options` | [SplashScreenOptions](#splashscreenoptions) |

  

Configures the splashscreens default animation behavior.

Returns: `void`

## Types

### `SplashScreenOptions`

Supported platforms: Android, iOS, tvOS.

| Property | Type | Description |
| --- | --- | --- |
| duration(optional) | `number` | The duration of the fade out animation in milliseconds. Default: `400` |
| fade(optional) | `boolean` | Supported platforms: iOS. Whether to hide the splash screen with a fade out animation. Default: `false` |
