---
modificationDate: June 03, 2026
title: Store data
description: Learn about different libraries available to store data in your Expo project.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/develop/user-interface/store-data/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/develop/user-interface/store-data/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > Develop > User interface
Pages in this section:
- [Splash screen and app icon](https://docs.expo.dev/develop/user-interface/splash-screen-and-app-icon.md)
- [Safe areas](https://docs.expo.dev/develop/user-interface/safe-areas.md)
- [System bars](https://docs.expo.dev/develop/user-interface/system-bars.md)
- [Fonts](https://docs.expo.dev/develop/user-interface/fonts.md)
- [Assets](https://docs.expo.dev/develop/user-interface/assets.md)
- [Color themes](https://docs.expo.dev/develop/user-interface/color-themes.md)
- [Animation](https://docs.expo.dev/develop/user-interface/animation.md)
- [Store data](https://docs.expo.dev/develop/user-interface/store-data.md) (this page)
- [Next steps](https://docs.expo.dev/develop/user-interface/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Store data

Learn about different libraries available to store data in your Expo project.

Storing data can be essential to the features implemented in your mobile app. There are different ways to save data in your Expo project depending on the type of data you want to store and the security requirements of your app. This page lists a variety of libraries to help you decide which solution is best for your project.

## Expo SecureStore

`expo-secure-store` provides a way to encrypt and securely store key-value pairs locally on the device. It's intended for small values such as tokens, keys, and other secrets.

[Expo SecureStore API reference](/versions/latest/sdk/securestore.md) — For more information on how to install and use expo-secure-store, see its API documentation.

## Expo FileSystem

`expo-file-system` provides access to a file system stored locally on the device. Within Expo Go, each project has a separate file system and no access to other Expo projects' files. However, it can save content shared by other projects to the local filesystem and share local files with other projects. It is also capable of uploading and downloading files from network URLs.

[Expo FileSystem API reference](/versions/latest/sdk/filesystem.md) — For more information on how to install and use expo-file-system, see its API documentation.

## Expo SQLite

`expo-sqlite` package gives your app access to a database that can be queried through a WebSQL-like API. The database is persisted across restarts of your app. You can use it for importing an existing database, opening databases, creating tables, inserting items, querying and displaying results, and using prepared statements.

[Expo SQLite API reference](/versions/latest/sdk/sqlite.md) — For more information on how to install and use expo-sqlite, see its API documentation.

## Async Storage

[Async Storage](https://react-native-async-storage.github.io/2.0/Installation/#install) is an asynchronous, unencrypted, persistent key-value storage for React Native apps. It has a simple API and is a good choice for storing small amounts of data. It is also a good choice for storing data that does not need encryption, such as user preferences or app state.

[Async Storage documentation](https://react-native-async-storage.github.io/2.0/Usage/) — For more information on how to install and use Async Storage, see its documentation.

## Other libraries

There are other libraries available for storing data for different purposes. For example, you might not need encryption in your project or are looking for a faster solution similar to Async Storage.

We recommend checking out [React Native for a list of libraries](https://reactnative.directory/?search=storage) to help you store your project's data.
