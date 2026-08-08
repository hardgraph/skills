---
modificationDate: June 30, 2026
title: Navigation in Expo and React Native apps
description: Learn about the recommended approach for integrating navigation in an Expo and React Native project.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/develop/app-navigation/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/develop/app-navigation/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > Develop
Pages in this section:
- [Tools for development](https://docs.expo.dev/develop/tools.md)
- [Navigation](https://docs.expo.dev/develop/app-navigation.md) (this page)
- [Database](https://docs.expo.dev/develop/database.md)
- [Authentication](https://docs.expo.dev/develop/authentication.md)
- [Unit testing](https://docs.expo.dev/develop/unit-testing.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Navigation in Expo and React Native apps

Learn about the recommended approach for integrating navigation in an Expo and React Native project.

The core React Native library does not include a built-in navigation solution, so you can choose a navigation library that best fits your needs. For Expo and React Native apps, it is generally a choice between [React Navigation](https://reactnavigation.org/) or [Expo Router](/router/introduction.md).

## Why React Native apps need a navigation library

React Native core includes basic UI components, touch handling, device APIs and networking, but excludes, among other things, storage, camera, maps, most device sensors, and **navigation**! These are intended to be covered by community libraries.

## React Navigation

React Navigation is a component-based navigation library widely used across the React Native ecosystem. It lets you compose stack, tab, and drawer navigators entirely in code so you can implement complex flows, custom transitions, and app-specific UX patterns.

The library offers platform-specific look-and-feel with smooth animations and gestures, unified mobile and web routing, automatic deep links, type routes with static configuration, and is highly customizable.

[React Navigation: Getting started](https://reactnavigation.org/docs/getting-started) — Learn how to get started with React Navigation.

## Expo Router (recommended for Expo projects)

Expo Router is a file-based routing library for Expo and React Native projects. By following the **app** directory convention, it turns files into routes and is integrated with Expo for [Expo CLI](/more/expo-cli.md) and bundling without additional setup. The library also adds features such as typed routes, dynamic routes, lazy bundling in development, static rendering for the web, and automatic deep linking.

New Expo projects created with `npx create-expo-app@latest --template default@sdk-57` include Expo Router by default.

[Introduction to Expo Router](/router/introduction.md) — Expo Router is an open-source routing library for Universal React Native applications built with Expo.

[Installation](/router/installation.md) — Learn how to quickly get started by creating a new project with Expo Router or adding the library to an existing project.

[Core concepts](/router/basics/core-concepts.md) — Learn about the core concepts of file-based routing in Expo.
