---
modificationDate: June 03, 2026
title: Troubleshooting
description: Fixing common issues with Expo Router setup.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/router/reference/troubleshooting/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/router/reference/troubleshooting/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Expo Router > Reference
Pages in this section:
- [Error handling and loading states](https://docs.expo.dev/router/error-handling.md)
- [URL parameters](https://docs.expo.dev/router/reference/url-parameters.md)
- [Color](https://docs.expo.dev/router/reference/color.md)
- [Sitemap](https://docs.expo.dev/router/reference/sitemap.md)
- [Redirects](https://docs.expo.dev/router/reference/redirects.md)
- [Link preview](https://docs.expo.dev/router/reference/link-preview.md)
- [Typed routes](https://docs.expo.dev/router/reference/typed-routes.md)
- [Screen tracking for analytics](https://docs.expo.dev/router/reference/screen-tracking.md)
- [Top-level src directory](https://docs.expo.dev/router/reference/src-directory.md)
- [Testing](https://docs.expo.dev/router/reference/testing.md)
- [Troubleshooting](https://docs.expo.dev/router/reference/troubleshooting.md) (this page)
- [Reserved paths](https://docs.expo.dev/router/reference/reserved-paths.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Troubleshooting

Fixing common issues with Expo Router setup.

## Missing files or source maps in React Native DevTools

This can happen if your Chrome DevTools has exclusions within its ignore list. To fix the issue, use [React Native DevTools](https://reactnative.dev/docs/react-native-devtools):

1.  Start the React Native DevTools by pressing J from the development server running in the terminal window
2.  Open **Settings** by clicking the gear icon
3.  Under **Extensions**, click **Restore defaults and reload**
4.  Open **Settings** again, and go to **Ignore List** tab
5.  Uncheck any exclusions for `/node_modules/`

## `EXPO_ROUTER_APP_ROOT` not defined

If `process.env.EXPO_ROUTER_APP_ROOT` is not defined you'll see the following error:

```sh
Invalid call at line 11: process.env.EXPO_ROUTER_APP_ROOT First argument of require.context should be a string.
```

This can happen when the Babel plugin `expo-router/babel` is not used in the project **babel.config.js**. You can try clearing the cache with:

```sh
# npm
npx expo start --clear

# yarn
yarn expo start --clear

# pnpm
pnpm expo start --clear

# bun
bun expo start --clear
```

Alternatively, you can circumvent this issue by creating an **index.js** file in the root of your project with the following contents:

```jsx
import { registerRootComponent } from 'expo';
import { ExpoRoot } from 'expo-router';

// Must be exported or Fast Refresh won't update the context
export function App() {
  const ctx = require.context('./app');
  return <ExpoRoot context={ctx} />;
}

registerRootComponent(App);
```

Then, update your app's main entry point in **package.json**:

```json
{
  "main": "index.js"
  ... 
}
```

> Do not use this to change the root directory (**app**) as it won't account for usage in any other places.

## `require.context` not enabled

This can happen when using a custom version of `@expo/metro-config` that does not enable context modules. Expo Router requires the project **metro.config.js** to use `expo-router/metro` as the default configuration. Delete the **metro.config.js**, or extend `expo/metro-config`. See [Customizing metro](/guides/customizing-metro.md) for more information.

## Missing back button

If you set up a modal or another screen that is expected to have a back button, then you'll need to add [`unstable_settings`](/router/advanced/router-settings.md) to the route's layout to ensure the initial route is configured. Initial routes are somewhat unique to mobile apps and fit awkwardly in the system — improvements pending.

```tsx
export const unstable_settings = {
  initialRouteName: 'index',
};
```
