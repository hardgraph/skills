---
modificationDate: August 05, 2024
title: Clear bundler caches on Windows
description: Learn how to clear the bundler cache when using Yarn or npm with Expo CLI or React Native CLI on Windows.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/troubleshooting/clear-cache-windows/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/troubleshooting/clear-cache-windows/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > More > Troubleshooting
Pages in this section:
- [Overview](https://docs.expo.dev/troubleshooting/overview.md)
- ["Application has not been registered" error](https://docs.expo.dev/troubleshooting/application-has-not-been-registered.md)
- [Clear bundler caches on macOS and Linux](https://docs.expo.dev/troubleshooting/clear-cache-macos-linux.md)
- [Clear bundler caches on Windows](https://docs.expo.dev/troubleshooting/clear-cache-windows.md) (this page)
- ["React Native version mismatch" errors](https://docs.expo.dev/troubleshooting/react-native-version-mismatch.md)
- [Proxies](https://docs.expo.dev/troubleshooting/proxies.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Clear bundler caches on Windows

Learn how to clear the bundler cache when using Yarn or npm with Expo CLI or React Native CLI on Windows.

> Need to clear development caches on macOS or Linux? [Find the relevant commands here.](/troubleshooting/clear-cache-macos-linux.md)

There are a number of different caches associated with your project that can prevent your project from running as intended. Clearing a cache sometimes can help you work around issues related to stale or corrupt data and is often useful when troubleshooting and debugging.

## Expo CLI and Yarn

```sh
rm -rf node_modules
yarn cache clean
yarn
watchman watch-del-all
del %localappdata%Temphaste-map-*
del %localappdata%Tempmetro-cache
npx expo start --clear
```

## Expo CLI and npm

```sh
rm -rf node_modules
npm cache clean --force
npm install
watchman watch-del-all
del %localappdata%Temphaste-map-*
del %localappdata%Tempmetro-cache
npx expo start --clear
```

## React Native CLI and Yarn

```sh
rm -rf node_modules
yarn cache clean
yarn
watchman watch-del-all
del %localappdata%Temphaste-map-*
del %localappdata%Tempmetro-cache
yarn start -- --reset-cache
```

## React Native CLI and npm

```sh
rm -rf node_modules
npm cache clean --force
npm install
watchman watch-del-all
del %localappdata%Temphaste-map-*
del %localappdata%Tempmetro-cache
npm start -- --reset-cache
```

## What these commands are doing

It is a good habit to understand commands you find on the internet before you run them. We explain each command below for Expo CLI, npm, and Yarn, but the corresponding commands React Native CLI have the same behavior.

| Command | Description |
| --- | --- |
| `del node_modules` | Clear all of the dependencies of your project |
| `yarn cache clean` | Clear the global Yarn cache |
| `npm cache clean --force` | Clear the global npm cache |
| `yarn`/`npm install` | Reinstall all dependencies |
| `watchman watch-del-all` | Reset the `watchman` file watcher |
| `del %localappdata%\Temp/<cache>` | Clear the given packager/bundler cache file or directory |
| `npx expo start --clear` | Restart the development server and clear the JavaScript transformation caches |
