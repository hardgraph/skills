---
modificationDate: June 26, 2026
title: '"React Native version mismatch" errors'
description: Learn about what React Native version mismatch means and how to resolve it in an Expo or React Native app.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/troubleshooting/react-native-version-mismatch/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/troubleshooting/react-native-version-mismatch/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > More > Troubleshooting
Pages in this section:
- [Overview](https://docs.expo.dev/troubleshooting/overview.md)
- ["Application has not been registered" error](https://docs.expo.dev/troubleshooting/application-has-not-been-registered.md)
- [Clear bundler caches on macOS and Linux](https://docs.expo.dev/troubleshooting/clear-cache-macos-linux.md)
- [Clear bundler caches on Windows](https://docs.expo.dev/troubleshooting/clear-cache-windows.md)
- ["React Native version mismatch" errors](https://docs.expo.dev/troubleshooting/react-native-version-mismatch.md) (this page)
- [Proxies](https://docs.expo.dev/troubleshooting/proxies.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# "React Native version mismatch" errors

Learn about what React Native version mismatch means and how to resolve it in an Expo or React Native app.

When developing an Expo or React Native app, it's common to run into an error that looks like:

```sh
React Native version mismatch.
JavaScript version: X.XX.X
Native version: X.XX.X
Make sure you have rebuilt the native code...
```

## What this error means

The bundler that you're running in your terminal (using `npx expo start`) is using a different JavaScript version of `react-native` than the native app on your device or emulator. This can happen after upgrading your React Native or Expo SDK version, _or_ when connecting to the wrong local development server.

## How to fix it

-   Close out any development servers that you have running (you can list all terminal processes with the `ps` command, and search for Expo CLI or React Native community CLI processes with `ps -A | grep "expo\|react-native"`).
    
-   If this is a Expo project, either remove the `sdkVersion` field from your **app.json** file, or make sure it matches the value of the `expo` dependency in your **package.json** file.
    
-   If this is a Expo project, you should make sure your `react-native` version is correct. Run `npx expo-doctor` will show a warning where the `react-native` version you should install. If you did upgrade to a newer SDK, make sure to run `npx expo install --fix` and follow the prompts. Expo CLI will make sure that your dependency versions for packages like `expo` and `react-native` are aligned.
    
-   If this is an [existing React Native project](/bare/overview.md), and this error is occurring right after upgrading your React Native version, you should double-check that you have performed each of the upgrade steps correctly.
    
-   Finally:
    
    -   Clear your bundler caches by running `rm -rf node_modules && npm cache clean --force && npm install && watchman watch-del-all && rm -rf $TMPDIR/haste-map-* && rm -rf $TMPDIR/metro-cache && npx expo start --clear`
        -   Commands if you are using npm can be found [here.](/troubleshooting/clear-cache-macos-linux.md)
        -   Commands if you are using Windows can be found [here.](/troubleshooting/clear-cache-windows.md)
    -   If this is an existing React Native project, run `npx pod-install`, then rebuild your native projects (run `yarn android` to rebuild for Android, and `yarn ios` to rebuild iOS)
