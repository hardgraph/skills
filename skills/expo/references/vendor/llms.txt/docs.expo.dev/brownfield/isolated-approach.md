---
modificationDate: July 20, 2026
title: How to add Expo to a native app using the isolated approach
description: A guide for adding Expo and React Native as a native library and integrating it into an existing (brownfield) native app using the isolated approach.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/brownfield/isolated-approach/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/brownfield/isolated-approach/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Existing native apps
Pages in this section:
- [Overview](https://docs.expo.dev/brownfield/overview.md)
- [Isolated approach](https://docs.expo.dev/brownfield/isolated-approach.md) (this page)
- [Integrated approach](https://docs.expo.dev/brownfield/integrated-approach.md)
- [Lifecycle listeners](https://docs.expo.dev/brownfield/lifecycle-listeners.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# How to add Expo to a native app using the isolated approach

A guide for adding Expo and React Native as a native library and integrating it into an existing (brownfield) native app using the isolated approach.

In the isolated approach, your React Native code is developed and maintained separately from your native project. You package it as a native library, using an AAR for Android or XCFramework for iOS, and integrate it into your native app like any other dependency.

This approach is ideal when you want to minimize the impact of React Native on your existing native build process, or when you have separate teams for native and React Native development. Using this approach, native developers don't need Node.js, Yarn, or any React Native build tooling, and they can just consume pre-built artifacts.

> For an alternative approach where React Native is integrated directly into your native project, see the [integrated approach guide](/brownfield/integrated-approach.md).

#### Prerequisites

##### Node.js (LTS)

Install [Node.js](https://nodejs.org/en/) to run JavaScript code and Expo CLI.

##### Yarn

Install [Yarn](https://yarnpkg.com/) as a package manager for JavaScript dependencies.

Learn more from the [Set up environment guide](/get-started/set-up-your-environment.md).

## Set up an Expo project

### Create a new Expo project

Run the following command to create a new directory named **my-project** that contains your new Expo project. While you can name the project anything, this guide uses **my-project** for consistency.

```sh
# npm
npx create-expo-app@latest my-project --template default@sdk-57

# yarn
yarn create expo-app my-project --template default@sdk-57

# pnpm
pnpm create expo-app my-project --template default@sdk-57

# bun
bun create expo my-project --template default@sdk-57
```

The **my-project** does not need to live inside your existing native app and can be created in a separate repository or a monorepo. The new project includes an example TypeScript application to help you get started.

### Install expo-brownfield

Navigate to your new Expo project and install the `expo-brownfield` library, which provides the tools to build your React Native code as native libraries and integrate them into your existing native app.

```sh
# npm
npx expo install expo-brownfield

# yarn
yarn expo install expo-brownfield

# pnpm
pnpm expo install expo-brownfield

# bun
bun expo install expo-brownfield
```

### Adjust the config plugin (optional)

`expo-brownfield` should automatically add an entry to the `plugins` array in your **app.json**, using the default configuration, which is sufficient for most projects.

```json
{
  "expo": {
    "plugins": ["expo-brownfield"]
  }
}
```

The defaults are derived from your app config (for example, target names are based on your app's scheme or slug). You can also pass options to customize the target names, bundle identifiers, and publishing configuration.

#### Custom expo-brownfield configuration

```json
{
  "expo": {
    "plugins": [
      [
        "expo-brownfield",
        {
          "ios": {
            "targetName": "MyBrownfield",
            "bundleIdentifier": "com.example.mybrownfield"
          },
          "android": {
            "libraryName": "mybrownfield",
            "group": "com.example",
            "package": "com.example.mybrownfield",
            "version": "1.0.0"
          }
        }
      ]
    ]
  }
}
```

See the [`expo-brownfield` API reference](/versions/latest/sdk/brownfield.md) for details on all available options.

## Export your Expo project as a native library

Once you have your Expo project set up, use the `expo-brownfield` CLI to build your React Native code as AARs for Android and XCFrameworks for iOS.

#### Android

From your Expo project directory, run:

```sh
# npm
npx expo-brownfield build:android

# yarn
yarn dlx expo-brownfield build:android

# pnpm
pnpm dlx expo-brownfield build:android

# bun
bunx expo-brownfield build:android
```

This will build the AAR and publish it to a Maven repository. By default, it publishes to your local Maven repository (`~/.m2`), but it can also be configured to publish to a remote repository. The produced artifact name will be determined by your config plugin settings, in this case `com.username.myproject:brownfield:1.0.0`.

See the [API reference](/versions/latest/sdk/brownfield.md) for more details on build options, such as building only debug or release, specifying a custom output directory, and more.

#### iOS

From your Expo project directory, run:

```sh
# npm
npx expo-brownfield build:ios

# yarn
yarn dlx expo-brownfield build:ios

# pnpm
pnpm dlx expo-brownfield build:ios

# bun
bunx expo-brownfield build:ios
```

This will build the XCFramework artifacts: compile the framework target for both device and simulator architectures, package them into XCFrameworks, and copy the Hermes engine framework.

When the build process is completed, the output is placed in the **./artifacts** directory and contains:

-   **{TargetName}.xcframework** - Your Expo project as a native library
-   **hermesvm.xcframework** - The Hermes JavaScript engine

See the [`expo-brownfield` API reference](/versions/latest/sdk/brownfield.md) for more details on build options, such as building only debug or release, specifying a custom output directory, and more.

### Ship the artifacts as a Swift Package

Pass the `--package [name]` flag to bundle the build output as a self-contained Swift Package instead of separate **.xcframework** directories. You can then add it to your host app as a local dependency in Xcode rather than manually dragging frameworks into the project.

```sh
# npm
npx expo-brownfield build:ios --release --package MyAppPackage

# yarn
yarn dlx expo-brownfield build:ios --release --package MyAppPackage

# pnpm
pnpm dlx expo-brownfield build:ios --release --package MyAppPackage

# bun
bunx expo-brownfield build:ios --release --package MyAppPackage
```

The flag accepts an optional name. If omitted, the package defaults to **{TargetName}Artifacts**. The resulting directory is a complete Swift Package with this layout:

`artifacts`

 `MyAppPackage`

  `Package.swift`

  `xcframeworks`

   `MyAppPackage.xcframework`

   `hermesvm.xcframework`

   `React.xcframework`

   `ReactNativeDependencies.xcframework`

#### Debugging native targets

If you need to debug native code of the Expo project targets, you can run `npx expo prebuild` to generate the native projects with the brownfield library targets, inside the **android** and **ios\`** directories.

```sh
# npm
npx expo prebuild

# yarn
yarn expo prebuild

# pnpm
pnpm expo prebuild

# bun
bun expo prebuild
```

The above command generates the following:

-   **Android**: A separate library module containing `ReactNativeHostManager`, `BrownfieldActivity`, `ReactNativeFragment`, `ReactNativeViewFactory`, and `BrownfieldMessaging`.
-   **iOS**: A separate Xcode framework target containing `ReactNativeHostManager`, `ReactNativeViewController`, `ReactNativeView` (SwiftUI), `BrownfieldMessaging`, and `ReactNativeDelegate`.

## Integrate into your native app

With the artifacts built, you can now integrate them into your existing native app. The exact steps will depend on your project structure and build system, but the general process involves adding the pre-built artifacts as dependencies and initializing the React Native host.

#### Android

### Add the Maven dependency

Start by adding the dependency to your app's **build.gradle.kts**. The group, artifact name, and version should match your config plugin settings:

```kotlin
dependencies {
  implementation("com.username.myproject:brownfield:1.0.0")
}
```

If the library was published to local Maven, make sure to add `mavenLocal()` to your repository configuration:

```kotlin
dependencyResolutionManagement {
  repositories {
    google()
    mavenCentral()
    mavenLocal()
  }
}
```

### Show a React Native screen

Create an activity that extends `BrownfieldActivity` and use the `showReactNativeFragment()` extension:

```kotlin
import android.os.Bundle
import com.example.brownfield.BrownfieldActivity
import com.example.brownfield.showReactNativeFragment

class ExpoActivity : BrownfieldActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    showReactNativeFragment()
  }
}
```

Add the activity to your **AndroidManifest.xml** with a non-ActionBar theme:

```xml
<activity
  android:name=".ExpoActivity"
  android:theme="@style/Theme.AppCompat.Light.NoActionBar"
  android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|screenSize|smallestScreenSize|uiMode"
/>
```

Then launch it from anywhere in your app:

```kotlin
startActivity(Intent(this, ExpoActivity::class.java))
```

`BrownfieldActivity` extends `AppCompatActivity` and handles forwarding configuration changes to Expo modules. The `showReactNativeFragment()` extension also sets up native back button handling automatically.

#### iOS

### Add the artifacts to your project

The integration steps depend on whether you built XCFrameworks or a Swift Package.

#### XCFrameworks (default)

Drag both XCFramework files ({TargetName}**.xcframework** and **hermesvm.xcframework**) into your Xcode project navigator. In the dialog that appears:

-   Check **Copy items if needed**
-   Add them to your app target

Then, in your target's **General** tab under **Frameworks, Libraries, and Embedded Content**, ensure both frameworks are set to **Embed & Sign**.

#### Swift Package

When `build:ios` produces a Swift Package (for example, **./artifacts/MyAppPackage-release**), add it to your host app as a local dependency in Xcode and select the package directory. Xcode automatically links the bundled **.xcframework** files through the aggregate library product.

To support both `--debug` and `--release`, point your host app at the matching package for each build configuration.

### Initialize React Native

Call `ReactNativeHostManager.shared.initialize()` early in your app's lifecycle. A good place is your `AppDelegate`:

```swift
import UIKit
import MyAppBrownfield // Replace with your target name

@main
class AppDelegate: UIResponder, UIApplicationDelegate {
  func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
  ) -> Bool {
    ReactNativeHostManager.shared.initialize()
    return true
  }
}
```

### Present a React Native view

#### UIKit

```swift
import UIKit
import MyAppBrownfield

class ViewController: UIViewController {
  @IBAction func openReactNative(_ sender: Any) {
    let rnViewController = ReactNativeViewController(moduleName: "main")
    navigationController?.pushViewController(rnViewController, animated: true)
  }
}
```

The `ReactNativeViewController` also accepts optional `initialProps` and `launchOptions` parameters:

```swift
let rnViewController = ReactNativeViewController(
  moduleName: "main",
  initialProps: ["userId": "123"],
  launchOptions: [:]
)
```

#### SwiftUI

```swift
import SwiftUI
import MyAppBrownfield

struct ContentView: View {
  @State private var showReactNative = false

  var body: some View {
    Button("Open React Native") {
      showReactNative = true
    }
    .fullScreenCover(isPresented: $showReactNative) {
      ReactNativeView(moduleName: "main")
    }
  }
}
```

## Test your integration

You have completed all the basic steps to integrate React Native with your application. Now it's time to test it out. The exact process will depend on whether you're running a debug or release build.

### Development (debug builds)

Now run the following command in the React Native directory to start the [Metro bundler](https://metrobundler.dev/)

```sh
# npm
npx expo start

# yarn
yarn expo start

# pnpm
pnpm expo start

# bun
bun expo start
```

Then, build and run the native app from Android Studio or Xcode. When you navigate to the React Native screen, it will load from the Metro dev server with hot reloading support.

### Production (release builds)

In release builds, the JavaScript bundle is embedded in the artifact (AAR or XCFramework), so the Metro server is not needed. Build the native app in Release configuration and verify the React Native screen loads correctly.

## Next steps

[Lifecycle listeners](/brownfield/lifecycle-listeners.md) — Configure application lifecycle listeners for deeper integration with Expo modules.

[expo-brownfield API reference](/versions/latest/sdk/brownfield.md) — Explore the full JavaScript API for communication, navigation, and more.
