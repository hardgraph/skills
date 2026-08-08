---
title: react-native-reanimated
description: A library that provides an API that greatly simplifies the process of creating smooth, powerful, and maintainable animations.
sourceCodeUrl: 'https://github.com/software-mansion/react-native-reanimated'
packageName: react-native-reanimated
platforms: ['android', 'ios', 'tvos', 'web', 'expo-go']
inExpoGo: true
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/reanimated/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/reanimated/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Reference (v57.0.0) > Third-party libraries
Pages in this section:
- [Overview](https://docs.expo.dev/versions/latest/sdk/third-party-overview.md)
- [@react-native-async-storage/async-storage](https://docs.expo.dev/versions/latest/sdk/async-storage.md)
- [@react-native-community/datetimepicker](https://docs.expo.dev/versions/latest/sdk/date-time-picker.md)
- [@react-native-community/netinfo](https://docs.expo.dev/versions/latest/sdk/netinfo.md)
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
- [react-native-reanimated](https://docs.expo.dev/versions/latest/sdk/reanimated.md) (this page)
- [react-native-safe-area-context](https://docs.expo.dev/versions/latest/sdk/safe-area-context.md)
- [react-native-screens](https://docs.expo.dev/versions/latest/sdk/screens.md)
- [react-native-svg](https://docs.expo.dev/versions/latest/sdk/svg.md)
- [react-native-view-shot](https://docs.expo.dev/versions/latest/sdk/captureRef.md)
- [react-native-webview](https://docs.expo.dev/versions/latest/sdk/webview.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# react-native-reanimated

A library that provides an API that greatly simplifies the process of creating smooth, powerful, and maintainable animations.
Android, iOS, tvOS, Web, Included in Expo Go

[`react-native-reanimated`](https://docs.swmansion.com/react-native-reanimated/docs/fundamentals/getting-started/) provides an API that greatly simplifies the process of creating smooth, powerful, and maintainable animations.

> **Reanimated uses React Native APIs that are incompatible with "Remote JS Debugging" for JavaScriptCore**. To use a debugger with your app with `react-native-reanimated`, you'll need to use the [Hermes JavaScript engine](/guides/using-hermes.md) and the [JavaScript Inspector for Hermes](/guides/using-hermes.md#javascript-debugger).

## Installation

```sh
npx expo install react-native-reanimated react-native-worklets
```

  

No additional configuration is required. [Reanimated Babel plugin](https://docs.swmansion.com/react-native-reanimated/docs/fundamentals/glossary#reanimated-babel-plugin) is automatically configured in [`babel-preset-expo`](https://www.npmjs.com/package/babel-preset-expo) when you install the library.

## Usage

The following example shows how to use the `react-native-reanimated` library to create a simple animation.

```jsx
import Animated, {
  useSharedValue,
  withTiming,
  useAnimatedStyle,
  Easing,
} from 'react-native-reanimated';
import { View, Button, StyleSheet } from 'react-native';

export default function AnimatedStyleUpdateExample() {
  const randomWidth = useSharedValue(10);

  const config = {
    duration: 500,
    easing: Easing.bezier(0.5, 0.01, 0, 1),
  };

  const style = useAnimatedStyle(() => {
    return {
      width: withTiming(randomWidth.value, config),
    };
  });

  return (
    <View style={styles.container}>
      <Animated.View style={[styles.box, style]} />
      <Button
        title="toggle"
        onPress={() => {
          randomWidth.value = Math.random() * 350;
        }}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
  box: {
    width: 100,
    height: 80,
    backgroundColor: 'black',
    margin: 30,
  },
});
```

## Learn more

[Visit official documentation](https://docs.swmansion.com/react-native-reanimated/docs/fundamentals/your-first-animation) — Get full information on API and its usage.
