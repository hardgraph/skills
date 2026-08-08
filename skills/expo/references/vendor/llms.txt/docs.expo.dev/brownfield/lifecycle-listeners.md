---
modificationDate: June 03, 2026
title: Configuring lifecycle listeners
description: Learn about the mechanism that allows the Expo Modules API to hook into the lifecycle of your app.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/brownfield/lifecycle-listeners/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/brownfield/lifecycle-listeners/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Existing native apps
Pages in this section:
- [Overview](https://docs.expo.dev/brownfield/overview.md)
- [Isolated approach](https://docs.expo.dev/brownfield/isolated-approach.md)
- [Integrated approach](https://docs.expo.dev/brownfield/integrated-approach.md)
- [Lifecycle listeners](https://docs.expo.dev/brownfield/lifecycle-listeners.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Configuring lifecycle listeners

Learn about the mechanism that allows the Expo Modules API to hook into the lifecycle of your app.

Some Expo libraries need to handle system events such as deep links, push notifications, and configuration changes by implementing `Activity`/`Application` or `AppDelegate` lifecycle callbacks.

The Expo Modules API provides an easy way to manage such callbacks:

-   Android `ApplicationLifecycleDispatcher` and `ReactActivityHandler` forward `Application` and `Activity` lifecycle events to registered listeners. Modules can provide `ReactActivityLifecycleListener` and `ApplicationLifecycleListener` implementations through a `Package` class to register callbacks.
-   iOS `ExpoAppDelegate` forwards `AppDelegate` calls to registered subscribers. Modules can provide an `ExpoAppDelegateSubscriber` implementation to register callbacks.

Using these mechanisms allows modules to register behavior without requiring you to edit native entry points repeatedly.

## Configure your native project

### Android

To integrate `Application` lifecycle listeners on Android, forward the `onCreate()` and `onConfigurationChanged()` calls from your `Application` class to `ApplicationLifecycleDispatcher`:

### iOS

To integrate `AppDelegate` subscribers on iOS, forward the relevant calls to `ExpoAppDelegateSubscriberManager` in your existing `AppDelegate` implementation so that subscribers can respond to them:

Alternatively, if your `AppDelegate` doesn't already extend another class, you can simplify the setup by inheriting from `ExpoAppDelegate`, which handles the forwarding automatically:

> **Note:** Not all `UIApplicationDelegate` methods that could cause significant side effects are supported. See the Expo source (**ExpoAppDelegate.swift**) for the full list of forwarded methods if you need to rely on a specific delegate.

## Test your integration

To test if the callbacks are working correctly, install a module that relies on them. Install `expo-linking`, which uses lifecycle listeners to handle deep links:

```sh
# npm
npx expo install expo-linking

# yarn
yarn expo install expo-linking

# pnpm
pnpm expo install expo-linking

# bun
bun expo install expo-linking
```

Add a listener for deep links in your code and observe the console when opening a deep link:

```jsx
import * as Linking from 'expo-linking';
import { useEffect } from 'react';

useEffect(() => {
  const listener = Linking.addEventListener('url', ({ url }) => {
    console.log('Received deep link:', url);
  });

  return listener.remove;
}, []);
```

Run the following command to open a deep link to your app:

```sh
# npm
npx uri-scheme open com.example.app://somepath/details --android
npx uri-scheme open myapp://somepath/details --ios

# yarn
yarn dlx uri-scheme open com.example.app://somepath/details --android
yarn dlx uri-scheme open myapp://somepath/details --ios

# pnpm
pnpm dlx uri-scheme open com.example.app://somepath/details --android
pnpm dlx uri-scheme open myapp://somepath/details --ios

# bun
bunx uri-scheme open com.example.app://somepath/details --android
bunx uri-scheme open myapp://somepath/details --ios
```
