---
modificationDate: June 11, 2026
title: Error handling and loading states
description: Learn how to handle unmatched routes, errors, and loading states in your app when using Expo Router.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/router/error-handling/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/router/error-handling/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Expo Router > Reference
Pages in this section:
- [Error handling and loading states](https://docs.expo.dev/router/error-handling.md) (this page)
- [URL parameters](https://docs.expo.dev/router/reference/url-parameters.md)
- [Color](https://docs.expo.dev/router/reference/color.md)
- [Sitemap](https://docs.expo.dev/router/reference/sitemap.md)
- [Redirects](https://docs.expo.dev/router/reference/redirects.md)
- [Link preview](https://docs.expo.dev/router/reference/link-preview.md)
- [Typed routes](https://docs.expo.dev/router/reference/typed-routes.md)
- [Screen tracking for analytics](https://docs.expo.dev/router/reference/screen-tracking.md)
- [Top-level src directory](https://docs.expo.dev/router/reference/src-directory.md)
- [Testing](https://docs.expo.dev/router/reference/testing.md)
- [Troubleshooting](https://docs.expo.dev/router/reference/troubleshooting.md)
- [Reserved paths](https://docs.expo.dev/router/reference/reserved-paths.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Error handling and loading states

Learn how to handle unmatched routes, errors, and loading states in your app when using Expo Router.

This guide specifies how to handle unmatched routes, errors, and loading states in your app when using Expo Router.

## Unmatched routes

Native apps don't have a server so there are technically no 404s. However, if you're implementing a router universally, then it makes sense to handle missing routes. This is done automatically for each app, but you can also customize it.

```tsx
import { Unmatched } from 'expo-router';
export default Unmatched;
```

This will render the default `Unmatched`. You can export any component you want to render instead. We recommend having a link to `/` so users can navigate back to the home screen.

### Route priority

On web, files are served in the following order:

1.  Static files in the **public** directory.
2.  Standard and dynamic routes in the app directory.
3.  [API routes](/router/web/api-routes.md) in the app directory.
4.  Not-found routes will be served last with a 404 status code.

## Error handling

Expo Router enables fine-tuned error handling to enable a more opinionated data-loading strategy in the future.

You can export a nested [`ErrorBoundary`](/versions/latest/sdk/router.md#errorboundary) component from any route to intercept and format component-level errors using [React Error Boundaries](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary):

```tsx
import { View, Text } from 'react-native';
import { type ErrorBoundaryProps } from 'expo-router';

export function ErrorBoundary({ error, retry }: ErrorBoundaryProps) {
  return (
    <View style={{ flex: 1, backgroundColor: "red" }}>
      <Text>{error.message}</Text>
      <Text onPress={retry}>Try Again?</Text>
    </View>
  );
}

export default function Page() { ... }
```

When you export an `ErrorBoundary` the route will be wrapped with a React Error Boundary effectively:

```tsx
function Route({ ErrorBoundary, Component }) {
  return (
    <Try catch={ErrorBoundary}>
      <Component />
    </Try>
  );
}
```

When `ErrorBoundary` is not present, the error will be thrown to the nearest parent's `ErrorBoundary` and accepts [`error`](/versions/latest/sdk/router.md#error) and [`retry`](/versions/latest/sdk/router.md#retry) props.

### Work in progress

React Native LogBox needs to be presented less aggressively to develop with errors. Currently, it shows for `console.error` and `console.warn`. However, it should ideally only show for uncaught errors.

## Loading states with Suspense fallback

> Custom suspense fallbacks are available in SDK 56 and later.

Expo Router wraps each route in a [React Suspense](https://react.dev/reference/react/Suspense) boundary. You can export a [`SuspenseFallback`](/versions/latest/sdk/router.md#suspensefallback) component from a layout file to customize the loading UI shown while any child route is suspended:

```tsx
import { ActivityIndicator, View } from 'react-native';
import { Stack } from 'expo-router';

export function SuspenseFallback() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <ActivityIndicator size="large" />
    </View>
  );
}

export default function RootLayout() {
  return <Stack />;
}
```

> When multiple parent layouts define a fallback, the nearest parent takes precedence.

### Accessing route parameters

The [`SuspenseFallback`](/versions/latest/sdk/router.md#suspensefallback) component receives `route` and `params` props. You can use `params` to display context-specific loading states for dynamic routes, for example:

```tsx
import { ActivityIndicator, Text, View } from 'react-native';
import { Stack, type SuspenseFallbackProps } from 'expo-router';

export function SuspenseFallback({ params }: SuspenseFallbackProps) {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Loading profile {params.id}...</Text>
      <ActivityIndicator size="large" />
    </View>
  );
}

export default function AppLayout() {
  return <Stack />;
}
```

### Limitations

-   [Async routes](/router/web/async-routes.md) do not support custom Suspense fallbacks.
