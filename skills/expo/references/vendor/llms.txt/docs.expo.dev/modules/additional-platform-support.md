---
modificationDate: May 23, 2026
title: Additional platform support
description: Learn how to add support for macOS and tvOS platforms.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/modules/additional-platform-support/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/modules/additional-platform-support/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [How to use a standalone Expo module](https://docs.expo.dev/modules/use-standalone-expo-module-in-your-project.md)
- [Wrap third-party native libraries](https://docs.expo.dev/modules/third-party-library.md)
- [Integrate in an existing library](https://docs.expo.dev/modules/existing-library.md)
- [Additional platform support](https://docs.expo.dev/modules/additional-platform-support.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Additional platform support

Learn how to add support for macOS and tvOS platforms.

Expo Modules API provides first-class support for Android and iOS. However, since all Apple platforms are based on the same foundation and use the same programming language, targeting other [Out-of-Tree platforms](https://reactnative.dev/docs/out-of-tree-platforms) in the Expo module is possible.

Currently, only **macOS** and **tvOS** platforms are supported. This guide will walk you through adding support for these platforms.

## Use the `"apple"` platform in `expo-module.config.json`

To provide seamless support for other Apple platforms, Expo SDK introduced a universal `"apple"` platform to instruct the [autolinking](/modules/autolinking.md) that the module may support any of the Apple platform and whether to link the module in the specific CocoaPods target is moved off to the podspec. If you have used `"ios"` before, you can safely replace it:

```diff
- "platforms": ["ios"],
- "ios": {
- "modules": ["MyModule"]
- }
+ "platforms": ["apple"],
+ "apple": {
+ "modules": ["MyModule"]
+ }
  }
```

## Update the podspec to declare support for other platforms

The module's podspec needs to be updated with a list of the supported platforms. Otherwise, CocoaPods would fail to install the pod on targets for the other platforms. As mentioned in the first step, this part of the spec is the source of truth for autolinking when the module is configured with a universal `"apple"` platform.

```diff
- s.platform       = :ios, '13.4'
+ s.platforms = {
+ :ios => '13.4',
+ :tvos => '13.4',
+ :osx => '10.15'
+ }
```

Any changes in the podspec require running `pod install` to have an effect.

## Set up `react-native-macos` or `react-native-tvos` in the app

If you are writing a local module and your app is already set up, you can skip this step. Otherwise, you will need to set up your app or the example app if you are writing a standalone (non-local) module.

-   **For macOS**: follow the official [Install React Native for macOS](https://microsoft.github.io/react-native-macos/docs/getting-started) guide from `react-native-macos` documentation.
-   **For tvOS**: follow the instructions in the [`react-native-tvos`](https://github.com/react-native-tvos/react-native-tvos) repository. If you are building an Expo app, you should also follow the instructions in the [Build Expo apps for TV guide](/guides/building-for-tv.md).

## Review the code for using APIs not supported on these platforms

Platform APIs may differ between Apple platforms. The most noticeable difference comes from relying on different UI frameworks —`UIKit` on iOS/tvOS and `AppKit` on macOS.

Both `react-native-macos` and `expo-modules-core` provide aliases and polyfills to reference`UIKit` classes on macOS target (for example, `UIView` is an alias to `NSView`, `UIApplication` is an alias to `NSApplication`), but it's usually not enough for iOS-first libraries to support other platforms out of the box. You may need to write conditionally compiled code that uses different implementations depending on the platform.

To do this, use Swift compiler directives with the `os` condition, which includes a given piece of code when our app is being built for a specific platform. In combination with the `#if` and `#else` directives, lets you set up platform-specific branches within the cross-platform code.

```swift
#if os(iOS)
  // iOS implementation
#elseif os(macOS)
  // macOS implementation
#elseif os(tvOS)
  // tvOS implementation
#endif
```

Your module is now ready to be used on Out-of-Tree platform.
