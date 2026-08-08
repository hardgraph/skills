---
modificationDate: July 28, 2026
title: Install expo-dev-client in an existing React Native project
description: Learn how to install and configure expo-dev-client in your existing React Native project.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/bare/install-dev-builds-in-bare/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/bare/install-dev-builds-in-bare/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Existing React Native apps
Pages in this section:
- [Overview](https://docs.expo.dev/bare/overview.md)
- [Install Expo modules](https://docs.expo.dev/bare/installing-expo-modules.md)
- [Migrate to Expo CLI](https://docs.expo.dev/bare/using-expo-cli.md)
- [Install expo-updates](https://docs.expo.dev/bare/installing-updates.md)
- [Install expo-dev-client](https://docs.expo.dev/bare/install-dev-builds-in-bare.md) (this page)
- [Native project upgrade helper](https://docs.expo.dev/bare/upgrade.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Install expo-dev-client in an existing React Native project

Learn how to install and configure expo-dev-client in your existing React Native project.

The following guide explains how to install and configure `expo-dev-client` in an existing React Native project.

#### Do you need to create a new project?

If you're starting with a new project, create it using the `with-dev-client` template:

```sh
# npm
npx create-expo-app -e with-dev-client

# yarn
yarn create expo-app -e with-dev-client

# pnpm
pnpm create expo-app -e with-dev-client

# bun
bun create expo -e with-dev-client
```

#### Do you use Continuous Native Generation (CNG) in your project?

To use `expo-dev-client` in a project that uses [CNG](/workflow/continuous-native-generation.md), see [Create a development build](/develop/development-builds/introduction.md#how-would-you-like-to-build-your-development-build).

#### Prerequisites

##### Install and configure the expo package

If you created your project with `npx @react-native-community/cli@latest init` and do not have any other Expo libraries installed, you will need to [install Expo modules](/bare/installing-expo-modules.md) before proceeding.

## Install expo-dev-client

Add the `expo-dev-client` library to your **package.json**:

```sh
# npm
npx expo install expo-dev-client

# yarn
yarn expo install expo-dev-client

# pnpm
pnpm expo install expo-dev-client

# bun
bun expo install expo-dev-client
```

If your project has an **ios** directory on disk, run the following command to fully install the native code for `expo-dev-client`:

```sh
# npm
npx pod-install

# yarn
yarn dlx pod-install

# pnpm
pnpm dlx pod-install

# bun
bunx pod-install
```

If your project doesn't have an **ios** directory, you can skip this step.

## Configure deep links

Expo CLI uses a deep link to launch your project, and it's also useful if you use plan to [use `expo-dev-client` for launching preview updates](/eas-update/getting-started.md) if you have added a custom deep link scheme to your project.

If you haven't configured a `scheme` for your app yet to support deep linking, then use `uri-scheme` library to do this for you.

```sh
# npm
npx uri-scheme list
npx uri-scheme add your-scheme

# yarn
yarn dlx uri-scheme list
yarn dlx uri-scheme add your-scheme

# pnpm
pnpm dlx uri-scheme list
pnpm dlx uri-scheme add your-scheme

# bun
bunx uri-scheme list
bunx uri-scheme add your-scheme
```

For more information, see the [`uri-scheme` library](https://www.npmjs.com/package/uri-scheme).

## Build and install the app

Create a debug build of your app using the tools of your choice. For example, you can do this [locally with Expo CLI](/guides/local-app-development.md) or [in the cloud with EAS Build](/develop/development-builds/introduction.md?buildenv=build-with-eas#how-would-you-like-to-build-your-development-build).
