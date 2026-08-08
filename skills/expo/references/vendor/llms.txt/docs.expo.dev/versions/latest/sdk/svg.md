---
title: react-native-svg
description: A library that allows using SVGs in your app.
sourceCodeUrl: 'https://github.com/react-native-community/react-native-svg'
packageName: react-native-svg
platforms: ['android', 'ios', 'macos', 'web', 'tvos', 'expo-go']
inExpoGo: true
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/svg/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/svg/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [react-native-reanimated](https://docs.expo.dev/versions/latest/sdk/reanimated.md)
- [react-native-safe-area-context](https://docs.expo.dev/versions/latest/sdk/safe-area-context.md)
- [react-native-screens](https://docs.expo.dev/versions/latest/sdk/screens.md)
- [react-native-svg](https://docs.expo.dev/versions/latest/sdk/svg.md) (this page)
- [react-native-view-shot](https://docs.expo.dev/versions/latest/sdk/captureRef.md)
- [react-native-webview](https://docs.expo.dev/versions/latest/sdk/webview.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# react-native-svg

A library that allows using SVGs in your app.
Android, iOS, macOS, tvOS, Web, Included in Expo Go

`react-native-svg` allows you to use SVGs in your app, with support for interactivity and animation.

## Installation

```sh
# npm
npx expo install react-native-svg

# yarn
yarn expo install react-native-svg

# pnpm
pnpm expo install react-native-svg

# bun
bun expo install react-native-svg
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project. Then, follow the [installation instructions](https://github.com/react-native-community/react-native-svg#with-react-native-cli) provided in the library's README or documentation.

## API

```js
import * as Svg from 'react-native-svg';
```

### `Svg`

A set of drawing primitives such as `Circle`, `Rect`, `Path`, `ClipPath`, and `Polygon`. It supports most SVG elements and properties. The implementation is provided by [react-native-svg](https://github.com/react-native-community/react-native-svg), and documentation is provided in that repository.

```tsx
import Svg, { Circle, Rect } from 'react-native-svg';

export default function SvgComponent(props) {
  return (
    <Svg height="50%" width="50%" viewBox="0 0 100 100" {...props}>
      <Circle cx="50" cy="50" r="45" stroke="blue" strokeWidth="2.5" fill="green" />
      <Rect x="15" y="15" width="70" height="70" stroke="red" strokeWidth="2" fill="yellow" />
    </Svg>
  );
}
```

### Tips

Here are some helpful links that will get you moving fast!

-   Looking for SVGs? Try [Lucide](https://lucide.dev/).
-   Create or modify your own SVGs for free using [Figma](https://www.figma.com/).
-   Optimize your SVG with [SVGOMG](https://jakearchibald.github.io/svgomg/). This will make the code smaller and easier to work with. Be sure not to remove the `viewbox` for best results on Android.
-   Convert your SVG to an Expo component in the browser using [SVGR](https://react-svgr.com/playground/?native=true&typescript=true).

## Learn more

[Visit official documentation](https://github.com/software-mansion/react-native-svg) — Get full information on API and its usage.
