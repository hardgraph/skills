---
modificationDate: July 08, 2026
title: Troubleshooting EAS Observe
description: Solutions for common EAS Observe issues.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/observe/reference/troubleshooting/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/observe/reference/troubleshooting/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Observe > Reference
Pages in this section:
- [Metrics](https://docs.expo.dev/eas/observe/reference/metrics.md)
- [Troubleshooting](https://docs.expo.dev/eas/observe/reference/troubleshooting.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Troubleshooting EAS Observe

Solutions for common EAS Observe issues.

## Common issues

### Metrics not appearing in the dashboard

1.  Ensure you have created a **new build** after installing `expo-observe`. Metrics are only collected from builds that include the library.
2.  Check that you are viewing the correct project in the EAS dashboard.
3.  If testing in a debug build, ensure `dispatchInDebug` is set to `true` via `configure()`. See [Enable metrics in development](/eas/observe/configuration.md#enable-metrics-in-development).

### Time to first render not showing

Verify that your root layout is wrapped with the root HOC:

#### SDK 56 and later

```jsx
import { ObserveRoot } from 'expo-observe';

function RootLayout() {
  return (/* your layout */);
}

export default ObserveRoot.wrap(RootLayout);
```

#### SDK 55

```jsx
import { AppMetricsRoot } from 'expo-observe';

function RootLayout() {
  return (/* your layout */);
}

export default AppMetricsRoot.wrap(RootLayout);
```

### Time to interactive not showing

This metric requires manual instrumentation. Verify that:

#### SDK 56 and later

1.  You are calling `markInteractive()` (from `useObserve()`) after your splash screen is hidden.
2.  The call is actually being executed (add a `console.log` to verify).

#### SDK 55

1.  You are calling `AppMetrics.markInteractive()` after your splash screen is hidden.
2.  The call is actually being executed (add a `console.log` to verify).

#### Migrating from expo-eas-observe

If you were part of the private preview and previously used `expo-eas-observe`, follow these steps to migrate to `expo-observe`.

**Replace the package**

```sh
npx expo install expo-observe
npm uninstall expo-eas-observe
```

If you previously installed `expo-eas-client` as a separate dependency, you can remove it:

```sh
npm uninstall expo-eas-client
```

**Update imports**

```diff
- import AppMetrics from 'expo-eas-observe';
+ import { AppMetrics } from 'expo-observe';
```

**Replace manual `markFirstRender()` with the root HOC**

Instead of calling `markFirstRender()` manually, wrap your root layout with the root HOC for your SDK. This handles the measurement automatically.

Before:

```jsx
import { useEffect } from 'react';
import AppMetrics from 'expo-eas-observe';

export default function RootLayout() {
  useEffect(() => {
    AppMetrics.markFirstRender();
  }, []);

  return (/* your layout */);
}
```

After:

#### SDK 56 and later

```jsx
import { ObserveRoot } from 'expo-observe';

function RootLayout() {
  return (/* your layout */);
}

export default ObserveRoot.wrap(RootLayout);
```

#### SDK 55

```jsx
import { AppMetricsRoot } from 'expo-observe';

function RootLayout() {
  return (/* your layout */);
}

export default AppMetricsRoot.wrap(RootLayout);
```

**Create a new build**

After completing the migration, create a new build of your app:

```sh
eas build
```
