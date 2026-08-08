---
modificationDate: July 22, 2026
title: Install app variants on the same device
description: Learn how to install multiple variants of an app on the same device.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build-reference/variants/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build-reference/variants/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Build > Reference
Pages in this section:
- [Build lifecycle hooks](https://docs.expo.dev/build-reference/npm-hooks.md)
- [Using private npm packages](https://docs.expo.dev/build-reference/private-npm-packages.md)
- [Git submodules](https://docs.expo.dev/build-reference/git-submodules.md)
- [Using npm cache with Yarn 1 (Classic)](https://docs.expo.dev/build-reference/npm-cache-with-yarn.md)
- [Set up EAS Build with a monorepo](https://docs.expo.dev/build-reference/build-with-monorepos.md)
- [Build APKs for Android Emulators and devices](https://docs.expo.dev/build-reference/apk.md)
- [Build for iOS Simulators](https://docs.expo.dev/build-reference/simulators.md)
- [App version management](https://docs.expo.dev/build-reference/app-versions.md)
- [Troubleshoot build errors and crashes](https://docs.expo.dev/build-reference/troubleshooting.md)
- [Install app variants on the same device](https://docs.expo.dev/build-reference/variants.md) (this page)
- [iOS capabilities](https://docs.expo.dev/build-reference/ios-capabilities.md)
- [Run EAS Build locally](https://docs.expo.dev/build-reference/local-builds.md)
- [Cache dependencies](https://docs.expo.dev/build-reference/caching.md)
- [Android build process](https://docs.expo.dev/build-reference/android-builds.md)
- [iOS build process](https://docs.expo.dev/build-reference/ios-builds.md)
- [Configuration process](https://docs.expo.dev/build-reference/build-configuration.md)
- [Server infrastructure](https://docs.expo.dev/build-reference/infrastructure.md)
- [iOS App Extensions](https://docs.expo.dev/build-reference/app-extensions.md)
- [Ignore files via .easignore](https://docs.expo.dev/build-reference/easignore.md)
- [npx testflight](https://docs.expo.dev/build-reference/npx-testflight.md)
- [Repack app](https://docs.expo.dev/build-reference/repack.md)
- [Limitations](https://docs.expo.dev/build-reference/limitations.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Install app variants on the same device

Learn how to install multiple variants of an app on the same device.

When creating [development, preview, and production builds](/build/eas-json.md#common-use-cases), installing these build variants simultaneously on the same device is common. This allows working in development, previewing the next version of the app, and running the production version on a device without needing to uninstall and reinstall the app.

This guide provides the steps required to configure multiple (development and production) variants to install and use them on the same device.

#### Prerequisites

##### Unique Application ID or Bundle Identifier per variant

To have multiple variants of an app installed on your device, each variant must have a unique [Application ID (Android)](/versions/latest/config/app.md#package) or [Bundle Identifier (iOS)](/versions/latest/config/app.md#bundleidentifier).

## Configure development and production variants

You have created a project using Expo tooling, and now you want to create a development and a production build. Your project's **app.json** may have the following configuration:

```json
{
  "expo": {
    "name": "MyApp",
    "slug": "my-app",
    "ios": {
      "bundleIdentifier": "com.myapp"
    },
    "android": {
      "package": "com.myapp"
    }
  }
}
```

If your project has EAS Build configured, the **eas.json** also has a similar configuration as shown below:

```json
{
  "build": {
    "development": {
      "developmentClient": true
    },
    "production": {}
  }
}
```

### Convert app.json to app.config.js

To have multiple variants of the app installed on the same device, rename the **app.json** to **app.config.js** and export the configuration as shown below:

```js
export default {
  name: 'MyApp',
  slug: 'my-app',
  ios: {
    bundleIdentifier: 'com.myapp',
  },
  android: {
    package: 'com.myapp',
  },
};
```

In **app.config.js**, add an environment variable called `IS_DEV` to switch the `android.package` and `ios.bundleIdentifier` for each variant based on the variable:

```js
const IS_DEV = process.env.APP_VARIANT === 'development';

export default {
  name: IS_DEV ? 'MyApp (Dev)' : 'MyApp',
  slug: 'my-app',
  ios: {
    bundleIdentifier: IS_DEV ? 'com.myapp.dev' : 'com.myapp',
  },
  android: {
    package: IS_DEV ? 'com.myapp.dev' : 'com.myapp',
  }
};
```

In the above example, the environment variable `IS_DEV` is used to differentiate between the development and production environment. Based on its value, the different Application IDs or Bundle Identifiers are set for each variant.

#### Additional app variant customizations

You can customize other aspects of your app on a per-variant basis. You can swap any configuration that you used previously in **app.json** using the same approach as above.

**Examples:**

-   If you are using a library that requires you to register your application identifier with an external service to use its SDK, such as Google Maps or Firebase Cloud Messaging (FCM), you'll need to have a separate configuration for that API for the `android.package` and `ios.bundleIdentifier`.
-   If you're using [development builds](/develop/development-builds/introduction.md), you can configure the `expo-dev-client` plugin to disable the app scheme used by Expo CLI and EAS Update QR codes in non-development builds. This ensures that those URLs will always launch the development build, regardless of your device's defaults:

```js
plugins: [
  [
    'expo-dev-client',
    {
      addGeneratedScheme: !!IS_DEV,
    },
  ],
],
```

### Configuration for EAS Build

In **eas.json**, set the `APP_VARIANT` environment variable to run builds with the **development** profile by using the `env` property:

```json
{
  "build": {
    "development": {
      "developmentClient": true,
      "env": {
        "APP_VARIANT": "development"
      }
    },
    "production": {}
  }
}
```

Now, when you run `eas build --profile development`, the environment variable `APP_VARIANT` is set to `development` when evaluating **app.config.js** both locally and on the EAS Build builder.

### Using the development server

When you start your development server, you'll need to run `APP_VARIANT=development npx expo start` (or the platform equivalent if you use Windows).

A shortcut for this is to add the following script to your **package.json**:

```json
{
  "scripts": {
    "dev": "APP_VARIANT=development npx expo start"
  }
}
```

### Using production variant

When you run `eas build --profile production` the `APP_VARIANT` variable environment is not set, and the build runs as the production variant.

> **Note**: If you use EAS Update to publish JavaScript updates of your app, you should be cautious to set the correct environment variables for the app variant that you are publishing for when you run the `eas update` command. See the EAS Build [Environment variables and secrets](/build/updates.md) for more information.

### Build and run multiple app variants locally

If you [build your app locally](/guides/local-app-overview.md) with [`expo run:android|ios`](/workflow/continuous-native-generation.md#usage), set the `APP_VARIANT` environment variable when you run the `expo run` command. For example, to compile the `development` variant for iOS as a debug build, run:

```sh
# npm
APP_VARIANT=development npx expo run:ios

# yarn
APP_VARIANT=development yarn expo run:ios

# pnpm
APP_VARIANT=development pnpm expo run:ios

# bun
APP_VARIANT=development bun expo run:ios
```

`APP_VARIANT` changes only the app's name and package name on Android and bundle identifier on iOS. It does not affect how the binary is compiled. When you switch variants, the existing **android** and **ios** directories still reflect the previous variant. If a native directory is already present, `expo run` skips regenerating it and compiles it as-is.

To switch variants, you need to regenerate the native directories again with `prebuild --clean` before compiling the app. In the example below, the `development` variant (existing debug build) is switched to `test`, which is another debug build. To regenerate the **android** and **ios** directories and then compile the `test` variant, set `APP_VARIANT` on both commands so that the clean prebuild and the compilation use the same variant:

```sh
# npm
APP_VARIANT=test npx expo prebuild --clean
APP_VARIANT=test npx expo run:ios

# yarn
APP_VARIANT=test yarn expo prebuild --clean
APP_VARIANT=test yarn expo run:ios

# pnpm
APP_VARIANT=test pnpm expo prebuild --clean
APP_VARIANT=test pnpm expo run:ios

# bun
APP_VARIANT=test bun expo prebuild --clean
APP_VARIANT=test bun expo run:ios
```

The **android** and **ios** directories are managed by [Continuous Native Generation](/workflow/continuous-native-generation.md), so regenerating them is safe when they are listed in your **.gitignore**.

### In an existing React Native project

> If you want to use your [app config file](/workflow/configuration.md) as the source of truth for your app configuration (including bundle identifiers and package names for variants), you need to migrate to [Continuous Native Generation (CNG)](/workflow/continuous-native-generation.md) and add **android** and **ios** directories to your **.gitignore** file. This is especially important if you started with a React Native CLI project and added custom native code. This ensures that the native project configuration doesn't take precedence over your app config, especially when using bundle ID lookup behavior. Without this, you may experience issues where the wrong app variant runs or development builds aren't detected properly. Alternatively, you can explicitly opt for the Android flavors and iOS schemes as described below.

#### Android

In **android/app/build.gradle**, create a separate flavor for every build profile from **eas.json** that you want to build.

```groovy
android {
    ... 
    flavorDimensions "env"
    productFlavors {
        production {
            dimension "env"
            applicationId 'com.myapp'
        }
        development {
            dimension "env"
            applicationId 'com.myapp.dev'
        }
    }
    ... 
}
```

> **Note**: Currently, EAS CLI supports only the `applicationId` field. If you use `applicationIdSuffix` inside `productFlavors` or `buildTypes` sections then this value will not be detected correctly.

Assign Android flavors to EAS Build profiles by specifying a `gradleCommand` in the **eas.json**:

```json
{
  "build": {
    "development": {
      "android": {
        "gradleCommand": ":app:assembleDevelopmentDebug"
      }
    },
    "production": {
      "android": {
        "gradleCommand": ":app:bundleProductionRelease"
      }
    }
  }
}
```

By default, every flavor can be built in either debug or release mode. If you want to restrict some flavor to a specific mode, see the snippet below, and modify **build.gradle**.

```groovy
android {
    ... 
    variantFilter { variant ->
        def validVariants = [
                ["production", "release"],
                ["development", "debug"],
        ]
        def buildTypeName = variant.buildType*.name
        def flavorName = variant.flavors*.name

        def isValid = validVariants.any { flavorName.contains(it[0]) && buildTypeName.contains(it[1]) }
        if (!isValid) {
            setIgnore(true)
        }
    }
    ... 
}
```

The rest of the configuration at this point is not specific to EAS, it's the same as it would be for any Android project with flavors. There are a few common configurations that you might want to apply to your project:

-   To change the name of the app built with the development profile, create a **android/app/src/development/res/value/strings.xml** file:
    
    ```xml
    <resources>
        <string name="app_name">MyApp - Dev</string>
    </resources>
    ```
    
-   To change the icon of the app built with the development profile, create **android/app/src/development/res/mipmap-\*** directories with appropriate assets (you can copy them from **android/app/src/main/res** and replace the icon files).
-   To specify **google-services.json** for a specific flavor, put it in the **android/app/src/{flavor}/google-services.json** file.
-   To configure sentry, add `project.ext.sentryCli = [ flavorAware: true ]` to **android/app/build.gradle** and name your properties file **android/sentry-{flavor}-{buildType}.properties** (for example, **android/sentry-production-release.properties**)

#### iOS

Assign a different `scheme` to every build profile in **eas.json**:

```json
{
  "build": {
    "development": {
      "ios": {
        "buildConfiguration": "Debug",
        "scheme": "myapp-dev"
      }
    },
    "production": {
      "ios": {
        "buildConfiguration": "Release",
        "scheme": "myapp"
      }
    }
  }
}
```

**Podfile** should have a target defined like this:

```ruby
target 'myapp' do
  ... 
end
```

Replace it with an abstract target, where the common configuration can be copied from the old target:

```ruby
abstract_target 'common' do
  # put common target configuration here

  target 'myapp' do
  end

  target 'myapp-dev' do
  end
end
```

Open project in Xcode, click on the project name in the navigation panel, right click on the existing target, and click "Duplicate":

Rename the target to something more meaningful, for example, `myapp copy` -> `myapp-dev`.

Configure a scheme for the new target:

-   Go to `Product` -> `Scheme` -> `Manage schemes`.
-   Find scheme `myapp copy` on the list.
-   Change scheme name `myapp copy` -> `myapp-dev`.
-   By default, the new scheme should be marked as shared, but Xcode does not create `.xcscheme` files. To fix that, uncheck the "Shared" checkbox and check it again, after that new `.xcscheme` file should show up in the **ios/myapp.xcodeproj/xcshareddata/xcschemes** directory.

By default, the newly created target has separate **Info.plist** file (in the above example, it's **ios/myapp copy-Info.plist**). To simplify your project we recommend using the same file for all targets:

-   Delete **./ios/myapp copy-Info.plist**.
-   Click on the new target.
-   Go to `Build Settings` tab.
-   Find `Packaging` section.
-   Change **Info.plist** value - **myapp copy-Info.plist** -> **myapp/Info.plist**.
-   Change `Product Bundle Identifier`.

To change the display name:

-   Open **Info.plist** and add key `Bundle display name` with value `$(DISPLAY_NAME)`.
-   Open `Build Settings` for both targets and find `User-Defined` section.
-   Add key `DISPLAY_NAME` with the name you want to use for that target.

To change the app icon:

-   Create a new image set (you can create it from the existing image set for the current icon, it's usually named `AppIcon`)
-   Open `Build Settings` for the target that you want to change icon.
-   Find `Asset Catalog Compiler - Options` section.
-   Change `Primary App Icon Set Name` to the name of the new image set.
