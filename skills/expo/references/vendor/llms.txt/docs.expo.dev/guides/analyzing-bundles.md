---
modificationDate: June 03, 2026
title: Analyzing JavaScript bundles with Expo Atlas and Lighthouse
description: Learn about improving the production JavaScript bundle size of Expo apps and websites with Expo Atlas and Lighthouse.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/analyzing-bundles/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/analyzing-bundles/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Bundling
Pages in this section:
- [Bundle with Metro](https://docs.expo.dev/guides/customizing-metro.md)
- [Analyzing JavaScript bundles](https://docs.expo.dev/guides/analyzing-bundles.md) (this page)
- [Tree shaking](https://docs.expo.dev/guides/tree-shaking.md)
- [Minification](https://docs.expo.dev/guides/minify.md)
- [Why Metro?](https://docs.expo.dev/guides/why-metro.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Analyzing JavaScript bundles with Expo Atlas and Lighthouse

Learn about improving the production JavaScript bundle size of Expo apps and websites with Expo Atlas and Lighthouse.

Bundle performance varies for different platforms. For example, web browsers don't support precompiled bytecode, so the JavaScript bundle size is important for improving startup time and performance. The smaller the bundle, the faster it can be downloaded and parsed.

## Analyzing bundle size with Expo Atlas

The libraries used in a project influence the size of the production JavaScript bundle. You can use [Expo Atlas](https://github.com/expo/expo-atlas#readme) to visualize the production bundle and identify which libraries contribute to the bundle size.

### Using Atlas with `npx expo start`

You can use Expo Atlas with the local development server. This method allows Atlas to update whenever you change any code in your project.

Once your app is running using the local development server on Android, iOS, and/or web, you can open Atlas through the [dev tools plugin menu](/debugging/devtools-plugins.md#using-a-dev-tools-plugin) using Shift + M.

```sh
# npm
EXPO_ATLAS=true npx expo start

# yarn
EXPO_ATLAS=true yarn expo start

# pnpm
EXPO_ATLAS=true pnpm expo start

# bun
EXPO_ATLAS=true bun expo start
```

#### Changing development mode to production

By default, Expo starts the local development server in [development mode](/workflow/development-mode.md#development-mode). Development mode disables some optimizations that are enabled in [production mode](/workflow/development-mode.md#production-mode). You can also start the local development server in production mode to get a more accurate representation of the production bundle size:

```sh
# npm
EXPO_ATLAS=true npx expo start --no-dev

# yarn
EXPO_ATLAS=true yarn expo start --no-dev

# pnpm
EXPO_ATLAS=true pnpm expo start --no-dev

# bun
EXPO_ATLAS=true bun expo start --no-dev
```

### Using Expo Atlas with `npx expo export`

You can also use Expo Atlas when generating a production bundle for your app or EAS Update. Atlas generates a **.expo/atlas.jsonl** file during export, which you can share and open without having access to the project.

```sh
# npm
EXPO_ATLAS=true npx expo export
npx expo-atlas .expo/atlas.jsonl

# yarn
EXPO_ATLAS=true yarn expo export
yarn dlx expo-atlas .expo/atlas.jsonl

# pnpm
EXPO_ATLAS=true pnpm expo export
pnpm dlx expo-atlas .expo/atlas.jsonl

# bun
EXPO_ATLAS=true bun expo export
bunx expo-atlas .expo/atlas.jsonl
```

You can also specify the platforms you want to analyze using the `--platform` option. Expo Atlas will gather the data for the exported platforms only.

### Analyzing transformed modules

Inside Atlas, you can hold ⌘ Cmd and click on a graph node to see the transformed module details. This feature helps you understand how a module is transformed by Babel, which modules it imports, and which modules imported it. This can be used to trace the origin of a module across the dependency graph.

## Analyzing bundle size with source-map-explorer

> Alternative method for **SDK 50 and earlier**.

If you are using SDK 50 or below, you can use the [`source-map-explorer`](https://www.npmjs.com/package/source-map-explorer) library to visualize and analyze the production JavaScript bundle.

To use source map explorer, run the following command to install it:

```sh
# npm
npm install --save-dev source-map-explorer

# yarn
yarn add --dev source-map-explorer

# pnpm
pnpm add --save-dev source-map-explorer

# bun
bun add --dev source-map-explorer
```

Add a script to **package.json** to run it. You might have to adjust the input path depending on the platform or SDK you are using. For brevity, the following example assumes the project is Expo SDK 50 and does not use Expo Router `server` output.

```json
{
  "scripts": {
    "analyze:web": "source-map-explorer 'dist/_expo/static/js/web/*.js' 'dist/_expo/static/js/web/*.js.map'",
    "analyze:ios": "source-map-explorer 'dist/_expo/static/js/ios/*.js' 'dist/_expo/static/js/ios/*.js.map'",
    "analyze:android": "source-map-explorer 'dist/_expo/static/js/android/*.js' 'dist/_expo/static/js/android/*.js.map'"
  }
}
```

If you are using the SDK 50 `server` output for web, then use the following to map web bundles:

```sh
# npm
npx source-map-explorer 'dist/client/_expo/static/js/web/*.js' 'dist/client/_expo/static/js/web/*.js.map'

# yarn
yarn dlx source-map-explorer 'dist/client/_expo/static/js/web/*.js' 'dist/client/_expo/static/js/web/*.js.map'

# pnpm
pnpm dlx source-map-explorer 'dist/client/_expo/static/js/web/*.js' 'dist/client/_expo/static/js/web/*.js.map'

# bun
bunx source-map-explorer 'dist/client/_expo/static/js/web/*.js' 'dist/client/_expo/static/js/web/*.js.map'
```

Web bundles are output to the **dist/client** subdirectory to prevent exposing server code to the client.

Export your production JavaScript bundle and include the `--source-maps` flag so that the source map explorer can read the source maps. For native apps using Hermes, you can use the `--no-bytecode` option to disable bytecode generation.

```sh
# npm
npx expo export --source-maps --platform web
npx expo export --source-maps --platform ios --no-bytecode

# yarn
yarn expo export --source-maps --platform web
yarn expo export --source-maps --platform ios --no-bytecode

# pnpm
pnpm expo export --source-maps --platform web
pnpm expo export --source-maps --platform ios --no-bytecode

# bun
bun expo export --source-maps --platform web
bun expo export --source-maps --platform ios --no-bytecode
```

This command shows the JavaScript bundle and source map paths in the output. In the next step, you will pass these paths to the source map explorer.

> Avoid publishing source maps to production as they can cause both security issues and performance issues (a browser will download the large maps).

Run the script to analyze your bundle:

```sh
# npm
npm run analyze:web

# yarn
yarn run analyze:web

# pnpm
pnpm run analyze:web

# bun
bun run analyze:web
```

On running this command, you might see the following error:

```text
You must provide the URL of lib/mappings.wasm by calling SourceMapConsumer.initialize({ 'lib/mappings.wasm': ... }) before using SourceMapConsumer
```

This is probably due to a [known issue](https://github.com/danvk/source-map-explorer/issues/247) in `source-map-explorer` in Node.js 18 and above. To resolve this, set the environment variable `NODE_OPTIONS=--no-experimental-fetch` before running the analyze script.

You might encounter a warning such as `Unable to map 809/13787 bytes (5.87%)`. This occurs because source maps often exclude bundler runtime definitions (for example, `__d(() => {}, [])`). This value is consistent and not a reason for concern.

## Lighthouse

Lighthouse is a great way to see how fast, accessible, and performant your website is. You can test your project with the **Audit** tab in Chrome, or with the [Lighthouse CLI](https://github.com/GoogleChrome/lighthouse#using-the-node-cli).

After creating a production build with `npx expo export -p web` and serving it (using either `npx serve dist`, or production deployment, or custom server), run Lighthouse with the URL your site is hosted at.

```sh
# npm
npm install --global lighthouse
npx lighthouse <url> --view

# yarn
yarn global add lighthouse
yarn dlx lighthouse <url> --view

# pnpm
pnpm add --global lighthouse
pnpm dlx lighthouse <url> --view

# bun
bun add --global lighthouse
bunx lighthouse <url> --view
```
