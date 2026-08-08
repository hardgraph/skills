---
modificationDate: June 03, 2026
title: Use build cache providers
description: Accelerate local development by caching and reusing builds from a provider.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/cache-builds-remotely/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/cache-builds-remotely/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Build locally
Pages in this section:
- [Overview](https://docs.expo.dev/guides/local-app-overview.md)
- [Development](https://docs.expo.dev/guides/local-app-development.md)
- [Release](https://docs.expo.dev/guides/local-app-production.md)
- [Cache builds remotely](https://docs.expo.dev/guides/cache-builds-remotely.md) (this page)
- [Precompiled Expo Modules](https://docs.expo.dev/guides/prebuilt-expo-modules.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Use build cache providers

Accelerate local development by caching and reusing builds from a provider.

Build caching is a feature that speeds up `npx expo run:[android|ios]` by caching builds remotely, based on the project [fingerprint](/versions/latest/sdk/fingerprint.md). When you run `npx expo run:[android|ios]`, it checks if a build with a matching fingerprint exists, then downloads and launches it rather than compiling it again. Otherwise, the project is compiled as usual and then the resulting binary is uploaded to the remote cache for future runs.

## Using EAS as a build provider

To use the EAS Build provider plugin, start by installing the `eas-build-cache-provider` package as a developer dependency:

#### macOS/Linux

```sh
# npm
npx expo install eas-build-cache-provider --dev

# yarn
yarn expo install eas-build-cache-provider --dev

# pnpm
pnpm expo install eas-build-cache-provider --dev

# bun
bun expo install eas-build-cache-provider --dev
```

#### Windows

```sh
# npm
npx expo install eas-build-cache-provider "--" --dev

# yarn
yarn expo install eas-build-cache-provider "--" --dev

# pnpm
pnpm expo install eas-build-cache-provider "--" --dev

# bun
bun expo install eas-build-cache-provider "--" --dev
```

Then, update your **app.json** to include the `buildCacheProvider` property and its provider:

```json
{
  "expo": {
    "buildCacheProvider": "eas"
    ... 
  }
}
```

You can roll your own cache provider by exporting a plugin that implements the following methods:

```ts
type BuildCacheProviderPlugin<T = any> = {
  /**
   * Try to fetch an existing build. Return its URL or null if missing.
   */
  resolveBuildCache(props: ResolveBuildCacheProps, options: T): Promise<string | null>;

  /**
   * Upload a new build binary. Return its URL or null on failure.
   */
  uploadBuildCache(props: UploadBuildCacheProps, options: T): Promise<string | null>;

  /**
   * (Optional) Customize the fingerprint hash algorithm.
   */
  calculateFingerprintHash?: (
    props: CalculateFingerprintHashProps,
    options: T
  ) => Promise<string | null>;
};

type ResolveBuildCacheProps = {
  projectRoot: string;
  platform: 'android' | 'ios';
  runOptions: RunOptions;
  fingerprintHash: string;
};
type UploadBuildCacheProps = {
  projectRoot: string;
  buildPath: string;
  runOptions: RunOptions;
  fingerprintHash: string;
  platform: 'android' | 'ios';
};
type CalculateFingerprintHashProps = {
  projectRoot: string;
  platform: 'android' | 'ios';
  runOptions: RunOptions;
};
```

A reference implementation using GitHub Releases to cache builds can be found in the [Build Cache Provider Example](https://github.com/expo/examples/tree/master/with-github-remote-build-cache-provider).

## Limitations

The build cache provider is consulted only by local `npx expo run:[android|ios]` commands. Builds triggered with `eas build` are not affected. They always produce fresh artifacts and never invoke the cache provider plugin.

Some local builds are also skipped:

-   **iOS physical device builds.** A device build is only valid on devices that match its provisioning profile, so reusing one across machines or devices is unsafe. `npx expo run:ios` therefore skips both the cache lookup and the post-build upload when the target is a physical device. Only iOS Simulator builds participate in caching.

If you're using `appVersionSource: "remote"` in **eas.json**, note that the `versionCode` (Android) and `buildNumber` (iOS) live on EAS servers rather than in your project source, so they are not part of the fingerprint inputs. This is fine for normal development iteration. Local `npx expo run:*` invocations don't auto-increment those values, but be aware that a cached artifact retains whatever build number was embedded when it was originally built.

## Creating a custom build provider

Start by creating a **provider** directory for writing the provider plugin in TypeScript and add a **provider.plugin.js** file in the project root, which will be the plugin's entry point.

### Create a `provider/tsconfig.json` file

```json
{
  "extends": "expo-module-scripts/tsconfig.plugin",
  "compilerOptions": {
    "outDir": "build",
    "rootDir": "src"
  },
  "include": ["./src"],
  "exclude": ["**/__mocks__/*", "**/__tests__/*"]
}
```

### Create a `provider/src/index.ts` file for your plugin

```ts
import { type BuildCacheProviderPlugin } from '@expo/config';

const plugin: BuildCacheProviderPlugin = {
  resolveBuildCache: async () => {
    console.log('Searching for remote builds...');
    return null;
  },
  uploadBuildCache: async () => {
    console.log('Uploading build to remote...');
    return null;
  },
};

export default plugin;
```

### Create an `provider.plugin.js` file in the root directory

```js
// This file configures the entry file for your plugin.
module.exports = require('./provider/build');
```

### Build your provider plugin

At the root of your project, run `npm run build provider` to start the TypeScript compiler in watch mode.

### Configure your example project to use your plugin by adding the following line to the `example/app.json` file:

```json
{
  "expo": {
    ... 
    "buildCacheProvider": {
      "plugin": "./provider.plugin.js"
    }
  }
}
```

### Test your provider

When you run the `npx expo run` command inside your **example** directory, you should see your plugin's console statements in the logs.

```sh
cd example
npx expo run:android
npx expo run:ios
```

That's it! You now have a remote build cache provider to speed up your builds.

### Passing custom options

To inject custom options to your plugin you can use the `options` field and it will be forwarded as the second parameter of your custom functions. To do so modify the `buildCacheProvider` field in **example/app.json** as shown below:

```json
{
  "expo": {
    ... 
    "buildCacheProvider": {
      "plugin": "./provider.plugin.js",
      "options": {
        "myCustomKey": "XXX-XXX-XXX"
      }
    }
  }
}
```
