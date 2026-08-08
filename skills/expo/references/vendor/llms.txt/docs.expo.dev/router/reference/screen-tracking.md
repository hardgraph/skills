---
modificationDate: May 15, 2026
title: Screen tracking for analytics
description: Learn how to enable screen tracking for analytics when using Expo Router.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/router/reference/screen-tracking/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/router/reference/screen-tracking/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Screen tracking for analytics](https://docs.expo.dev/router/reference/screen-tracking.md) (this page)
- [Top-level src directory](https://docs.expo.dev/router/reference/src-directory.md)
- [Testing](https://docs.expo.dev/router/reference/testing.md)
- [Troubleshooting](https://docs.expo.dev/router/reference/troubleshooting.md)
- [Reserved paths](https://docs.expo.dev/router/reference/reserved-paths.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Screen tracking for analytics

Learn how to enable screen tracking for analytics when using Expo Router.

Unlike React Navigation, Expo Router always has access to a URL. This means screen tracking is as easy as the web.

1.  Create a higher-order component that observes the currently selected URL
2.  Track the URL in your analytics provider

`src`

 `app`

  `_layout.tsx`

```tsx
import { useEffect } from 'react';
import { usePathname, useGlobalSearchParams, Slot } from 'expo-router';

export default function Layout() {
  const pathname = usePathname();
  const params = useGlobalSearchParams();

  // Track the location in your analytics provider here.
  useEffect(() => {
    analytics.track({ pathname, params });
  }, [pathname, params]);

  // Export all the children routes in the most basic way.
  return <Slot />;
}
```

Now when the user changes routes, the analytics provider will be notified.

## Migrating from React Navigation

React Navigation's [screen tracking guide](https://reactnavigation.org/docs/screen-tracking/) cannot make the same assumptions about the navigation state that Expo Router can. As a result, the implementation requires the use of `onReady` and `onStateChange` callbacks. Avoid using these methods if possible as the root `<NavigationContainer />` is not directly exposed and allows cascading in Expo Router.
