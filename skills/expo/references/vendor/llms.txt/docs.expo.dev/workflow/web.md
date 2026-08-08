---
modificationDate: June 03, 2026
title: Develop websites with Expo
description: Learn how to develop your app for the web so you can build a universal app.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/workflow/web/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/workflow/web/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Web
Pages in this section:
- [Develop websites](https://docs.expo.dev/workflow/web.md) (this page)
- [Publish websites](https://docs.expo.dev/guides/publishing-websites.md)
- [DOM components](https://docs.expo.dev/guides/dom-components.md)
- [React Server Components](https://docs.expo.dev/guides/server-components.md)
- [Testing RSC](https://docs.expo.dev/guides/testing-rsc.md)
- [Progressive web apps](https://docs.expo.dev/guides/progressive-web-apps.md)
- [Tailwind CSS](https://docs.expo.dev/guides/tailwind.md)
- [Local HTTPS development](https://docs.expo.dev/guides/local-https-development.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Develop websites with Expo

Learn how to develop your app for the web so you can build a universal app.

Expo has first-class support for building full-stack websites with React. Expo websites can be [statically rendered](/router/web/static-rendering.md) for SEO and performance, or client-rendered for a more app-like experience in the browser.

#### Universal

Render text on any platform with the `<Text>` component from [React Native for web](https://github.com/necolas/react-native-web).

```jsx
import { Text } from 'react-native';

export default function Page() {
  return <Text>Home page</Text>;
}
```

React Native for web (RNW) is a set of component libraries such as `<View>`, and `<Text>`, that wrap `react-dom` primitives such as `<div>`, `<p>`, and `<img>`. RNW is optional when developing for web since you can use React DOM directly, but we often recommended it when building across platforms as it maximizes code reuse.

> React Native for web is used to power the entire [X](https://x.com/) website.

#### Web-only

Alternatively, you can write web-only React DOM components such as `<div>`, `<p>`, and so on, however, these components won't render on native platforms.

```jsx
export default function Page() {
  return <p>Home page</p>;
}
```

Building web-only components is fully supported by Expo, however, you may want to organize your code to better support building for both web and native platforms simultaneously. Learn more in [platform-specific modules](/router/advanced/platform-specific-modules.md).

All of the libraries in the Expo SDK are built to support both browser and server rendering environments (when applicable). Libraries are also optimized for the individual platforms they target.

Development features like Fast Refresh, debugging, environment variables, and [bundling](/guides/customizing-metro.md) are also fully universal, enabling a unified developer experience. Expo CLI **automatically optimizes code** for individual platforms when you build for production, using techniques like [platform shaking](/guides/tree-shaking.md#platform-shaking).

## Getting started

### Install web dependencies

```sh
# npm
npx expo install react-dom react-native-web @expo/metro-runtime

# yarn
yarn expo install react-dom react-native-web @expo/metro-runtime

# pnpm
pnpm expo install react-dom react-native-web @expo/metro-runtime

# bun
bun expo install react-dom react-native-web @expo/metro-runtime
```

#### Not using the expo package in your app yet?

If you haven't added Expo to your React Native app yet, you can either [install Expo modules](/bare/installing-expo-modules.md) (recommended) or just install the `expo` package and configure the app entry file. This will allow you to target web, but it will not include support for the Expo SDK.

1.  Install [Expo CLI](/more/expo-cli.md) in your project:

```sh
# npm
npm install expo

# yarn
yarn add expo

# pnpm
pnpm add expo

# bun
bun add expo
```

2.  Modify the entry file to use [`registerRootComponent`](/versions/latest/sdk/expo.md#registerrootcomponentcomponent) instead of `AppRegistry.registerComponent`:

```diff
- import {AppRegistry} from 'react-native';
- import {name as appName} from './app.json';
+ import {registerRootComponent} from 'expo';
  import App from './App';
- AppRegistry.registerComponent(appName, () => App);
+ registerRootComponent(App);
```

### Start the dev server

You can now start the dev server and develop in the browser with:

```sh
# npm
npx expo start --web

# yarn
yarn expo start --web

# pnpm
pnpm expo start --web

# bun
bun expo start --web
```

The app can be exported as a production website with:

```sh
# npm
npx expo export --platform web

# yarn
yarn expo export --platform web

# pnpm
pnpm expo export --platform web

# bun
bun expo export --platform web
```

## Next

[File-based routing](/router/introduction.md) — Build routes and navigation with Expo Router.

[Static rendering and SEO](/router/web/static-rendering.md) — Render your website as static HTML with Expo Router to improve SEO and performance.

[Deploy instantly with EAS Hosting](/eas/hosting/get-started.md) — EAS Hosting is the best way to deploy your Expo Router and React Native web apps with support for custom domains, SSL, and more.

[Customizing the JavaScript bundler](/guides/customizing-metro.md) — Customize Metro bundler for your project.
