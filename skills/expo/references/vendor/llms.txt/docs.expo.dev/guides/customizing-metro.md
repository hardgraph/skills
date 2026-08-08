---
modificationDate: July 29, 2026
title: Metro bundler
description: Learn about different Metro bundler configurations that can be customized.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/customizing-metro/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/customizing-metro/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Bundling
Pages in this section:
- [Bundle with Metro](https://docs.expo.dev/guides/customizing-metro.md) (this page)
- [Analyzing JavaScript bundles](https://docs.expo.dev/guides/analyzing-bundles.md)
- [Tree shaking](https://docs.expo.dev/guides/tree-shaking.md)
- [Minification](https://docs.expo.dev/guides/minify.md)
- [Why Metro?](https://docs.expo.dev/guides/why-metro.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Metro bundler

Learn about different Metro bundler configurations that can be customized.

Expo CLI uses [Metro](https://metrobundler.dev/) during [`npx expo start`](/more/expo-cli.md#develop) and [`npx expo export`](/more/expo-cli.md#exporting) to bundle your JavaScript code and assets. Metro is built and optimized for React Native and used for large-scale applications such as Facebook and Instagram.

## Customizing

You can customize the Metro bundler by creating a **metro.config.js** file at the root of your project. This file should export a [Metro configuration](https://metrobundler.dev/docs/configuration/) that extends [`expo/metro-config`](https://github.com/expo/expo/tree/main/packages/@expo/metro-config). Import `expo/metro-config` instead of `@expo/metro-config` to ensure version consistency.

Run the following command to generate the template file:

```sh
# npm
npx expo customize metro.config.js

# yarn
yarn expo customize metro.config.js

# pnpm
pnpm expo customize metro.config.js

# bun
bun expo customize metro.config.js
```

The **metro.config.js** file looks as below:

```js
const { getDefaultConfig } = require('expo/metro-config');

const config = getDefaultConfig(__dirname);

module.exports = config;
```

See [**metro.config.js** documentation](/versions/latest/config/metro.md) for more information.

> Expo locks down some Metro configuration options to prevent projects from breaking. Not every option upstream Metro accepts can be customized or is supported. Expo also doesn't support loading Metro configs from YAML files (deprecated upstream) or any Metro configs located outside your repository.

## Assets

Metro resolves files as either source code or assets. Source code is JavaScript, TypeScript, JSON, and other files used by your application. [Assets](/develop/user-interface/assets.md) are images, fonts, and other files that should not be transformed by Metro. To accommodate large-scale codebases, Metro requires all extensions for both source code and assets to be explicitly defined before starting the bundler. This is done by adding the `resolver.sourceExts` and `resolver.assetExts` options to the Metro configuration. By default, the following extensions are included:

-   [`resolver.assetExts`](https://github.com/facebook/metro/blob/7028b7f51074f9ceef22258a8643d0f90de2388b/packages/metro-config/src/defaults/defaults.js#L15)
-   [`resolver.sourceExts`](https://github.com/facebook/metro/blob/7028b7f51074f9ceef22258a8643d0f90de2388b/packages/metro-config/src/defaults/defaults.js#L53)

### Adding more file extensions to `assetExts`

The most common customization is to include extra asset extensions to Metro.

In the **metro.config.js** file, add the file extension (without a leading `.`) to `resolver.assetExts` array:

```js
const { getDefaultConfig } = require('expo/metro-config');

const config = getDefaultConfig(__dirname);

config.resolver.assetExts.push(
  // Adds support for `.db` files for SQLite databases
  'db'
);

module.exports = config;
```

## Aliases

Sometimes you want an import to be redirected to another module or file. This is called an alias. Due to the way Metro bundles for multiple platforms simultaneously, we recommend using a custom resolver to handle aliases.

In the following example, we'll add an alias for `old-module` to `new-module`:

```js
const { getDefaultConfig } = require('expo/metro-config');

/** @type {import('expo/metro-config').MetroConfig} */
const config = getDefaultConfig(__dirname);

const ALIASES = {
  'old-module': 'new-module',
};

config.resolver.resolveRequest = (context, moduleName, platform) => {
  // Ensure you call the default resolver.
  return context.resolveRequest(
    context,
    // Use an alias if one exists.
    ALIASES[moduleName] ?? moduleName,
    platform
  );
};

module.exports = config;
```

If you want to only apply the alias on a certain platform, you can check the `platform` argument:

```js
config.resolver.resolveRequest = (context, moduleName, platform) => {
  if (platform === 'web') {
    // The alias will only be used when bundling for the web.
    return context.resolveRequest(context, ALIASES[moduleName] ?? moduleName, platform);
  }
  // Ensure you call the default resolver.
  return context.resolveRequest(context, moduleName, platform);
};
```

You will see the changes the next time you restart the dev server. Resolutions are never cached and do not need the `--clear` flag to update. If you use a transform-based system like `babel-plugin-module-resolver`, you will need to clear the cache to see changes applied.

[Customizing Metro resolution](/versions/latest/config/metro.md#custom-resolving) — Learn more about advanced Metro resolving in your project.

## File watching and crawling

From **SDK 56**, Expo's file map supports on-demand filesystem access. In practice, this means your `watchFolders` no longer need to include every module your app bundles, and projects that symlink to dependencies outside of the project root will now resolve correctly.

The on-demand filesystem is controlled by the [`experiments.onDemandFilesystem`](/versions/v56.0.0/config/app.md#ondemandfilesystem) flag in your [app config](/workflow/configuration.md) and is enabled by default.

[On-demand filesystem](/versions/v56.0.0/config/metro.md#on-demand-filesystem) — Learn how Expo's file map discovers, watches, and lazily resolves source files.

## Bundle splitting

Expo CLI automatically splits bundles based on async imports (web-only).

This technique can be used with Expo Router to automatically split the bundle based on route files in the **app** directory. It will only load the code required for the current route, and defer loading additional JavaScript until the user navigates to different pages. See [Async Routes](/router/web/async-routes.md) for more information.

## Tree shaking

[Tree shaking](/guides/tree-shaking.md) — Learn about how Expo CLI optimizes production JavaScript bundles.

## Minification

[Minifying JavaScript](/guides/minify.md) — Learn about customizing the JavaScript minification process in Expo CLI with Metro bundler.

## Web support

Expo CLI has support for bundling websites using Metro. This is the same bundler used for native apps, and it is designed to be universal across platforms. It is the recommended bundler for web projects.

### Expo webpack versus Expo Metro

If you previously wrote your website using the deprecated `@expo/webpack-adapter`, see the [migration guide](/router/migrate/from-expo-webpack.md) and [comparison chart](/router/migrate/from-expo-webpack.md#expo-cli).

### Adding web support to Metro

Modify your [app config](/workflow/configuration.md) to enable the feature using the `expo.web.bundler` field:

```json
{
  "expo": {
    "web": {
      "bundler": "metro"
    }
  }
}
```

#### Development

To start the development server run the following command:

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

Alternatively, press W in the Expo CLI terminal UI.

#### Static files

Expo's Metro implementation supports hosting static files from the dev server by putting them in the root **public/** directory. It is similar to many other web frameworks.

When exporting with `npx expo export`, the contents of the **public** directory are copied into the **dist/** directory. It means your app can expect to fetch these assets relative to the host URL. The most common example of this is the **public/favicon.ico** which is used by websites to render the tab icon.

You can overwrite the default **index.html** in Metro web by creating a **public/index.html** file in your project.

In the future, this will work universally across platforms with EAS Update hosting. Currently, the feature is web-only based on the static host used for the native app, for example, the legacy Expo service updates do not support this feature.

> Some paths such as `/assets` are reserved by Metro. Avoid placing files in **public/assets/** or other reserved paths. See [Reserved paths](/router/reference/reserved-paths.md) for the complete list.

## TypeScript

Expo's Metro config supports the `compilerOptions.paths` and `compilerOptions.baseUrl` fields in the project's **tsconfig.json** (or **jsconfig.json**) file. This enables absolute imports and aliases in the project. See [TypeScript](/guides/typescript.md) guide for more information.

This feature requires additional setup in [existing React Native projects](/bare/overview.md). See the [Metro setup guide](/versions/latest/config/metro.md#existing-react-native-apps) for more information.

## CSS

[Metro web CSS guide](/versions/latest/config/metro.md#css) — Learn how to use CSS in websites that are bundled with Expo CLI and Metro bundler.
