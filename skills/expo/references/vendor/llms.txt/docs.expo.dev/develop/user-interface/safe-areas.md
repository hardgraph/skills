---
modificationDate: June 11, 2026
title: Safe areas
description: Learn how to add safe areas for screen components inside your Expo project.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/develop/user-interface/safe-areas/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/develop/user-interface/safe-areas/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > Develop > User interface
Pages in this section:
- [Splash screen and app icon](https://docs.expo.dev/develop/user-interface/splash-screen-and-app-icon.md)
- [Safe areas](https://docs.expo.dev/develop/user-interface/safe-areas.md) (this page)
- [System bars](https://docs.expo.dev/develop/user-interface/system-bars.md)
- [Fonts](https://docs.expo.dev/develop/user-interface/fonts.md)
- [Assets](https://docs.expo.dev/develop/user-interface/assets.md)
- [Color themes](https://docs.expo.dev/develop/user-interface/color-themes.md)
- [Animation](https://docs.expo.dev/develop/user-interface/animation.md)
- [Store data](https://docs.expo.dev/develop/user-interface/store-data.md)
- [Next steps](https://docs.expo.dev/develop/user-interface/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Safe areas

Learn how to add safe areas for screen components inside your Expo project.

Creating a safe area ensures your app screen's content is positioned correctly. This means it doesn't get overlapped by notches, status bars, home indicators, and other interface elements that are part of the device's physical hardware or are controlled by the operating system. When the content gets overlapped, it gets concealed by these interface elements.

Here's an example of an app screen's content getting concealed by the status bar on Android. On iOS, the same content is concealed by rounded corners, notch, and the status bar.

## Use `react-native-safe-area-context` library

[`react-native-safe-area-context`](https://github.com/AppAndFlow/react-native-safe-area-context) provides a flexible API for handling Android and iOS device's safe area insets. It also provides a `SafeAreaView` component that you can use instead of a [`<View>`](https://reactnative.dev/docs/view) to account for safe areas automatically in your screen components.

Using the library, the result of the previous example changes as it displays the content inside a safe area, as shown below:

### Installation

You can skip installing `react-native-safe-area-context` if you have created a project using [the default template](/get-started/create-a-project.md). This library is installed as peer dependency for Expo Router library. Otherwise, install it by running the following command:

```sh
# npm
npx expo install react-native-safe-area-context

# yarn
yarn expo install react-native-safe-area-context

# pnpm
pnpm expo install react-native-safe-area-context

# bun
bun expo install react-native-safe-area-context
```

### Usage

You can directly use [`SafeAreaView`](https://appandflow.github.io/react-native-safe-area-context/api/safe-area-view) to wrap the content of your screen's component. It is a regular `<View>` with the safe area insets applied as extra padding or margin.

```tsx
import { Text } from 'react-native';
import { SafeAreaView } from 'react-native-safe-area-context';

export default function HomeScreen() {
  return (
    <SafeAreaView style={{ flex: 1 }}>
      <Text>Content is in safe area.</Text>
    </SafeAreaView>
  );
}
```

#### Using a different Expo template and don't have Expo Router installed?

Import and add [`SafeAreaProvider`](https://appandflow.github.io/react-native-safe-area-context/api/safe-area-provider) to the root component file (such as **App.tsx**) before using `SafeAreaView` in your screen component.

```tsx
import { SafeAreaProvider } from 'react-native-safe-area-context';

export default function App() {
  return (
    <SafeAreaProvider>
      {/* Your app content */}
    </SafeAreaProvider>
  );
}
```

## Alternate: `useSafeAreaInsets` hook

Alternate to `SafeAreaView`, you can use [`useSafeAreaInsets`](https://appandflow.github.io/react-native-safe-area-context/api/use-safe-area-insets) hook in your screen component. It provides direct access to the safe area insets, allowing you to apply padding for each edge of the `<View>` using an inset from this hook.

The example below uses the `useSafeAreaInsets` hook. It applies top padding to a `<View>` using `insets.top`.

```tsx
import { Text, View } from 'react-native';
import { useSafeAreaInsets } from 'react-native-safe-area-context';

export default function HomeScreen() {
  const insets = useSafeAreaInsets();

  return (
    <View style={{ flex: 1, paddingTop: insets.top }}>
      <Text>Content is in safe area.</Text>
    </View>
  );
}
```

The hook provides the insets in the following object:

```ts
{
  top: number,
  right: number,
  bottom: number,
  left: number
}
```

## Additional information

### Minimal example

Below is a minimal working example that uses the `useSafeAreaInsets` hook to apply top padding to a view.

```tsx
import { Text, View } from 'react-native';
import { SafeAreaProvider, useSafeAreaInsets } from 'react-native-safe-area-context';

function HomeScreen() {
  const insets = useSafeAreaInsets();
  return (
    <View style={{ flex: 1, paddingTop: insets.top }}>
      <Text style={{ fontSize: 28 }}>Content is in safe area.</Text>
    </View>
  );
}

export default function App() {
  return (
    <SafeAreaProvider>
      <HomeScreen />
    </SafeAreaProvider>
  );
}
```

### Usage with React Navigation

By default, React Navigation supports safe areas and uses `react-native-safe-area-context` as a peer dependency. For more information, see the [React Navigation documentation](https://reactnavigation.org/docs/handling-safe-area/).

### Usage with web

If you are targeting the web, set up `SafeAreaProvider` as described in the [usage section](/develop/user-interface/safe-areas.md#usage). If you are doing server-side rendering (SSR), see the [Web SSR section](https://appandflow.github.io/react-native-safe-area-context/optimizations#web-ssr) in the library's documentation.
