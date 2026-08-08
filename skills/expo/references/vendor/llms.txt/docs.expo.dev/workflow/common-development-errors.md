---
modificationDate: July 21, 2026
title: Common development errors
description: A list of common development errors that are encountered by developers using Expo.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/workflow/common-development-errors/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/workflow/common-development-errors/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Reference
Pages in this section:
- [Work with monorepos](https://docs.expo.dev/guides/monorepos.md)
- [View logs](https://docs.expo.dev/workflow/logging.md)
- [Development and production modes](https://docs.expo.dev/workflow/development-mode.md)
- [Common development errors](https://docs.expo.dev/workflow/common-development-errors.md) (this page)
- [Android Studio Emulator](https://docs.expo.dev/workflow/android-studio-emulator.md)
- [iOS Simulator](https://docs.expo.dev/workflow/ios-simulator.md)
- [New Architecture](https://docs.expo.dev/guides/new-architecture.md)
- [React Compiler](https://docs.expo.dev/guides/react-compiler.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Common development errors

A list of common development errors that are encountered by developers using Expo.

This page outlines a list of errors that are commonly encountered by developers using Expo. For each error, the first bullet provides an explanation for why the error occurs and the second bullet contains debugging suggestions. If there is an error you think belongs here, we welcome and encourage you to [create a PR](https://github.com/expo/expo/pulls)!

### Metro bundler ECONNREFUSED 127.0.0.1:19001

-   An error is preventing the connection to your local development server.
-   Run `rm -rf .expo` to clear your local state. Check for firewalls or [proxies](/troubleshooting/proxies.md) affecting the network you are currently connected to.

### Module AppRegistry is not a registered callable module (calling runApplication)

-   An error in your code is preventing the JavaScript bundle from being executed on startup.
-   Try running `npx expo start --no-dev --minify` to reproduce the production JS bundle locally. If possible, connect your device and access the device logs via Android Studio or Xcode. Device logs contain much more detailed stacktraces and information. Check to see if you have any changes or errors in your Babel configuration. In some rare cases, this issue could be caused by incompatibility between the Metro JavaScript minifier and certain code in your app.

### npm ERR! No git binary found in $PATH

-   Either you do not have git installed or it is not properly configured in your `$PATH`.
-   [Install git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) if you have not already. Otherwise, check how to set it in your `$PATH` based on your OS.

### XX.X.X is not a valid SDK version

-   The SDK version you are running has been deprecated and is no longer supported.
-   [Upgrade your project](/workflow/upgrading-expo-sdk-walkthrough.md) to a supported SDK version. If you are using a supported version and see this message, you'll need to update your Expo Go app.

### React Native version mismatch

-   The development server running in your terminal is bundling a different version of React Native than the app in your device or simulator.
-   [Align your versions of react-native](/troubleshooting/react-native-version-mismatch.md) by checking the versions in your **app.json** and **package.json**

### Application has not been registered

-   There is a mismatch between the AppKey registered in the native and JS portion of your app.
-   [Align your AppKey](/troubleshooting/application-has-not-been-registered.md) with the native side of your project.

### Application not behaving as expected

-   It is possible caches may be preventing you from seeing the current state of your application.
-   Clear all caches associated with your project in [Unix-like](/troubleshooting/clear-cache-macos-linux.md) or [Windows](/troubleshooting/clear-cache-windows.md) systems.
