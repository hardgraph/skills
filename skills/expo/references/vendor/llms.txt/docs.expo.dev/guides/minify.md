---
modificationDate: June 03, 2026
title: Minifying JavaScript
description: Learn about customizing the JavaScript minification process in Expo CLI with Metro bundler.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/minify/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/minify/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Bundling
Pages in this section:
- [Bundle with Metro](https://docs.expo.dev/guides/customizing-metro.md)
- [Analyzing JavaScript bundles](https://docs.expo.dev/guides/analyzing-bundles.md)
- [Tree shaking](https://docs.expo.dev/guides/tree-shaking.md)
- [Minification](https://docs.expo.dev/guides/minify.md) (this page)
- [Why Metro?](https://docs.expo.dev/guides/why-metro.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Minifying JavaScript

Learn about customizing the JavaScript minification process in Expo CLI with Metro bundler.

Minification is an optimization build step. It removes unnecessary characters such as collapses whitespace, removes comments, and shortens static operations, from the source code. This process reduces the final size and improves load times.

## Minification in Expo CLI

In Expo CLI, minification is performed on JavaScript files during the production export (when `npx expo export`, `npx expo export:embed`, `eas build`, and so on, commands run).

For example, consider following code snippet in a project:

```js
// This comment will be stripped
console.log('a' + ' ' + 'long' + ' string' + ' to ' + 'collapse');
```

This will be minified by the Expo CLI:

```js
console.log('a long string to collapse');
```

> **Tip:** Comments can be preserved by using the `/** @preserve */` directive.

The default minification of Expo CLI is sufficient for most projects. However, you can customize the minifier to optimize for speed or remove additional features like logs.

## Remove console logs

You can remove console logs from your production build. Use the `drop_console` option in the Terser minifier config.

```js
const { getDefaultConfig } = require('expo/metro-config');

const config = getDefaultConfig(__dirname);

config.transformer.minifierConfig = {
  compress: {
    // The option below removes all console logs statements in production.
    drop_console: true,
  },
};

module.exports = config;
```

You can also pass an array of console types to drop if you want to preserve certain logs. For example: `drop_console: ['log', 'info']` will remove `console.log` and `console.info` but preserve `console.warn` and `console.error`.

## Customizing the minifier

Different minifiers have tradeoffs between speed and compression. You can customize the minifier used by Expo CLI by modifying the **metro.config.js** file in your project.

### Terser

> [`terser`](https://github.com/terser/terser) is the default minifier ([Metro@0.73.0 changelog](https://github.com/facebook/metro/releases/tag/v0.73.0)).

To install Terser in a project, run the command:

```sh
# npm
npm install --save-dev metro-minify-terser

# yarn
yarn add --dev metro-minify-terser

# pnpm
pnpm add --save-dev metro-minify-terser

# bun
bun add --dev metro-minify-terser
```

Set Terser as a minifier with `transformer.minifierPath`, and pass in [`terser` options](https://github.com/terser/terser#compress-options) to `transformer.minifierConfig`.

```js
const { getDefaultConfig } = require('expo/metro-config');

const config = getDefaultConfig(__dirname);

config.transformer.minifierPath = 'metro-minify-terser';
config.transformer.minifierConfig = {
  // Terser options...
};

module.exports = config;
```

### Unsafe Terser options

For additional compression that may not work in all JavaScript engines, enable the [`unsafe` `compress` options](https://terser.org/docs/miscellaneous/#the-unsafe-compress-option):

```js
const { getDefaultConfig } = require('expo/metro-config');

const config = getDefaultConfig(__dirname);

config.transformer.minifierPath = 'metro-minify-terser';

config.transformer.minifierConfig = {
  compress: {
    // Enable all unsafe optimizations.
    unsafe: true,
    unsafe_arrows: true,
    unsafe_comps: true,
    unsafe_Function: true,
    unsafe_math: true,
    unsafe_symbols: true,
    unsafe_methods: true,
    unsafe_proto: true,
    unsafe_regexp: true,
    unsafe_undefined: true,
    unused: true,
  },
};

module.exports = config;
```

### esbuild

[`esbuild`](https://esbuild.github.io/) is used to minify exponentially faster than `uglify-es` and `terser`. For more information, see [`metro-minify-esbuild`](https://github.com/EvanBacon/metro-minify-esbuild#usage) usage.

### Uglify

You can use [`uglify-es`](https://github.com/mishoo/UglifyJS) by following the steps below:

To install Uglify in a project, run the command:

```sh
# npm
npm install --save-dev metro-minify-uglify

# yarn
yarn add --dev metro-minify-uglify

# pnpm
pnpm add --save-dev metro-minify-uglify

# bun
bun add --dev metro-minify-uglify
```

> Make sure the version of `metro-minify-uglify` matches the version of `metro` in your project.

Set Uglify as a minifier with `transformer.minifierPath`, and pass in [options](https://github.com/mishoo/UglifyJS#compress-options) to `transformer.minifierConfig`.

```js
const { getDefaultConfig } = require('expo/metro-config');

const config = getDefaultConfig(__dirname);

config.transformer.minifierPath = 'metro-minify-uglify';
config.transformer.minifierConfig = {
  // Options: https://github.com/mishoo/UglifyJS#compress-options
};

module.exports = config;
```
