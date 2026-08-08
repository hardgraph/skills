---
modificationDate: July 27, 2026
title: How to use a standalone Expo module
description: Learn how to use a standalone module created with create-expo-module in your project by using a monorepo or publishing the package to npm.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/modules/use-standalone-expo-module-in-your-project/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/modules/use-standalone-expo-module-in-your-project/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Expo Modules API > Tutorials
Pages in this section:
- [Create a native module](https://docs.expo.dev/modules/native-module-tutorial.md)
- [Create a native view](https://docs.expo.dev/modules/native-view-tutorial.md)
- [Create an inline module](https://docs.expo.dev/modules/inline-modules-tutorial.md)
- [Generate module TS interface](https://docs.expo.dev/modules/type-generation-tutorial.md)
- [Create a module with a config plugin](https://docs.expo.dev/modules/config-plugin-and-native-module-tutorial.md)
- [How to use a standalone Expo module](https://docs.expo.dev/modules/use-standalone-expo-module-in-your-project.md) (this page)
- [Wrap third-party native libraries](https://docs.expo.dev/modules/third-party-library.md)
- [Integrate in an existing library](https://docs.expo.dev/modules/existing-library.md)
- [Additional platform support](https://docs.expo.dev/modules/additional-platform-support.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# How to use a standalone Expo module

Learn how to use a standalone module created with create-expo-module in your project by using a monorepo or publishing the package to npm.

**The recommended way to create an Expo module** in an existing project is described in the [Expo Modules API: Get Started](/modules/get-started.md) guide. This tutorial explains two additional methods for using a module created with `create-expo-module` in an existing project:

-   [Configure a monorepo](/modules/use-standalone-expo-module-in-your-project.md#use-a-monorepo)
-   [Publish the module to npm](/modules/use-standalone-expo-module-in-your-project.md#publish-the-module-to-npm)

These methods are useful if you still want to keep the module separate from the application or share it with other developers.

## Use a monorepo

Your project should use the following structure:

-   **apps**: A directory to store multiple projects, including React Native apps.
-   **packages**: A directory to keep different packages used by your apps.
-   **package.json**: This is the root package file that contains the Yarn workspaces configuration.

> To learn how to configure your project as a monorepo, check out the [Working with monorepos](/guides/monorepos.md) guide.

### Initialize a new module

Once you have set up the basic monorepo structure, create a new module using `create-expo-module` with the flag `--no-example` to skip creating the example app:

```sh
# npm
npx create-expo-module packages/expo-settings --no-example

# yarn
yarn create expo-module packages/expo-settings --no-example

# pnpm
pnpm create expo-module packages/expo-settings --no-example

# bun
bun create expo-module packages/expo-settings --no-example
```

### Set up a workspace dependency

Add your native module from **packages** to your apps' dependencies. Update the **package.json** file in each app inside the **apps** directory that will use your native module and add your native module to the existing entries of dependencies:

```json
{
  "dependencies": {
    ... 
    "expo-settings": "*"
    ... 
  }
}
```

### Run the module

Run one of your apps to ensure everything works. Then, start the TypeScript compiler in **packages/expo-settings** to watch for changes and rebuild the module's JavaScript:

```sh
# npm
cd packages/expo-settings
npm run build

# yarn
cd packages/expo-settings
yarn run build

# pnpm
cd packages/expo-settings
pnpm run build

# bun
cd packages/expo-settings
bun run build
```

Open another terminal window, select an app from the **apps** directory, and run the `prebuild` command with the `--clean` option. Repeat these steps for each app in your monorepo to use the new module.

```sh
# npm
npx expo prebuild --clean

# yarn
yarn expo prebuild --clean

# pnpm
pnpm expo prebuild --clean

# bun
bun expo prebuild --clean
```

Compile and run the app with the following command:

```sh
# npm
npx expo run:android
npx expo run:ios

# yarn
yarn expo run:android
yarn expo run:ios

# pnpm
pnpm expo run:android
pnpm expo run:ios

# bun
bun expo run:android
bun expo run:ios
```

You can now use the module in your app. To test it, edit the **src/app/index.tsx** file in your app and render the text message from the `expo-settings` module:

```tsx
import React from 'react';
import { Text, View } from 'react-native';
import * as Settings from 'expo-settings';

export default function TabOneScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>{Settings.hello()}</Text>
    </View>
  );
}
```

After this configuration, the app displays the text "Hello world! 👋".

## Publish the module to npm

You can publish the module on npm and install it as a dependency in your project by following the steps below.

### Initialize a new module

Start by creating a new module with `create-expo-module`. Follow the prompts carefully, as you will publish this library, and choose a unique name for your npm package.

```sh
# npm
npx create-expo-module expo-settings

# yarn
yarn create expo-module expo-settings

# pnpm
pnpm create expo-module expo-settings

# bun
bun create expo-module expo-settings
```

### Run the example project

Run one of your apps to ensure everything works. Then, start the TypeScript compiler in the root of your project to watch for changes and rebuild the module's JavaScript:

```sh
# npm
npm run build

# yarn
yarn run build

# pnpm
pnpm run build

# bun
bun run build
```

Open another terminal window, compile and run the example app:

```sh
# npm
cd example
npx expo run:android
npx expo run:ios

# yarn
cd example
yarn expo run:android
yarn expo run:ios

# pnpm
cd example
pnpm expo run:android
pnpm expo run:ios

# bun
cd example
bun expo run:android
bun expo run:ios
```

### Publish the package to npm

To publish your package to npm, you need an npm account. If you don't have one, create an account on [the npm website](https://www.npmjs.com/signup). After creating an account, log in by running the following command:

```sh
npm login
```

Navigate to the root of your module, then run the following command to publish it:

```sh
npm publish
```

Your module will now be published to npm and can be installed in other projects using `npm install`.

Apart from publishing your module to npm, you can use it in your project in the following ways:

-   **Create a tarball**: Use `npm pack` to create a tarball of your module, then install it in your project by running `npm install /path/to/tarball`. This method is helpful for testing your module locally before publishing it or sharing it with others who don't have access to the npm registry.
-   **Run a local npm registry**: Use a tool such as [Verdaccio](https://verdaccio.org/) to host a local npm registry. You can install your module from this registry, which is useful for managing internal packages within a company or organization.
-   **Publish a private package**: [Use a private registry with EAS Build](/build-reference/private-npm-packages.md) to manage private modules securely.

### Test the published module

To test the published module in a new project, create a new app and install the module as a dependency by running the following command:

```sh
# npm
npx create-expo-app@latest my-app --template default@sdk-57
cd my-app
npx expo install expo-settings

# yarn
yarn create expo-app my-app --template default@sdk-57
cd my-app
yarn expo install expo-settings

# pnpm
pnpm create expo-app my-app --template default@sdk-57
cd my-app
pnpm expo install expo-settings

# bun
bun create expo my-app --template default@sdk-57
cd my-app
bun expo install expo-settings
```

You can now use the module in your app! To test it, edit **src/app/index.tsx** and render the text message from **expo-settings**.

```tsx
import React from 'react';
import * as Settings from 'expo-settings';
import { Text, View } from 'react-native';

export default function TabOneScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>{Settings.hello()}</Text>
    </View>
  );
}
```

Finally, prebuild your project and run the app by executing the following commands:

```sh
# npm
npx expo prebuild --clean
npx expo run:android
npx expo run:ios

# yarn
yarn expo prebuild --clean
yarn expo run:android
yarn expo run:ios

# pnpm
pnpm expo prebuild --clean
pnpm expo run:android
pnpm expo run:ios

# bun
bun expo prebuild --clean
bun expo run:android
bun expo run:ios
```

After this configuration, you see the text "Hello world! 👋" displayed in the app.

## Next steps

[Wrap third-party native libraries](/modules/third-party-library.md) — Learn how to wrap third-party native libraries in an Expo module.

[Tutorial: Creating a native module](/modules/native-module-tutorial.md) — A tutorial on creating a native module that persists settings with Expo Modules API.
