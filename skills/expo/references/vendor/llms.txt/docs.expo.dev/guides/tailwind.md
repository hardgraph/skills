---
modificationDate: July 22, 2026
title: Tailwind CSS
description: Learn how to configure and use Tailwind CSS in your Expo project.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/tailwind/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/tailwind/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Web
Pages in this section:
- [Develop websites](https://docs.expo.dev/workflow/web.md)
- [Publish websites](https://docs.expo.dev/guides/publishing-websites.md)
- [DOM components](https://docs.expo.dev/guides/dom-components.md)
- [React Server Components](https://docs.expo.dev/guides/server-components.md)
- [Testing RSC](https://docs.expo.dev/guides/testing-rsc.md)
- [Progressive web apps](https://docs.expo.dev/guides/progressive-web-apps.md)
- [Tailwind CSS](https://docs.expo.dev/guides/tailwind.md) (this page)
- [Local HTTPS development](https://docs.expo.dev/guides/local-https-development.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Tailwind CSS

Learn how to configure and use Tailwind CSS in your Expo project.

> Standard Tailwind CSS supports only web platform. For universal support, use a library such as [NativeWind](https://www.nativewind.dev/) or [Uniwind](https://uniwind.dev/), which allow creating styled React Native components with Tailwind CSS.

[Tailwind CSS](https://tailwindcss.com/) is a utility-first CSS framework and can be used with Metro for web projects. This guide explains how to configure your Expo project to use the framework.

#### Prerequisites

##### A project using Metro for web

Ensure your project is using Metro for web. Verify this by checking that `web.bundler` is set to `metro` in **app.json**:

```json
{
  "expo": {
    "web": {
      "bundler": "metro"
    }
  }
}
```

The following files will be modified to set the Tailwind CSS configuration:

`app.json`

`package.json`

`global.css`

`index.js`

## Configuration

Configure Tailwind CSS in your Expo project according to the [Tailwind PostCSS documentation](https://tailwindcss.com/docs/installation/using-postcss).

#### v3

Install `tailwindcss` and its required peer dependencies. Then, run the initialization command to create **tailwind.config.js** and **post.config.js** files in the root of your project.

```sh
# npm
npx expo install tailwindcss@3 postcss autoprefixer --dev
npx tailwindcss init -p

# yarn
yarn expo install tailwindcss@3 postcss autoprefixer --dev
yarn dlx tailwindcss init -p

# pnpm
pnpm expo install tailwindcss@3 postcss autoprefixer --dev
pnpm dlx tailwindcss init -p

# bun
bun expo install tailwindcss@3 postcss autoprefixer --dev
bunx tailwindcss init -p
```

Add paths to all of your template files inside **tailwind.config.js**.

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    // Ensure this points to your source code
    './src/app/**/*.{js,tsx,ts,jsx}',
    // If you use a `src` directory, add: './src/**/*.{js,tsx,ts,jsx}'
    // Do the same with `components`, `hooks`, `styles`, or any other top-level directories
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

> If you are using Expo Router, consider using a root **src** directory to simplify this step. Learn more about [top-level src directory](/router/reference/src-directory.md).

Create a **global.css** file in the root of your project and directives for each of Tailwind's layers:

```css
/* This file adds the requisite utility classes for Tailwind to work. */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Import the **global.css** file in your **src/app/_layout.tsx** (if using Expo Router) or **index.js** file:

```tsx
import '../../global.css';
```

```tsx
// Import the global.css file in the index.js file:
import './global.css';
```

> If you are using [DOM components](/guides/dom-components.md), add this file import to each module using the `"use dom"` directive since they don't share globals.

> Always import global CSS in your root **_layout.tsx**, not in a nested layout. Expo Router traverses the dependency graph starting from the root layout. Importing CSS in a nested layout (for example, **app/blog/_layout.tsx**) causes **node_modules** CSS to load before your custom styles, which can break your intended style order.

You now start your project and use Tailwind CSS classes in your components.

```sh
# npm
npx expo start

# yarn
yarn expo start

# pnpm
pnpm expo start

# bun
bun expo start
```

#### v4

Install `tailwindcss` and its required peer dependencies:

```sh
# npm
npx expo install tailwindcss @tailwindcss/postcss postcss --dev

# yarn
yarn expo install tailwindcss @tailwindcss/postcss postcss --dev

# pnpm
pnpm expo install tailwindcss @tailwindcss/postcss postcss --dev

# bun
bun expo install tailwindcss @tailwindcss/postcss postcss --dev
```

Add Tailwind to your PostCSS configuration

```js
export default {
  plugins: {
    '@tailwindcss/postcss': {},
  },
};
```

Create a global CSS file that imports Tailwind CSS.

You can choose any name for this file. Using **global.css** is common practice.

```css
@import 'tailwindcss';
```

Import your CSS file in your **src/app/_layout.tsx** (if using Expo Router) or **index.js** file:

```tsx
// If using Expo Router, import your CSS file in the src/app/_layout.tsx file
import '../../global.css';
```

```tsx
// Otherwise import your CSS file in the index.js file:
import './global.css';
```

> If you are using [DOM components](/guides/dom-components.md), add this file import to each module using the `"use dom"` directive since they don't share globals.

> Always import global CSS in your root **_layout.tsx** and not in a nested layout. Expo Router traverses the dependency graph starting from the root layout. Importing CSS in a nested layout (for example, **app/blog/_layout.tsx**) causes **node_modules** CSS to load before your custom styles, which can break your intended style order.

You now start your project and use Tailwind CSS classes in your components.

```sh
# npm
npx expo start

# yarn
yarn expo start

# pnpm
pnpm expo start

# bun
bun expo start
```

## Usage

You can use Tailwind with React DOM elements as-is:

```tsx
export default function Index() {
  return (
    <div className="bg-slate-100 rounded-xl">
      <p className="text-lg font-medium">Welcome to Tailwind</p>
    </div>
  );
}
```

You can use the `{ $$css: true }` syntax to use Tailwind with React Native web elements:

```tsx
import { View, Text } from 'react-native';

export default function Index() {
  return (
    <View style={{ $$css: true, _: 'bg-slate-100 rounded-xl' }}>
      <Text style={{ $$css: true, _: 'text-lg font-medium' }}>Welcome to Tailwind</Text>
    </View>
  );
}
```

## Tailwind for Android and iOS

Tailwind does not support Android and iOS platforms. You can use a compatibility library such as [NativeWind](https://www.nativewind.dev/) or [Uniwind](https://uniwind.dev/) for universal support.

### Expo Skills for AI agents

If you use an AI agent, install [Expo Skills](/skills.md) to teach it universal Tailwind CSS setup:

[expo-tailwind-setup](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-tailwind-setup/SKILL.md) — Set up Tailwind CSS v4 in Expo with react-native-css and NativeWind v5 for universal styling.

## Alternative for Android and iOS

Alternatively, you can use [DOM components](/guides/dom-components.md) to render your Tailwind web code in a `WebView` on native.

```tsx
'use dom';

// Remember to import the global.css file in each DOM component.
import '../../global.css';

export default function Page() {
  return (
    <div className="bg-slate-100 rounded-xl">
      <p className="text-lg font-medium">Welcome to Tailwind</p>
    </div>
  );
}
```

## Troubleshooting

If you have a custom `config.cacheStores` in your **metro.config.js**, you need to extend the Expo superclass of `FileStore`:

```js
// Import the Expo superclass which has support for PostCSS.
const { FileStore } = require('@expo/metro-config/file-store');

config.cacheStores = [
  new FileStore({
    root: '/path/to/custom/cache',
  }),
];

module.exports = config;
```

Ensure you don't have CSS disabled in your **metro.config.js**:

```js
/** @type {import('expo/metro-config').MetroConfig} */
const config = getDefaultConfig(__dirname, {
  // Do not disable CSS support when using Tailwind.
  isCSSEnabled: true,
});
```
