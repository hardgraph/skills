---
modificationDate: February 26, 2026
title: Nesting navigators
description: Learn how to nest navigators in Expo Router.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/router/advanced/nesting-navigators/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/router/advanced/nesting-navigators/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Expo Router > Navigation patterns
Pages in this section:
- [Stack](https://docs.expo.dev/router/advanced/stack.md)
- [JavaScript tabs](https://docs.expo.dev/router/advanced/tabs.md)
- [Native tabs](https://docs.expo.dev/router/advanced/native-tabs.md)
- [Drawer](https://docs.expo.dev/router/advanced/drawer.md)
- [Authentication](https://docs.expo.dev/router/advanced/authentication.md)
- [Authentication (redirects)](https://docs.expo.dev/router/advanced/authentication-rewrites.md)
- [Nesting navigators](https://docs.expo.dev/router/advanced/nesting-navigators.md) (this page)
- [Modals](https://docs.expo.dev/router/advanced/modals.md)
- [Web modals](https://docs.expo.dev/router/advanced/web-modals.md)
- [Shared routes](https://docs.expo.dev/router/advanced/shared-routes.md)
- [Protected routes](https://docs.expo.dev/router/advanced/protected.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Nesting navigators

Learn how to nest navigators in Expo Router.

> Navigation UI elements (Link, Tabs, Stack) may move out of the Expo Router library in the future.

[Using a Stack Navigator with Expo Router](https://www.youtube.com/watch?v=izZv6a99Roo) — Navigate between screens, pass params between screens, create dynamic routes, and configure the screen titles and animations.

Nesting navigators allow rendering a navigator inside the screen of another navigator. This guide is an extension of [React Navigation: Nesting navigators](https://reactnavigation.org/docs/nesting-navigators) to Expo Router. It provides an example of how nesting navigators work when using Expo Router.

## Example

Consider the following file structure which is used as an example:

`src`

 `app`

  `_layout.tsx`

  `index.tsx`

  `home`

   `_layout.tsx`

   `feed.tsx`

   `messages.tsx`

In the above example, **src/app/home/feed.tsx** matches `/home/feed`, and **src/app/home/messages.tsx** matches `/home/messages`.

```tsx
import { Stack } from 'expo-router';

export default Stack;
```

Both **src/app/home/_layout.tsx** and **src/app/index.tsx** below are nested in the **src/app/_layout.tsx** layout so that it will be rendered as a stack.

```tsx
import { Tabs } from 'expo-router';

export default Tabs;
```

```tsx
import { Link } from 'expo-router';

export default function Root() {
  return <Link href="/home/messages">Navigate to nested route</Link>;
}
```

Both **src/app/home/feed.tsx** and **src/app/home/messages.tsx** below are nested in the **home/_layout.tsx** layout, so it will be rendered as a tab.

```tsx
import { View, Text } from 'react-native';

export default function Feed() {
  return (
    <View>
      <Text>Feed screen</Text>
    </View>
  );
}
```

```tsx
import { View, Text } from 'react-native';

export default function Messages() {
  return (
    <View>
      <Text>Messages screen</Text>
    </View>
  );
}
```

## Stack inside native tabs

When using native tabs, you can nest a `<Stack />` layout inside each tab to support headers and pushing screens. For a complete example, see [Use Stacks inside tabs](/router/advanced/native-tabs.md#use-stacks-inside-tabs).

## Navigate to a screen in a nested navigator

In React Navigation, navigating to a specific nested screen can be controlled by passing the screen name in params. This renders the specified nested screen instead of the initial screen for that nested navigator.

For example, from the initial screen inside the `root` navigator, you want to navigate to a screen called `media` inside `settings` (a nested navigator). In React Navigation, this is done as shown in the example below:

```jsx
navigation.navigate('root', {
  screen: 'settings',
  params: {
    screen: 'media',
  },
});
```

In Expo Router, you can use `router.push()` to achieve the same result. There is no need to pass the screen name in the params explicitly.

```jsx
router.push('/root/settings/media');
```
