---
modificationDate: July 01, 2026
title: Install Expo modules in an existing React Native project
description: Learn how to prepare your existing React Native project to install and use any Expo module.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/bare/installing-expo-modules/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/bare/installing-expo-modules/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Existing React Native apps
Pages in this section:
- [Overview](https://docs.expo.dev/bare/overview.md)
- [Install Expo modules](https://docs.expo.dev/bare/installing-expo-modules.md) (this page)
- [Migrate to Expo CLI](https://docs.expo.dev/bare/using-expo-cli.md)
- [Install expo-updates](https://docs.expo.dev/bare/installing-updates.md)
- [Install expo-dev-client](https://docs.expo.dev/bare/install-dev-builds-in-bare.md)
- [Native project upgrade helper](https://docs.expo.dev/bare/upgrade.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Install Expo modules in an existing React Native project

Learn how to prepare your existing React Native project to install and use any Expo module.

To use Expo modules in your app, you will need to install and configure the `expo` package.

The `expo` package has a small footprint; it includes only a minimal set of packages that are needed in nearly every app and the module and autolinking infrastructure that other Expo SDK packages are built with. Once the `expo` package is installed and configured in your project, you can use `npx expo install` to add any other Expo module from the SDK.

Depending on how you [initialized the project](/bare/overview.md), there are two ways you can install the Expo modules: [automatically](/bare/installing-expo-modules.md#automatic-installation) or [manually](/bare/installing-expo-modules.md#manual-installation).

## Automatic installation

To install and use Expo modules, the easiest way to get up and running is with the `install-expo-modules` command.

```sh
npx install-expo-modules@latest
```

-   ✓ **When the command succeeds**, you will be able to add any Expo module in your app! Proceed to [Usage](/bare/installing-expo-modules.md#usage) for more information.
    
-   ✗ **If the command fails**, follow the manual installation instructions. Updating code programmatically can be tricky, and if your project deviates significantly from a default React Native project, then you need to perform manual installation and adapt the instructions here to your codebase.
    

## Manual installation

The following instructions apply to installing the latest version of Expo modules in React Native 0.86. For previous versions, check the [native upgrade helper](/bare/upgrade.md) to see how these files are customized.

```sh
npm install expo
```

Once installation is complete, apply the changes from the following diffs to configure Expo modules in your project. This is expected to take about five minutes, and you may need to adapt it slightly depending on how customized your project is.

### Configuration for Android

### Configuration for iOS

Optionally, you can also add additional delegate methods to your **AppDelegate.swift**. Some libraries may require them, so unless you have a good reason to leave them out, it is recommended to add them. [See delegate methods in AppDelegate.swift](https://github.com/expo/expo/blob/sdk-57/templates/expo-template-bare-minimum/ios/HelloWorld/AppDelegate.swift#L34-L51).

Save all of your changes and update your iOS Deployment Target in Xcode to `iOS 16.4`:

-   Open **your-project-name.xcworkspace** in Xcode, select your project in the left sidebar.
-   Select **Targets** > **your-project-name** > **Build Settings** > **iOS Deployment Target** and ensure it is set to `iOS 16.4`.

The last step is to install the project's CocoaPods again to pull in Expo modules that are detected by `use_expo_modules!` directive that we added to the **Podfile**:

```sh
npx pod-install
npx expo run:ios
```

### Configure Expo CLI for bundling on Android and iOS

We recommend using Expo CLI and related tooling configurations to bundle your app JavaScript code and assets. This adds support for using the `"main"` field in **package.json** to use [Expo Router](/router/introduction.md) library. Not using Expo CLI for bundling may result in unexpected behavior. [Learn more about Expo CLI](/bare/using-expo-cli.md).

#### Use babel-preset-expo in your babel.config.js

#### Extend expo/metro-config in your metro.config.js

#### Configure Android project to bundle with Expo CLI

#### Configure iOS project to bundle with Expo CLI

Replace the shell script under **Build Phases** > **Bundle React Native code and images** in Xcode with the following:

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
  export ENTRY_FILE="$("$NODE_BINARY" -e "require('expo/scripts/resolveAppEntry')" "$PROJECT_ROOT" ios relative | tail -n 1)"
fi

if [[ -z "$CLI_PATH" ]]; then
  # Use Expo CLI
  export CLI_PATH="$("$NODE_BINARY" --print "require.resolve('@expo/cli')")"
fi
if [[ -z "$BUNDLE_COMMAND" ]]; then
  # Default Expo CLI command for bundling
  export BUNDLE_COMMAND="export:embed"
fi

`"$NODE_BINARY" --print "require('path').dirname(require.resolve('react-native/package.json')) + '/scripts/react-native-xcode.sh'"`
```

And add support the `"main"` field in **package.json** by making the following change to **AppDelegate.swift**:

```diff
override func bundleURL() -> URL? {
  #if DEBUG
- RCTBundleURLProvider.sharedSettings().jsBundleURL(forBundleRoot: "index")
+ RCTBundleURLProvider.sharedSettings().jsBundleURL(forBundleRoot: ".expo/.virtual-metro-entry")
  #else
  Bundle.main.url(forResource: "main", withExtension: "jsbundle")
  #endif
  }
```

## Usage

### Verifying installation

You can verify that the installation was successful by logging a value from [`expo-constants`](/versions/latest/sdk/constants.md).

-   Run `npx expo install expo-constants`
-   Then, run `npx expo run` and modify your app JavaScript code to add the following:

```tsx
import Constants from 'expo-constants';
console.log(Constants.systemFonts);
```

### Using Expo SDK packages

Once the `expo` package is installed and configured in your project, you can use `npx expo install` to add any other Expo module from the SDK. See [Using Libraries](/workflow/using-libraries.md) for more information.

### Expo modules included in the `expo` package

The following Expo modules are brought in as dependencies of the `expo` package:

-   [`expo-asset`](/versions/latest/sdk/asset.md) - A JavaScript-only package that builds around `expo-file-system` and provides a common foundation for assets across all Expo modules.
-   [`expo-constants`](/versions/latest/sdk/constants.md) - Provides access to the manifest.
-   [`expo-file-system`](/versions/latest/sdk/filesystem.md) - Interact with the device file system. Used by `expo-asset` and many other Expo modules. Commonly used directly by developers in application code.
-   [`expo-font`](/versions/latest/sdk/font.md) - Load fonts at runtime. This module is optional and can be safely removed. However, it is recommended if you use `expo-dev-client` for development and it is required by `@expo/vector-icons`.
-   [`expo-keep-awake`](/versions/latest/sdk/keep-awake.md) - Prevents your device from going to sleep while developing your app. This module is optional and can be safely removed.

To exclude any of these modules, refer to the following guide on [excluding modules from autolinking](/bare/installing-expo-modules.md#excluding-specific-modules-from-autolinking).

### Excluding specific modules from autolinking

If you need to exclude native code from Expo modules you are not using, but were installed by other dependencies, you can use the [`expo.autolinking.exclude`](/modules/autolinking.md#exclude) property in **package.json**:

```json
{
  "name": "...",
  "dependencies": {},
  "expo": {
    "autolinking": {
      "exclude": ["expo-keep-awake"]
    }
  }
}
```
