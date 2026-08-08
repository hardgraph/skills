---
modificationDate: July 22, 2026
title: Create a project
description: Learn how to create a new Expo project.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/get-started/create-a-project/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/get-started/create-a-project/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > Get started
Pages in this section:
- [Create a project](https://docs.expo.dev/get-started/create-a-project.md) (this page)
- [Set up your environment](https://docs.expo.dev/get-started/set-up-your-environment.md)
- [Start developing](https://docs.expo.dev/get-started/start-developing.md)
- [Next steps](https://docs.expo.dev/get-started/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Create a project

Learn how to create a new Expo project.

Expo is a React Native framework that makes developing Android and iOS apps easier. Our framework provides file-based routing, a standard library of native modules, and much more. Expo is open source with an active community on [GitHub](https://github.com/expo/expo) and [Discord](https://chat.expo.dev).

We also make [Expo Application Services (EAS)](https://expo.dev/eas), a set of services that complement the Expo framework in each step of the development process.

> **New to programming?** You can build your first Expo app by prompting an AI coding agent instead of writing code. Follow the [Build with AI tutorial](/tutorial/build-with-ai/introduction.md). It covers setup from scratch.

## System requirements

-   [Node.js (LTS)](https://nodejs.org/en/).
-   macOS, Windows (Powershell and [WSL 2](https://expo.fyi/wsl)), and Linux are supported.

## Start with a default project

We recommend starting with the default project created by [`create-expo-app`](/more/create-expo.md). The default project includes example code to help you get started.

To create a new project, run the following command:

```sh
# npm
npx create-expo-app@latest --template default@sdk-57

# yarn
yarn create expo-app --template default@sdk-57

# pnpm
pnpm create expo-app --template default@sdk-57

# bun
bun create expo --template default@sdk-57
```

> **Note:** During the SDK 57 transition period, `create-expo-app@latest` without the `--template` flag creates an SDK 54 project. If you plan to use Expo Go on a physical device, use an SDK 54 project. Otherwise, use `--template default@sdk-57` to create an SDK 57 project. You can also choose a different template by adding the [`--template` option](/more/create-expo.md#--template).

## Start from an example

Instead of the default project, you can start from one of the [Expo examples](https://github.com/expo/examples). These are small apps that each demonstrate a specific feature or integration, such as Expo Router, Expo Widgets, or a camera screen.

To browse the full list and pick one interactively, run `create-expo-app` with the [`--example`](/more/create-expo.md#--example) option and no name:

```sh
# npm
npx create-expo-app@latest --example

# yarn
yarn create expo-app --example

# pnpm
pnpm create expo-app --example

# bun
bun create expo --example
```

To create a known example directly, pass its name:

```sh
# npm
npx create-expo-app@latest --example with-widgets

# yarn
yarn create expo-app --example with-widgets

# pnpm
pnpm create expo-app --example with-widgets

# bun
bun create expo --example with-widgets
```

> The rest of the guides in this **"Get started"** section follows the default project. An example app may be organized differently but the concepts are the same.

## Expo Skills for AI agents

If you use an AI agent, install [Expo Skills](/skills.md) to teach it where files live in a new Expo project:

[expo-project-structure](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-project-structure/SKILL.md) — Folder structure for a new Expo app.

## Next step

You have a project. Now it's time to set up your development environment so that you can start developing.
