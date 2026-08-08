---
modificationDate: June 03, 2026
title: Drawer
description: Learn how to use the Drawer layout in Expo Router.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/router/advanced/drawer/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/router/advanced/drawer/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Expo Router > Navigation patterns
Pages in this section:
- [Stack](https://docs.expo.dev/router/advanced/stack.md)
- [JavaScript tabs](https://docs.expo.dev/router/advanced/tabs.md)
- [Native tabs](https://docs.expo.dev/router/advanced/native-tabs.md)
- [Drawer](https://docs.expo.dev/router/advanced/drawer.md) (this page)
- [Authentication](https://docs.expo.dev/router/advanced/authentication.md)
- [Authentication (redirects)](https://docs.expo.dev/router/advanced/authentication-rewrites.md)
- [Nesting navigators](https://docs.expo.dev/router/advanced/nesting-navigators.md)
- [Modals](https://docs.expo.dev/router/advanced/modals.md)
- [Web modals](https://docs.expo.dev/router/advanced/web-modals.md)
- [Shared routes](https://docs.expo.dev/router/advanced/shared-routes.md)
- [Protected routes](https://docs.expo.dev/router/advanced/protected.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Drawer

Learn how to use the Drawer layout in Expo Router.

A navigation drawer is a common pattern in mobile apps, it allows users to swipe open a menu from a side of their screen to expose navigation options. This menu is also typically toggleable through a button in the app's header.

## Installation

In **SDK 56 and later**, the drawer navigator is bundled in `expo-router` and uses [`react-native-drawer-layout`](https://www.npmjs.com/package/react-native-drawer-layout) under the hood.

On Android and iOS, the drawer requires `react-native-reanimated` and `react-native-worklets` to drive its animations. On web, animation is handled by CSS.

#### SDK 56 and later

To use the drawer navigator, install these dependencies if you do not have them already:

```sh
# npm
npx expo install react-native-reanimated react-native-worklets react-native-gesture-handler

# yarn
yarn expo install react-native-reanimated react-native-worklets react-native-gesture-handler

# pnpm
pnpm expo install react-native-reanimated react-native-worklets react-native-gesture-handler

# bun
bun expo install react-native-reanimated react-native-worklets react-native-gesture-handler
```

#### SDK 54 and 55

To use [drawer navigator](https://reactnavigation.org/docs/drawer-navigator), install these dependencies if you do not have them already:

```sh
# npm
npx expo install @react-navigation/drawer react-native-reanimated react-native-worklets react-native-gesture-handler

# yarn
yarn expo install @react-navigation/drawer react-native-reanimated react-native-worklets react-native-gesture-handler

# pnpm
pnpm expo install @react-navigation/drawer react-native-reanimated react-native-worklets react-native-gesture-handler

# bun
bun expo install @react-navigation/drawer react-native-reanimated react-native-worklets react-native-gesture-handler
```

#### SDK 53 and earlier

To use [drawer navigator](https://reactnavigation.org/docs/drawer-navigator), install these dependencies if you do not have them already:

```sh
# npm
npx expo install @react-navigation/drawer react-native-reanimated react-native-gesture-handler

# yarn
yarn expo install @react-navigation/drawer react-native-reanimated react-native-gesture-handler

# pnpm
pnpm expo install @react-navigation/drawer react-native-reanimated react-native-gesture-handler

# bun
bun expo install @react-navigation/drawer react-native-reanimated react-native-gesture-handler
```

## Usage

Now you can use the `Drawer` layout to create a drawer navigator.

```tsx
import { Drawer } from 'expo-router/drawer';

export default function Layout() {
  return <Drawer />;
}
```

To edit the drawer navigation menu labels, titles and screen options specific screens are required as follows:

```tsx
import { Drawer } from 'expo-router/drawer';

export default function Layout() {
  return (
    <Drawer>
      <Drawer.Screen
        name="index" // This is the name of the page and must match the url from root
        options={{
          drawerLabel: 'Home',
          title: 'overview',
        }}
      />
      <Drawer.Screen
        name="user/[id]" // This is the name of the page and must match the url from root
        options={{
          drawerLabel: 'User',
          title: 'overview',
        }}
      />
    </Drawer>
  );
}
```
