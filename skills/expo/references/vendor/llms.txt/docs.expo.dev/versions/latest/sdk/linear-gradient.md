---
title: LinearGradient
description: A universal React component that renders a gradient view.
sourceCodeUrl: 'https://github.com/expo/expo/tree/sdk-57/packages/expo-linear-gradient'
packageName: 'expo-linear-gradient'
iconUrl: '/static/images/packages/expo-linear-gradient.png'
platforms: ['android', 'ios', 'tvos', 'web', 'expo-go']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/linear-gradient/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/linear-gradient/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, use llms.txt to find the relevant page as Markdown (.md) instead of guessing.

You are here: Reference (v57.0.0) > Expo SDK (86 pages in this section)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo LinearGradient

A universal React component that renders a gradient view.
Android, iOS, tvOS, Web, Included in Expo Go

`expo-linear-gradient` provides a native React view that transitions between multiple colors in a linear direction.

> React Native also provides `experimental_backgroundImage` (Android and iOS) and `backgroundImage` (Web) style properties on `View` component that support CSS gradient syntax such as [`linear-gradient()`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/gradient/linear-gradient) and [`radial-gradient()`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/gradient/radial-gradient). This can be an alternative for gradient backgrounds without adding an extra dependency. Since this is an experimental property, if you encounter any issues, please [report them to the React Native repository](https://github.com/facebook/react-native/issues).

## Installation

```sh
# npm
npx expo install expo-linear-gradient

# yarn
yarn expo install expo-linear-gradient

# pnpm
pnpm expo install expo-linear-gradient

# bun
bun expo install expo-linear-gradient
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project.

## Usage

```tsx
import { StyleSheet, Text, View } from 'react-native';
import { LinearGradient } from 'expo-linear-gradient';

export default function App() {
  return (
    <View style={styles.container}>
      <LinearGradient
        // Background Linear Gradient
        colors={['rgba(0,0,0,0.8)', 'transparent']}
        style={styles.background}
      />
      <LinearGradient
        // Button Linear Gradient
        colors={['#4c669f', '#3b5998', '#192f6a']}
        style={styles.button}>
        <Text style={styles.text}>Sign in with Facebook</Text>
      </LinearGradient>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    backgroundColor: 'orange',
  },
  background: {
    position: 'absolute',
    left: 0,
    right: 0,
    top: 0,
    height: 300,
  },
  button: {
    padding: 15,
    alignItems: 'center',
    borderRadius: 5,
  },
  text: {
    backgroundColor: 'transparent',
    fontSize: 15,
    color: '#fff',
  },
});
```

## API

```js
import { LinearGradient } from 'expo-linear-gradient';
```

## Component

### `LinearGradient`

Supported platforms: Android, iOS, tvOS, Web.

Type: React.[Component](https://react.dev/reference/react/Component)<[LinearGradientProps](#lineargradientprops)\>

Renders a native view that transitions between multiple colors in a linear direction.

LinearGradientProps

### `colors`

Supported platforms: Android, iOS, tvOS, Web.

Type: readonly \[[ColorValue](https://reactnative.dev/docs/colors), [ColorValue](https://reactnative.dev/docs/colors), ...[ColorValue[]](https://reactnative.dev/docs/colors)\]

A readonly array of colors that represent stops in the gradient. At least two colors are required (for a single-color background, use the `style.backgroundColor` prop on a `View` component).

For TypeScript to know the provided array has 2 or more values, it should be provided "inline" or typed `as const`.

### `dither`

Supported platforms: Android.

Optional • Type: `boolean` • Default: `true`

Enables or disables paint dithering. Dithering can reduce the gradient color banding issue. Setting `false` may improve gradient rendering performance.

### `end`

Supported platforms: Android, iOS, tvOS, Web.

Optional • Literal type: `union`

For example, `{ x: 0.1, y: 0.2 }` means that the gradient will end `10%` from the left and `20%` from the bottom.

**On web**, this only changes the angle of the gradient because CSS gradients don't support changing the end position.

Acceptable values are: [LinearGradientPoint](#lineargradientpoint) | `null`

### `locations`

Supported platforms: Android, iOS, tvOS, Web.

Optional • Literal type: `union` • Default: `[]`

A readonly array that contains `number`s ranging from `0` to `1`, inclusive, and is the same length as the `colors` property. Each number indicates a color-stop location where each respective color should be located. If not specified, the colors will be distributed evenly across the gradient.

For example, `[0.5, 0.8]` would render:

-   the first color, solid, from the beginning of the gradient view to 50% through (the middle);
-   a gradient from the first color to the second from the 50% point to the 80% point; and
-   the second color, solid, from the 80% point to the end of the gradient view.

> The color-stop locations must be ascending from least to greatest.

Acceptable values are: `readonly [number, number, ...number[]]` | `null`

### `start`

Supported platforms: Android, iOS, tvOS, Web.

Optional • Literal type: `union`

For example, `{ x: 0.1, y: 0.2 }` means that the gradient will start `10%` from the left and `20%` from the top.

**On web**, this only changes the angle of the gradient because CSS gradients don't support changing the starting position.

Acceptable values are: [LinearGradientPoint](#lineargradientpoint) | `null`

#### Inherited props

-   [ViewProps](https://reactnative.dev/docs/view#props)

## Types

### `LinearGradientPoint`

Supported platforms: Android, iOS, tvOS, Web.

An object `{ x: number; y: number }` or array `[x, y]` that represents the point at which the gradient starts or ends, as a fraction of the overall size of the gradient ranging from `0` to `1`, inclusive.

Type: [NativeLinearGradientPoint](#nativelineargradientpoint) or `object` shaped as below:

| Property | Type | Description |
| --- | --- | --- |
| x | `number` | A number ranging from `0` to `1`, representing the position of gradient transformation. |
| y | `number` | A number ranging from `0` to `1`, representing the position of gradient transformation. |

### `NativeLinearGradientPoint`

Supported platforms: Android, iOS, tvOS, Web.

Tuple: `[x: number, y: number]`
