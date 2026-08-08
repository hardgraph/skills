---
modificationDate: June 03, 2026
title: Migrate Expo Router from SDK 55 to SDK 56
description: Learn how to migrate Expo Router from SDK 55 to 56 using a codemod or manually.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/router/migrate/sdk-55-to-56/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/router/migrate/sdk-55-to-56/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Expo Router > Migration
Pages in this section:
- [React Navigation](https://docs.expo.dev/router/migrate/from-react-navigation.md)
- [Expo Webpack](https://docs.expo.dev/router/migrate/from-expo-webpack.md)
- [SDK 55 to 56](https://docs.expo.dev/router/migrate/sdk-55-to-56.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Migrate Expo Router from SDK 55 to SDK 56

Learn how to migrate Expo Router from SDK 55 to 56 using a codemod or manually.

In **SDK 56 and later**, Expo Router no longer supports importing from external `@react-navigation/*` packages in application code. Update those imports to the matching `expo-router` entry points. The runtime API is unchanged - only the module specifiers move.

## Automated migration

Run the codemod from the root of your project. It rewrites `@react-navigation/*` imports in your application code to the matching `expo-router` entry points.

```sh
# npm
npx expo-codemod sdk-56-expo-router-react-navigation-replace src

# yarn
yarn dlx expo-codemod sdk-56-expo-router-react-navigation-replace src

# pnpm
pnpm dlx expo-codemod sdk-56-expo-router-react-navigation-replace src

# bun
bunx expo-codemod sdk-56-expo-router-react-navigation-replace src
```

Replace `src` with the directory or glob that contains your application code.

## Manual migration

If you cannot run the codemod, repoint each import by hand:

```tsx
// Before (SDK 55)
import { ThemeProvider, DarkTheme } from '@react-navigation/native';
import { createMaterialTopTabNavigator } from '@react-navigation/material-top-tabs';

// After (SDK 56)
import { ThemeProvider, DarkTheme } from 'expo-router/react-navigation';
import { createMaterialTopTabNavigator } from 'expo-router/js-top-tabs';
```

Use the following table to map each React Navigation import in your code to its `expo-router` equivalent:

| React Navigation source | Expo Router target |
| --- | --- |
| `@react-navigation/native` | `expo-router/react-navigation` |
| `@react-navigation/core` | `expo-router/react-navigation` |
| `@react-navigation/elements` | `expo-router/react-navigation` |
| `@react-navigation/routers` | `expo-router/react-navigation` |
| `@react-navigation/stack` | `expo-router/js-stack` |
| `@react-navigation/bottom-tabs` | `expo-router/js-tabs` |
| `@react-navigation/material-top-tabs` | `expo-router/js-top-tabs` |
| `@react-navigation/native-stack` | No direct equivalent. Use the [`Stack`](/router/advanced/stack.md) layout instead. |
| `@react-navigation/drawer` | No direct equivalent. Use the [`Drawer`](/router/advanced/drawer.md) layout instead. |

## Libraries

Many third-party libraries still import from `@react-navigation/core`. To ease the SDK 56 transition, Expo CLI automatically rewrites those imports to `expo-router` when they originate from **node_modules**. Your application code is unaffected by this rewrite.

This is a temporary compatibility shim. A dedicated library migration guide will explain the replacement before the automatic rewrite is removed.

To opt out, set `EXPO_ROUTER_DISABLE_RN_NAVIGATION_CHECK=1` in your environment before starting the bundler. This also disables the bundler error for application code that imports from `@react-navigation/*`.

```sh
# npm
EXPO_ROUTER_DISABLE_RN_NAVIGATION_CHECK=1 npx expo start

# yarn
EXPO_ROUTER_DISABLE_RN_NAVIGATION_CHECK=1 yarn expo start

# pnpm
EXPO_ROUTER_DISABLE_RN_NAVIGATION_CHECK=1 pnpm expo start

# bun
EXPO_ROUTER_DISABLE_RN_NAVIGATION_CHECK=1 bun expo start
```
