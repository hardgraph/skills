---
modificationDate: July 28, 2026
title: Get started with EAS Update
description: Learn how to get started with the setup required to configure and use EAS Update in your project.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas-update/getting-started/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas-update/getting-started/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Update
Pages in this section:
- [Introduction](https://docs.expo.dev/eas-update/introduction.md)
- [Get started](https://docs.expo.dev/eas-update/getting-started.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Get started with EAS Update

Learn how to get started with the setup required to configure and use EAS Update in your project.

Setting up EAS Update allows you to push critical bug fixes and improvements that your users need right away. This guide will walk you through the process of setting up EAS Update in a new or existing project.

> If you plan to use EAS Update with EAS Build, we recommend following the [EAS Build setup guide](/build/setup.md) before proceeding here. That said, [you can use EAS Update without any other EAS services](/eas-update/standalone-service.md).

#### Prerequisites

##### An Expo account

EAS Update is available to anyone with an Expo account, regardless of whether you pay for EAS or use the Free plan. You can sign up at [expo.dev/signup](https://expo.dev/signup).

Paid subscribers can publish updates to more users and use more bandwidth and storage. Learn more about different plans and benefits at [EAS pricing](https://expo.dev/pricing).

##### A React Native project

Don't have a project yet? It's quick and easy to create a "Hello world" app you can use with this guide. Run the following command to create a new project:

```sh
# npm
npx create-expo-app@latest my-app --template default@sdk-57

# yarn
yarn create expo-app my-app --template default@sdk-57

# pnpm
pnpm create expo-app my-app --template default@sdk-57

# bun
bun create expo my-app --template default@sdk-57
```

EAS Update also works well with projects created by `npx create-react-native-app`, `npx react-native`, `ignite-cli`, and other project bootstrapping tools.

##### Expo CLI and the Expo Metro Config

If you already run your project with `npx expo [command]` (for example, if you created it with `npx create-expo-app`), you're all set.

If you don't have the `expo` package in your project yet, install it by running the command below and [opt in to using Expo CLI and Metro Config](/bare/installing-expo-modules.md#configure-expo-cli-for-bundling-on-android-and-ios):

```sh
npx install-expo-modules@latest
```

If the command fails, refer to the [Installing Expo modules](/bare/installing-expo-modules.md#manual-installation) guide.

##### Use registerRootComponent instead of registerComponent

If you created your project with `npx create-expo-app`, or you don't call `registerRootComponent` in your app at all (for example, it's handled by Expo Router), you are all set. The following applies to projects created with other tools, such as React Native Community CLI.

We recommend apps using EAS Update use Expo's [`registerRootComponent`](/versions/latest/sdk/expo.md#registerrootcomponentcomponent) instead of React Native's `registerApplication`. This ensures Expo can configure React Native to load assets, such as images, that are included in updates. If you do not use `registerRootComponent`, your assets may not be available in release builds.

In a simple app created with React Native Community CLI, the diff would look like this:

After making that change, update your [`MainActivity`](/versions/latest/sdk/expo.md#rootregistercomponent-setup-for-existing-react-native-projects) and [`AppDelegate`](/versions/latest/sdk/expo.md#rootregistercomponent-setup-for-existing-react-native-projects) to use the module name `"main"` instead of your app name.

## Install the latest EAS CLI

EAS CLI is the command-line app you will use to interact with EAS services from your terminal. To install it, run the command:

```sh
# npm
npm install --global eas-cli

# yarn
yarn global add eas-cli

# pnpm
pnpm add --global eas-cli

# bun
bun add --global eas-cli
```

You can also use the above command to check if a new version of EAS CLI is available. We encourage you to always stay up to date with the latest version.

> We recommend using `npm` instead of `yarn` for global package installations. You may alternatively use `npx eas-cli@latest`. Remember to use that instead of `eas` whenever it's called for in the documentation.

## Log in to your Expo account

If you are already signed in to an Expo account using Expo CLI, you can skip the steps described in this section. If you are not, run the following command to log in:

```sh
eas login
```

You can check whether you are logged in by running `eas whoami`.

## Configure your project

Navigate to your project directory in your terminal and run the following command:

```sh
eas update:configure
```

#### What does this command do?

The `eas update:configure` command will update your **app.json** file with the `runtimeVersion` and `updates.url` properties, and add the `extra.eas.projectId` field if your project wasn't using any EAS services previously.

When you run `eas update:configure` in a project that doesn't use [CNG](/workflow/continuous-native-generation.md), you'll see the following changes to your native projects:

### Android

Inside the **android/app/src/main/AndroidManifest.xml** file, you'll see the following additions:

```xml
<meta-data android:name="expo.modules.updates.EXPO_UPDATE_URL" android:value="https://u.expo.dev/your-project-id"/>
<meta-data android:name="expo.modules.updates.EXPO_RUNTIME_VERSION" android:value="@string/expo_runtime_version"/>
```

The `EXPO_UPDATE_URL` value should contain your project's ID.

Inside **android/app/src/main/res/values/strings.xml**, you'll see the `expo_runtime_version` string entry in the `resources` object:

### iOS

Inside **ios/project-name/Supporting/Expo.plist**, you'll see the following additions:

```xml
<key>EXUpdatesRuntimeVersion</key>
<string>1.0.0</string>
<key>EXUpdatesURL</key>
<string>https://u.expo.dev/your-project-id</string>
```

The `EXUpdatesURL` value should contain your project's ID.

> If you use Xcode to create project builds, make sure that the `Expo.plist` file [is added to your Xcode project](https://developer.apple.com/documentation/xcode/managing-files-and-folders-in-your-xcode-project#Add-existing-files-and-folders-to-a-project).

## Configure the update channel

The channel property on a build allows you to point updates at specific types of builds. For example, it allows you to publish updates to a preview build without impacting your production deployment.

**If you are using EAS Build**, `eas update:configure` will set the update `channel` property on the `preview` and `production` profiles in **eas.json**. Set them manually if you use different profile names.

#### Example channel configuration in eas.json

```json
{
  "build": {
    "preview": {
      "channel": "preview"
      // ...
    },
    "production": {
      "channel": "production"
      // ...
    }
  }
}
```

  

**If you are not using EAS Build**, then you will need to configure the channel in **app.json** or in your native projects, depending on whether you use [CNG](/workflow/continuous-native-generation.md). When you create a build for a different environment, you will need to modify the channel to ensure your build pulls updates from the correct channel. Learn more using [EAS Update as a standalone service](/eas-update/standalone-service.md).

#### Configure update channels in app.json

If you use Continuous Native Generation (CNG), then you can configure the channel with the `updates.requestHeaders` property in your **app.json**:

```json
{
  "expo": {
    ... 
    "updates": {
      ... 
      "requestHeaders": {
        "expo-channel-name": "your-channel-name"
      }
      ... 
    }
    ... 
  }
}
```

The configuration will be applied the next time you run `npx expo prebuild`.

#### Configure update channels in an Android native project

In **AndroidManifest.xml**, you will need to add replace `your-channel-name` with the channel that matches your project:

```xml
<meta-data android:name="expo.modules.updates.UPDATES_CONFIGURATION_REQUEST_HEADERS_KEY" android:value="{"expo-channel-name":"your-channel-name"}"/>
```

#### Configure update channels in an iOS native project

In **Expo.plist**, you'll need to add the following, replacing `your-channel-name` with the channel that matches your project:

```xml
<key>EXUpdatesRequestHeaders</key>
<dict>
  <key>expo-channel-name</key>
  <string>your-channel-name</string>
</dict>
```

> If you use Xcode to create project builds, make sure that the `Expo.plist` file [is added to your Xcode project](https://developer.apple.com/documentation/xcode/managing-files-and-folders-in-your-xcode-project#Add-existing-files-and-folders-to-a-project).

## Create a build for the project

You need to create a build for Android or iOS. We recommend creating a build with the `preview` build profile first. See [Create your first build](/build/setup.md) on how to get started and set up [Internal distribution](/build/internal-distribution.md#using-internal-distribution) for your device or simulator.

Once you have a build running on your device or a simulator, you are ready to send an update.

## Make changes locally

After creating the build, you are ready to iterate on the project. Start a local development server with the following command:

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

Then, make any desired changes to your project's JavaScript, styling, or image assets.

## Publish an update

Publishing an update allows:

-   Fixing bugs and quickly updating non-native parts of a project instead of creating a new build
-   [Sharing a preview version of an app](/review/overview.md) using internal distribution

To publish an update with changes from your project, use the `eas update` command, and specify a name for the channel, a `message` to describe the update, and the `--environment` flag to specify which [EAS environment variables](/eas/environment-variables.md) to use (required in SDK 55 and later):

```sh
eas update --channel [channel-name] --message "[message]" --environment [environment-name]
```

#### How does publishing an update work?

When you publish an update with the `eas update` command, it generates a new update bundle and uploads it to the EAS servers. The channel name is used to locate the correct branch to publish a new update from other update branches. It is similar to how Git commit works, where every commit is on a Git branch.

For example, when an app is set to pull updates from the `preview` channel, you can publish an update for that build with `eas update --channel preview`. This will create a branch (called `preview` by default) on the `preview` channel. Behind the scenes, this command runs `npx expo export` to generate a **dist** directory and create a local update bundle. This update bundle is uploaded to EAS Update servers.

[In-depth guide on how EAS Update works](/eas-update/how-it-works.md) — Dive deep and learn how EAS Update works.

## Test the update

After the update is uploaded to EAS Update, you can use one of the following methods to test the update:

-   Use the Extensions tab in a [development build](/eas-update/expo-dev-client.md) to load the update.
-   Use [Expo Orbit](/review/with-orbit.md) to install and launch the update in a development build.
-   Implement a custom strategy with [Updates API](/versions/latest/sdk/updates.md) and [app config](/versions/latest/config/app.md#updates) to load updates in the app programmatically.
-   Manually test the update by force closing and reopening a release build of your app up to two times to download and apply the update. Updates for non-development builds (preview or production) are automatically downloaded to the device in the background when the app starts up and makes a request for any new updates. The update will be applied after it is downloaded and the app is restarted.

#### Something not working?

If your app is not updating as expected, see the [debugging guide](/eas-update/debug.md) for techniques to validate your configuration.

## Next steps

[Previewing updates](/eas-update/preview.md) — Learn how to iterate quickly by sharing updates for QA and testing.

[Deploying updates](/eas-update/deployment.md) — Learn about different deployment patterns for your project when using EAS Update.
