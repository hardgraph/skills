---
modificationDate: February 26, 2026
title: Router settings
description: Learn how to configure layouts with static properties in Expo Router.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/router/advanced/router-settings/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/router/advanced/router-settings/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Expo Router > Advanced
Pages in this section:
- [Platform-specific extensions and module](https://docs.expo.dev/router/advanced/platform-specific-modules.md)
- [Customizing links](https://docs.expo.dev/router/advanced/native-intent.md)
- [Settings](https://docs.expo.dev/router/advanced/router-settings.md) (this page)
- [Apple Handoff](https://docs.expo.dev/router/advanced/apple-handoff.md)
- [Custom tabs](https://docs.expo.dev/router/advanced/custom-tabs.md)
- [Custom navigators](https://docs.expo.dev/router/advanced/custom-navigators.md)
- [Stack Toolbar](https://docs.expo.dev/router/advanced/stack-toolbar.md)
- [Zoom transition](https://docs.expo.dev/router/advanced/zoom-transition.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Router settings

Learn how to configure layouts with static properties in Expo Router.

> **Warning:** `unstable_settings` currently do not work with [async routes](/router/web/async-routes.md) (development-only). This is why the feature is designated _unstable_.

### `initialRouteName`

When deep linking to a route, you may want to provide a user with a "back" button. The `initialRouteName` sets the default screen of the stack and should match a valid filename (without the extension).

`src`

 `app`

  `_layout.tsx`

  `index.tsx`

  `other.tsx`

```tsx
import { Stack } from 'expo-router';

export const unstable_settings = {
  // Ensure any route can link back to `/`
  initialRouteName: 'index',
};

export default function Layout() {
  return <Stack />;
}
```

Now deep linking directly to `/other` or reloading the page will continue to show the back arrow.

When using [array syntax](/router/advanced/shared-routes.md#arrays) `(foo,bar)` you can specify the name of a group in the `unstable_settings` object to target a particular segment.

```tsx
export const unstable_settings = {
  // Used for `(foo)`
  initialRouteName: 'first',
  // Used for `(bar)`
  bar: {
    initialRouteName: 'second',
  },
};
```

The `initialRouteName` is only used when deep-linking to a route. During app navigation, the route you are navigating to will be the initial route. You can disable this behavior using the `initial` prop on the `<Link />` component or by passing the option to the imperative APIs.

```js
// If this navigates to a new _layout, don't override the initial route
<Link href="/route" initial={false} />;

router.push('/route', { overrideInitialScreen: false });
```
