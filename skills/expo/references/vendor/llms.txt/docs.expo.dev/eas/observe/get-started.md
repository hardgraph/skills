---
modificationDate: July 28, 2026
title: Set up EAS Observe
description: Learn how to install EAS Observe and start collecting performance metrics from your production app.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/observe/get-started/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/observe/get-started/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Observe
Pages in this section:
- [Introduction](https://docs.expo.dev/eas/observe/introduction.md)
- [Get started](https://docs.expo.dev/eas/observe/get-started.md) (this page)
- [Dashboard](https://docs.expo.dev/eas/observe/dashboard.md)
- [EAS Update](https://docs.expo.dev/eas/observe/eas-update.md)
- [Events](https://docs.expo.dev/eas/observe/events.md)
- [Configuration](https://docs.expo.dev/eas/observe/configuration.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Set up EAS Observe

Learn how to install EAS Observe and start collecting performance metrics from your production app.

EAS Observe tracks your app's startup performance in production. This guide walks you through installing the library, setting up your app, and viewing your first metrics.

> EAS Observe is not available in Expo Go because it relies on the `expo-observe` native library. To use it, create a [development build](/develop/development-builds/introduction.md) or a production build.

## Prerequisites

#### Prerequisites

##### An Expo user account

EAS Observe is available to anyone with an Expo account. You can sign up at [expo.dev/signup](https://expo.dev/signup).

##### Expo SDK 55 or later

EAS Observe requires SDK 55 or later. Run `npx expo-doctor` to check your SDK version and `npx expo install --fix` to update dependencies.

##### An EAS project

Your app must be linked to an EAS project. Ensure `extra.eas.projectId` in your app config includes the project ID, or create one by running `eas init`.

## Install the library

Make sure you are using the latest version of `expo`, then install `expo-observe`:

```sh
# npm
npx expo install --fix
npx expo install expo-observe

# yarn
yarn expo install --fix
yarn expo install expo-observe

# pnpm
pnpm expo install --fix
pnpm expo install expo-observe

# bun
bun expo install --fix
bun expo install expo-observe
```

## Wrap your root layout

Wrap your root layout with `AppMetricsRoot` (SDK 55) or `ObserveRoot` (SDK 56 and later). This higher-order component (HOC) automatically measures [Time to First Render (TTR)](/eas/observe/reference/metrics.md#time-to-first-render-ttr) for you.

#### SDK 56 and later

```tsx
import { Stack } from 'expo-router';
import { ObserveRoot } from 'expo-observe';

function RootLayout() {
  return <Stack />;
}

export default ObserveRoot.wrap(RootLayout);
```

#### SDK 55

```tsx
import { Stack } from 'expo-router';
import { AppMetricsRoot } from 'expo-observe';

function RootLayout() {
  return <Stack />;
}

export default AppMetricsRoot.wrap(RootLayout);
```

## Mark interactive

Call `markInteractive()` when your app is fully ready for user interaction. This should be called **after** any initialization work behind the splash screen completes, such as:

-   Checking for updates
-   Authenticating the user
-   Fetching initial data
-   Animating the splash screen

#### SDK 56 and later

```tsx
import { Stack } from 'expo-router';
import * as SplashScreen from 'expo-splash-screen';
import { ObserveRoot, useObserve } from 'expo-observe';
import { useEffect, useState } from 'react';

// Keep the splash screen visible while we fetch resources
SplashScreen.preventAutoHideAsync();

function RootLayout() {
  const [isReady, setIsReady] = useState(false);
  const { markInteractive } = useObserve();

  useEffect(() => {
    async function prepare() {
      try {
        await authenticateUser();
        await fetchInitialData();
      } catch (e) {
        console.warn(e);
      } finally {
        setIsReady(true);
      }
    }

    prepare();
  }, []);

  useEffect(() => {
    if (isReady) {
      SplashScreen.hide();
      markInteractive();
    }
  }, [isReady, markInteractive]);

  if (!isReady) {
    return null;
  }

  return <Stack />;
}

export default ObserveRoot.wrap(RootLayout);
```

#### SDK 55

```tsx
import { Stack } from 'expo-router';
import * as SplashScreen from 'expo-splash-screen';
import { AppMetrics, AppMetricsRoot } from 'expo-observe';
import { useEffect, useState } from 'react';

// Keep the splash screen visible while we fetch resources
SplashScreen.preventAutoHideAsync();

function RootLayout() {
  const [isReady, setIsReady] = useState(false);

  useEffect(() => {
    async function prepare() {
      try {
        await authenticateUser();
        await fetchInitialData();
      } catch (e) {
        console.warn(e);
      } finally {
        setIsReady(true);
      }
    }

    prepare();
  }, []);

  useEffect(() => {
    if (isReady) {
      SplashScreen.hide();
      AppMetrics.markInteractive();
    }
  }, [isReady]);

  if (!isReady) {
    return null;
  }

  return <Stack />;
}

export default AppMetricsRoot.wrap(RootLayout);
```

> `markInteractive()` can safely be called multiple times per session, but only the first call records the measurement. If your app has multiple entry screens (for example, an onboarding flow, login flow, or deep link targets), call `markInteractive` on **every one of these screens**. If you only place it on one screen, [Time to Interactive (TTI)](/eas/observe/reference/metrics.md#time-to-interactive-tti) will not be recorded when the app opens via a deep link to a different screen.

## Create a new build

After installing `expo-observe` and adding the instrumentation, create a new build of your app:

```sh
eas build
```

> By default, metrics collected from debug builds are not dispatched. To test your integration in a debug build, see [Enable metrics in development](/eas/observe/configuration.md#enable-metrics-in-development).

## View your metrics

Open your project and open [**Observe** tab in EAS dashboard](https://expo.dev/accounts/%5Baccount%5D/projects/%5Bproject%5D/observe) to view metrics from your app.

For details on filtering, release comparison, and session investigation, see the [Dashboard guide](/eas/observe/dashboard.md).

You can also query metrics from the terminal using the EAS CLI:

-   `eas observe:versions`: Lists app versions along with their build IDs, update group IDs, and release dates. Useful for finding the version identifiers needed to filter the other commands.
-   `eas observe:metrics-summary`: Shows aggregated performance metric statistics (such as median, p75, and p95 values) grouped by app version. Use this to compare overall startup performance across releases.
-   `eas observe:metrics`: Shows individual performance metric events ordered by value, including session and device metadata. Use this to investigate specific slow sessions or outliers.
-   `eas observe:events`: Shows individual events emitted by your app via `Observe.logEvent`. See [User-defined events](/eas/observe/events.md) for details.

Run any of these commands with `--help` to see the available flags and arguments.
