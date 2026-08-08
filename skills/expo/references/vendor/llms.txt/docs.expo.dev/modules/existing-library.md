---
modificationDate: May 23, 2026
title: Integrate in an existing library
description: Learn how to integrate Expo Modules API into an existing React Native library.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/modules/existing-library/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/modules/existing-library/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Integrate in an existing library](https://docs.expo.dev/modules/existing-library.md) (this page)
- [Additional platform support](https://docs.expo.dev/modules/additional-platform-support.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Integrate in an existing library

Learn how to integrate Expo Modules API into an existing React Native library.

There are cases where you may want to integrate the Expo Modules API into an existing React Native library. For example, it might be useful to incrementally rewrite your library or to take advantage of [Android lifecycle listeners](/modules/android-lifecycle-listeners.md) and [iOS AppDelegate subscribers](/modules/appdelegate-subscribers.md) to automatically set up the library.

This guide will help you set up your existing React Native library to access Expo Modules API.

#### Prerequisites

##### Create expo-module.config.json

Create the [**expo-module.config.json**](/modules/module-config.md) file at the root of your project and add an empty object `{}` inside it. You will update it later to enable specific features.

This file is required for [Expo Autolinking](/modules/autolinking.md) to recognize your library as an Expo module and automatically link your native code.

## Add the `expo-modules-core` native dependency

Add `expo-modules-core` as a dependency in your **build.gradle** and **podspec** files:

```groovy
// ...
dependencies {
  // ...
  implementation project(':expo-modules-core')
}
```

```ruby
# ...
Pod::Spec.new do |s|
  # ...
  s.dependency 'ExpoModulesCore'
end
```

## Add Expo packages to dependencies

Add `expo` package as a peer dependency in your **package.json** — we recommend using `*` as a version range so as not to cause any duplicated packages in user's **node_modules** directory.

Your library also needs to depend on `expo-modules-core` but only as a dev dependency — it's already provided in the projects depending on your library by the `expo` package with the version of core that is compatible with the specific SDK used in the project.

```json
{
  ... 
  "devDependencies": {
    "expo-modules-core": "^X.Y.Z"
  },
  "peerDependencies": {
    "expo": "*"
  },
  "peerDependenciesMeta": {
    "expo": {
      "optional": true
    }
  }
}
```

## Create a native module

Create Kotlin and Swift files from the templates below:

```kotlin
package my.module.package

import expo.modules.kotlin.modules.Module
import expo.modules.kotlin.modules.ModuleDefinition

class MyModule : Module() {
  override fun definition() = ModuleDefinition {
    // Definition components go here
  }
}
```

```swift
import ExpoModulesCore

public class MyModule: Module {
  public func definition() -> ModuleDefinition {
    // Definition components go here
  }
}
```

Then, add your classes to Android and/or iOS `modules` in the [**expo-module.config.json**](/modules/module-config.md) file. Expo Autolinking will automatically link these classes as native modules in the user's project.

```json
{
  "ios": {
    "modules": ["MyModule"]
  },
  "android": {
    "modules": ["my.module.package.MyModule"]
  }
}
```

If you already have an example app in your workspace, ensure that the module is linked correctly.

-   **On Android** the native module class will be linked automatically before building, as part of the Gradle build task.
-   **On iOS** you need to run `pod install` to link the new class.

These module classes are now accessible from the JavaScript code using the `requireNativeModule` function from the `expo-modules-core` package. We recommend creating a separate file that exports the native module for simplicity.

```ts
import { requireNativeModule } from 'expo-modules-core';

export default requireNativeModule('MyModule');
```

Now that the class is set up and linked, you can start to implement its functionality. See the [native module API](/modules/module-api.md) reference page and links to [examples](/modules/module-api.md#examples) from simple to moderately complex real-world modules to understand how to use the API.
