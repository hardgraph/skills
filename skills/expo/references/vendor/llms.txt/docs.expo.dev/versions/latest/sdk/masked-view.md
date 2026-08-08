---
title: '@react-native-masked-view/masked-view'
description: A library that provides a masked view.
sourceCodeUrl: 'https://github.com/react-native-masked-view/masked-view'
packageName: '@react-native-masked-view/masked-view'
platforms: ['android', 'ios', 'tvos', 'expo-go']
inExpoGo: true
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/masked-view/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/masked-view/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [@react-native-masked-view/masked-view](https://docs.expo.dev/versions/latest/sdk/masked-view.md) (this page)
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

# @react-native-masked-view/masked-view

A library that provides a masked view.
Android, iOS, tvOS, Included in Expo Go

> [`@expo/ui` provides a drop-in replacement](/versions/latest/sdk/ui/drop-in-replacements/maskedview.md) for `@react-native-masked-view/masked-view`, powered by Jetpack Compose on Android and SwiftUI on iOS.

`@react-native-masked-view/masked-view` provides a masked view that only displays the pixels that overlap with the view rendered in its mask element.

> You can only have one of either `@react-native-community/masked-view` (deprecated) or `@react-native-masked-view/masked-view` installed in your project at any given time. React Navigation v6 and later requires `@react-native-masked-view/masked-view`, so you should use that package instead if you are using the latest version of React Navigation.

> Android support for this library is [experimental](/more/release-statuses.md#experimental) and you may encounter inconsistencies in behavior across platforms. Report issues you encounter to [`react-native-masked-view` GitHub repository](https://github.com/react-native-masked-view/masked-view).

## Installation

```sh
# npm
npx expo install @react-native-masked-view/masked-view

# yarn
yarn expo install @react-native-masked-view/masked-view

# pnpm
pnpm expo install @react-native-masked-view/masked-view

# bun
bun expo install @react-native-masked-view/masked-view
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project. Then, follow the [installation instructions](https://github.com/react-native-masked-view/masked-view#getting-started) provided in the library's README or documentation.

## Learn more

[Visit official documentation](https://github.com/react-native-masked-view/masked-view) — Get full information on API and its usage.
