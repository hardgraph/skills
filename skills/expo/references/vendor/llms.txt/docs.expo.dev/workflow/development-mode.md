---
modificationDate: June 03, 2026
title: Development and production modes
description: Learn how to run a project in development mode or production mode.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/workflow/development-mode/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/workflow/development-mode/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Reference
Pages in this section:
- [Work with monorepos](https://docs.expo.dev/guides/monorepos.md)
- [View logs](https://docs.expo.dev/workflow/logging.md)
- [Development and production modes](https://docs.expo.dev/workflow/development-mode.md) (this page)
- [Common development errors](https://docs.expo.dev/workflow/common-development-errors.md)
- [Android Studio Emulator](https://docs.expo.dev/workflow/android-studio-emulator.md)
- [iOS Simulator](https://docs.expo.dev/workflow/ios-simulator.md)
- [New Architecture](https://docs.expo.dev/guides/new-architecture.md)
- [React Compiler](https://docs.expo.dev/guides/react-compiler.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Development and production modes

Learn how to run a project in development mode or production mode.

Your project will always run in either **development** or **production** mode. By default, running your project locally with `npx expo start` runs it in development mode, whereas a published project (with `eas update`), or any standalone app, will run in production mode.

**Development mode** includes useful warnings and gives you access to tools that make development and debugging easier. **Production mode** [minifies your code](/guides/customizing-metro.md#minification) and better represents the performance your app will have on end users' devices. Let's look at each of these modes in more detail and learn how you can switch between them.

## Development mode

React Native includes some very useful tools for development: remote JavaScript debugging in Chrome, live reload, hot reloading, and an element inspector similar to the beloved inspector you use in Chrome. If you want to see how to use those tools, see [Debugging](/debugging/runtime-issues.md).

Development mode also performs validations while your app is running to give you warnings. For example, if you're using a deprecated property or if you forgot to pass a required property into a component. The video below shows the Element Inspector and Performance Monitor in action on both Android Emulator and iOS Simulator:

> **This comes at a cost. Your app runs slower in development mode.**  
> You can switch it on and off with the Expo CLI, see [Production mode](/workflow/development-mode.md#production-mode). When you switch it, close and re-open your app for the change to take effect. **Any time you are testing your app's performance, make sure to disable development mode**.

### View the developer menu

The menu gives access to a host of features that make development and debugging much easier. For more information on how to open it on Android and iOS, see [Developer menu](/debugging/tools.md#developer-menu).

## Production mode

Production mode is most useful for two things:

-   Testing your app's performance, as Development slows your app down considerably.
-   Catching bugs that only show up in production.

The easiest way to simulate how your project will run on end users' devices is with the command:

```sh
# npm
npx expo start --no-dev --minify

# yarn
yarn expo start --no-dev --minify

# pnpm
pnpm expo start --no-dev --minify

# bun
bun expo start --no-dev --minify
```

It runs the JavaScript of your app in production mode (which tells the Metro bundler to set the `__DEV__` environment variable to `false`, among a few other things). The `--minify` flag minifies your app. This flag also eliminates unnecessary data such as comments, formatting, and unused code. If you are getting an error or crash in your standalone app, running your project with this command can save you a lot of time in finding the root cause.

To completely compile your app for production see [Compiling Android](/more/expo-cli.md#compiling-android) and [Compiling iOS](/more/expo-cli.md#compiling-ios).
