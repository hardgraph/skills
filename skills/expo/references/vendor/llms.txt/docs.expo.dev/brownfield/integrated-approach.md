---
modificationDate: July 29, 2026
title: How to add Expo to a native app using the integrated approach
description: A guide for adding Expo and React Native to existing native (brownfield) apps using the integrated approach.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/brownfield/integrated-approach/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/brownfield/integrated-approach/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Existing native apps
Pages in this section:
- [Overview](https://docs.expo.dev/brownfield/overview.md)
- [Isolated approach](https://docs.expo.dev/brownfield/isolated-approach.md)
- [Integrated approach](https://docs.expo.dev/brownfield/integrated-approach.md) (this page)
- [Lifecycle listeners](https://docs.expo.dev/brownfield/lifecycle-listeners.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# How to add Expo to a native app using the integrated approach

A guide for adding Expo and React Native to existing native (brownfield) apps using the integrated approach.

React Native and Expo are flexible and can be adopted incrementally, one screen (or even one view) at a time. You might even find that using Expo in this way is the best fit for your particular application, or you may end up slowly adopting it across more surfaces in your app. Either way, this flexibility allows enables developers to adopt modern, cross-platform tools in their native apps immediately instead of risking a complete rewrite.

This guide will walk you through the steps to add a React Native view into an existing native app. The approach covered here is what we call the "integrated" approach, because React Native and Expo are integrated in the same way that you would any other library.

> Another popular technique is what we call the "isolated" approach, where your Expo app is packaged as a library and treated as a black box by the main existing application. See the [isolated approach guide](/brownfield/isolated-approach.md) for details.

#### Prerequisites

##### Node.js (LTS)

Install [Node.js](https://nodejs.org/en/) to run JavaScript code and Expo CLI.

##### Yarn

Install [Yarn](https://yarnpkg.com/) as a package manager for JavaScript dependencies.

##### CocoaPods iOS

Install [CocoaPods](https://cocoapods.org/), one of the dependency management systems for iOS. CocoaPods is a Ruby [gem](https://en.wikipedia.org/wiki/RubyGems), and you can install it using the version of Ruby that ships with the latest version of macOS.

Learn more from the [Set up environment guide](/get-started/set-up-your-environment.md).

## Create an Expo project

First, create an Expo project inside your existing native project's root directory.

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

This command creates a new directory named **my-project** that contains your new Expo project. While you can name the project anything, this guide uses **my-project** for consistency. The new project includes an example TypeScript application to help you get started.

## Set up your project structure

A standard React Native project places native code in **android** and **ios** directories. The specifics of how to do this depend on your project, but it could be as simple as creating the directories and moving your projects there. For example:

#### Android

```sh
mkdir my-project/android
mv /path/to/your/android-project my-project/android/
```

#### iOS

```sh
mkdir my-project/ios
mv /path/to/your/ios-project my-project/ios/
```

#### Can't move your native projects to android and ios directories?

### Set up a monorepo

Monorepos, or "monolithic repositories", are single repositories containing multiple apps or packages. See [how to work with monorepos](/guides/monorepos.md).

Setting up a monorepo will ensure that Android and iOS scripts will be able to invoke commands from Node libraries even with a custom folder structure. To set up a Yarn monorepo, create a **package.json** file at the root of your project and add the following content:

```json
{
  "version": "1.0.0",
  "private": true,
  "workspaces": ["my-project"]
}
```

Then run `yarn install` to install the dependencies. This will ensure **node_modules** are installed at the root of your project, and that native scripts can interact with React Native code. Make sure to change `["my-project"]` to the name of the Expo project you created in the previous step.

> Opting for a monorepo requires you to configure a custom project root, in Gradle/CocoaPods. This will be covered in the next sections.

## Configuring your native project

#### Configuring Android

To integrate React Native on Android, you need to configure the native project by modifying the following files:

-   **Gradle files**: **settings.gradle**, top-level **build.gradle**, **app/build.gradle**, and **gradle.properties** to add the React Native Gradle Plugin (RNGP) and other properties.
-   **AndroidManifest.xml**: To add necessary permissions. ([Learn more](/brownfield/integrated-approach.md#configuring-your-manifest))
-   **MainActivity**: To load your React Native application.

### Configuring Gradle

Start by editing your **settings.gradle** file and add the following lines (Use the [bare minimum template](https://github.com/expo/expo/blob/main/templates/expo-template-bare-minimum/android/settings.gradle) as a reference):

```groovy
// Configures the React Native Gradle Settings plugin used for autolinking
pluginManagement {
  def reactNativeGradlePlugin = new File(
    providers.exec {
      workingDir(rootDir)
      commandLine("node", "--print", "require.resolve('@react-native/gradle-plugin/package.json', { paths: [require.resolve('react-native/package.json')] })")
    }.standardOutput.asText.get().trim()
  ).getParentFile().absolutePath
  includeBuild(reactNativeGradlePlugin)

  def expoPluginsPath = new File(
    providers.exec {
      workingDir(rootDir)
      commandLine("node", "--print", "require.resolve('expo-modules-autolinking/package.json', { paths: [require.resolve('expo/package.json')] })")
    }.standardOutput.asText.get().trim(),
    "../android/expo-gradle-plugin"
  ).absolutePath
  includeBuild(expoPluginsPath)
}

plugins {
  id("com.facebook.react.settings")
  id("expo-autolinking-settings")
}

extensions.configure(com.facebook.react.ReactSettingsExtension) { ex ->
  ex.autolinkLibrariesFromCommand(expoAutolinking.rnConfigCommand)
}
expoAutolinking.useExpoModules()

// rootProject.name = 'HelloWorld'

expoAutolinking.useExpoVersionCatalog()

includeBuild(expoAutolinking.reactNativeGradlePlugin)
// Include your existing Gradle modules here.
// include(":app")
```

#### Using a custom folder structure?

If you're using a custom folder structure, you need to explicitly set your project root in **settings.gradle** for autolinking to work. Modify the following lines:

Then open your top-level **build.gradle** and include this line (as suggested from the [bare minimum template](https://github.com/expo/expo/blob/main/templates/expo-template-bare-minimum/android/build.gradle)):

This makes sure the React Native Gradle and the Expo plugins are available and applied inside your project.

Add the following lines inside your app's **build.gradle** file (usually **app/build.gradle** — you can use the [bare minimum template file as reference](https://github.com/expo/expo/blob/main/templates/expo-template-bare-minimum/android/app/build.gradle)):

#### Using a custom folder structure?

If you're using a custom folder structure, you need to adjust the `projectRoot` value to point to root of your Expo project in **app/build.gradle**. Modify the following lines:

Finally, open your app's **gradle.properties** file and add the following lines (use the [bare minimum template file as reference](https://github.com/expo/expo/blob/main/templates/expo-template-bare-minimum/android/gradle.properties)):

```properties
reactNativeArchitectures=armeabi-v7a,arm64-v8a,x86,x86_64
newArchEnabled=true
hermesEnabled=true
```

### Configuring your manifest

First, make sure you have the `INTERNET` permission in your **AndroidManifest.xml**:

Now in your **debug** **AndroidManifest.xml**, enable [cleartext traffic](https://developer.android.com/training/articles/security-config#CleartextTrafficPermitted):

This is necessary for your app to communicate with your local [Metro bundler](https://metrobundler.dev/) via HTTP. You can use the **AndroidManifest.xml** files from the bare minimum template as a reference: [main](https://github.com/expo/expo/blob/main/templates/expo-template-bare-minimum/android/app/src/main/AndroidManifest.xml) and [debug](https://github.com/expo/expo/blob/main/templates/expo-template-bare-minimum/android/app/src/debug/AndroidManifest.xml)

### Integrating with your code

Now, you need to add some native code to start the React Native runtime and tell it to render your React components.

#### Updating your `Application` class

Start by updating your `Application` class to initialize React Native. You can use **MainApplication.kt** from the [bare minimum template](https://github.com/expo/expo/blob/main/templates/expo-template-bare-minimum/android/app/src/main/java/com/helloworld/MainApplication.kt) as a reference:

#### Creating a `ReactActivity`

Create a new `Activity` that will extend `ReactActivity` and host the React Native code. This activity will be responsible for starting the React Native runtime and rendering the React component. You can use the [**MainActivity.kt** from bare minimum template file](https://github.com/expo/expo/blob/main/templates/expo-template-bare-minimum/android/app/src/main/java/com/helloworld/MainActivity.kt) as a reference:

```kotlin
// package <your-package-here>

import android.os.Build

import com.facebook.react.ReactActivity
import com.facebook.react.ReactActivityDelegate
import com.facebook.react.defaults.DefaultNewArchitectureEntryPoint.fabricEnabled
import com.facebook.react.defaults.DefaultReactActivityDelegate

import expo.modules.ReactActivityDelegateWrapper

class MyReactActivity : ReactActivity() {

  /**
   * Returns the name of the main component registered from JavaScript. This is used to schedule
   * rendering of the component.
   */
  override fun getMainComponentName(): String = "main"

  /**
   * Returns the instance of the [ReactActivityDelegate]. We use [DefaultReactActivityDelegate]
   * which allows you to enable New Architecture with a single boolean flags [fabricEnabled]
   */
  override fun createReactActivityDelegate(): ReactActivityDelegate {
    return ReactActivityDelegateWrapper(
          this,
          BuildConfig.IS_NEW_ARCHITECTURE_ENABLED,
          object : DefaultReactActivityDelegate(
              this,
              mainComponentName,
              fabricEnabled
          ){})
  }
}
```

Add the new Activity to your **AndroidManifest.xml** file, make sure to set the theme of `MyReactActivity` to `Theme.AppCompat.Light.NoActionBar` (or to any non-ActionBar theme) to avoid your application rendering an `ActionBar` on top of the React Native screen:

Now your activity is ready to run some JavaScript code.

#### Configuring iOS

To integrate React Native on iOS, you need to configure the native iOS project by modifying the following files:

-   **Podfile**: To add the React Native dependencies.
-   **Xcode project**: To add a build phase for bundling JavaScript code.
-   **Info.plist**: To configure app settings required by React Native.

### Configuring CocoaPods

If your project does not have a **Podfile**, you can create one using the [bare minimum template](https://github.com/expo/expo/blob/main/templates/expo-template-bare-minimum/ios/Podfile) as a reference:

```rb
require File.join(File.dirname(`node --print "require.resolve('expo/package.json')"`), "scripts/autolinking")
require File.join(File.dirname(`node --print "require.resolve('react-native/package.json')"`), "scripts/react_native_pods")

require 'json'

platform :ios, '16.4'
install! 'cocoapods',
  :deterministic_uuids => false

prepare_react_native_project!

target 'HelloWorld' do
  use_expo_modules!

  config_command = [
    'npx',
    'expo-modules-autolinking',
    'react-native-config',
    '--json',
    '--platform',
    'ios'
  ]
  config = use_native_modules!(config_command)

  use_frameworks! :linkage => ENV['USE_FRAMEWORKS'].to_sym if ENV['USE_FRAMEWORKS']

  use_react_native!(
    :path => config[:reactNativePath],
    :hermes_enabled => true,
    # An absolute path to your application root.
    :app_path => "#{Pod::Config.instance.installation_root}/..",
    :privacy_file_aggregation_enabled => true,
  )

  post_install do |installer|
    react_native_post_install(
      installer,
      config[:reactNativePath],
      :mac_catalyst_enabled => false,
    )
  end
end
```

If your project already has a **Podfile**, you'll need to manually merge the React Native dependencies into your existing **Podfile**.

#### Using a custom folder structure?

If you're using a custom folder structure, you need to explicitly set your project root in **Podfile** for autolinking to work. Modify the following lines in your **Podfile**:

Now, run the following command:

```sh
pod install
```

Running the `pod` command will integrate the React Native code into your app, allowing your iOS files to import the React Native headers.

### Configuring your Xcode project

After the `pod install` command, CocoaPods will create an Xcode workspace **{Project}.xcworkspace**, you will need to open the **xcworkspace** project than the traditional **xcodeproj** project. Alternatively, you can use the following command to open the project:

```sh
xed my-project/ios
```

In the Xcode project navigator, select your project and then select your app target under `TARGETS`. In **Build Settings**, using the search bar, search for `ENABLE_USER_SCRIPT_SANDBOXING`. If it is not already, set its value to `No`. This is needed to properly switch between the Debug and Release versions of the [Hermes engine](https://github.com/facebook/hermes/blob/main/README.md) that is shipped with React Native.

Now switch to the **Build Phases** tab and add a new `Run Script Phase` before the `[CP] Embed Pods Frameworks` phase. This script will bundle your JavaScript code and assets into the iOS application.

```sh
if [[ -f "$PODS_ROOT/../.xcode.env" ]]; then
  source "$PODS_ROOT/../.xcode.env"
fi
if [[ -f "$PODS_ROOT/../.xcode.env.local" ]]; then
  source "$PODS_ROOT/../.xcode.env.local"
fi

# The project root by default is one level up from the ios directory
export PROJECT_ROOT="$PROJECT_DIR"/..

if [[ "$CONFIGURATION" = *Debug* ]]; then
  export SKIP_BUNDLING=1
fi
if [[ -z "$ENTRY_FILE" ]]; then
  # Set the entry JS file using the bundler's entry resolution.
  export ENTRY_FILE="$("$NODE_BINARY" -e "require('expo/scripts/resolveAppEntry')" "$PROJECT_ROOT" ios absolute | tail -n 1)"
fi

if [[ -z "$CLI_PATH" ]]; then
  # Use Expo CLI
  export CLI_PATH="$("$NODE_BINARY" --print "require.resolve('@expo/cli', { paths: [require.resolve('expo/package.json')] })")"
fi
if [[ -z "$BUNDLE_COMMAND" ]]; then
  # Default Expo CLI command for bundling
  export BUNDLE_COMMAND="export:embed"
fi

# Source .xcode.env.updates if it exists to allow
# SKIP_BUNDLING to be unset if needed
if [[ -f "$PODS_ROOT/../.xcode.env.updates" ]]; then
  source "$PODS_ROOT/../.xcode.env.updates"
fi
# Source local changes to allow overrides
# if needed
if [[ -f "$PODS_ROOT/../.xcode.env.local" ]]; then
  source "$PODS_ROOT/../.xcode.env.local"
fi

`"$NODE_BINARY" --print "require('path').dirname(require.resolve('react-native/package.json')) + '/scripts/react-native-xcode.sh'"`
```

Next time, when you build your app for Release, the React Native code will be bundled using Expo CLI and embedded into the app.

Edit your **Info.plist** file and make sure to add the `UIViewControllerBasedStatusBarAppearance` key with a value of `NO`, this is needed to ensure that the status bar is properly managed by React Native.

### Integrating with your code

Now, you need to add some native code to start the React Native runtime and tell it to render your React components.

#### Create the ReactViewController

Create a new file called **ReactViewController.swift**, this will be the `ViewController` that loads a React Native view as its `view`.

```swift
import UIKit
import React
import React_RCTAppDelegate
import ReactAppDependencyProvider

class ReactNativeViewController: UIViewController {
  var reactNativeFactory: RCTReactNativeFactory?
  var reactNativeFactoryDelegate: RCTReactNativeFactoryDelegate?

  override func viewDidLoad() {
    super.viewDidLoad()
    reactNativeFactoryDelegate = ReactNativeDelegate()
    reactNativeFactoryDelegate!.dependencyProvider = RCTAppDependencyProvider()
    reactNativeFactory = RCTReactNativeFactory(delegate: reactNativeFactoryDelegate!)
    view = reactNativeFactory!.rootViewFactory.view(withModuleName: "HelloWorld")

  }
}

class ReactNativeDelegate: RCTDefaultReactNativeFactoryDelegate {
    override func sourceURL(for bridge: RCTBridge) -> URL? {
      self.bundleURL()
    }

    override func bundleURL() -> URL? {
      #if DEBUG
      RCTBundleURLProvider.sharedSettings().jsBundleURL(forBundleRoot: ".expo/.virtual-metro-entry")
      #else
      Bundle.main.url(forResource: "main", withExtension: "jsbundle")
      #endif
    }

}
```

#### Presenting a React Native view in a rootViewController

Finally, you can present your React Native view. To do so, you need a new View Controller that can host a view in which we can load the JS content. You already have the initial `ViewController`, and you can make it present the `ReactViewController`. There are several ways to do so, depending on your app. For this example, let's assume that you have a button that presents React Native modally.

```swift
import UIKit

class ViewController: UIViewController {

  var reactViewController: ReactViewController?

  override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view.
    self.view.backgroundColor = .systemBackground

    let button = UIButton()
    button.setTitle("Open React Native", for: .normal)
    button.setTitleColor(.systemBlue, for: .normal)
    button.setTitleColor(.blue, for: .highlighted)
    button.addAction(UIAction { [weak self] _ in
      guard let self else { return }
      if reactViewController == nil {
       reactViewController = ReactViewController()
      }
      present(reactViewController!, animated: true)
    }, for: .touchUpInside)
    self.view.addSubview(button)

    button.translatesAutoresizingMaskIntoConstraints = false
    NSLayoutConstraint.activate([
      button.leadingAnchor.constraint(equalTo: self.view.leadingAnchor),
      button.trailingAnchor.constraint(equalTo: self.view.trailingAnchor),
      button.centerXAnchor.constraint(equalTo: self.view.centerXAnchor),
      button.centerYAnchor.constraint(equalTo: self.view.centerYAnchor),
    ])
  }
}
```

## Test your integration

You have completed all the basic steps to integrate React Native with your application. Now run the following command in the React Native directory to start the [Metro bundler](https://metrobundler.dev/)

```sh
# npm
npm run start

# yarn
yarn run start

# pnpm
pnpm run start

# bun
bun run start
```

Metro builds your TypeScript application code into a bundle, serves it through its HTTP server, and shares the bundle from `localhost` on your developer environment to a simulator or device, allowing for [hot reloading](https://reactnative.dev/blog/2016/03/24/introducing-hot-reloading). Now you can build and run your app as normal. Once you reach your React-powered Activity inside the app, it should load the JavaScript code from the development server.
