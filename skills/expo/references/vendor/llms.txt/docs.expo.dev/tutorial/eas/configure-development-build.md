---
modificationDate: July 29, 2026
title: Configure a development build in cloud
description: Learn how to configure a development build for a project using EAS Build.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/eas/configure-development-build/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/eas/configure-development-build/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > EAS tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/eas/introduction.md)
- [Configure development build](https://docs.expo.dev/tutorial/eas/configure-development-build.md) (this page)
- [Android development build](https://docs.expo.dev/tutorial/eas/android-development-build.md)
- [iOS development build for simulators](https://docs.expo.dev/tutorial/eas/ios-development-build-for-simulators.md)
- [iOS development build for devices](https://docs.expo.dev/tutorial/eas/ios-development-build-for-devices.md)
- [Multiple app variants](https://docs.expo.dev/tutorial/eas/multiple-app-variants.md)
- [Internal distribution build](https://docs.expo.dev/tutorial/eas/internal-distribution-builds.md)
- [Manage app versions](https://docs.expo.dev/tutorial/eas/manage-app-versions.md)
- [Android production build](https://docs.expo.dev/tutorial/eas/android-production-build.md)
- [iOS production build](https://docs.expo.dev/tutorial/eas/ios-production-build.md)
- [Share previews](https://docs.expo.dev/tutorial/eas/team-development.md)
- [Builds from GitHub](https://docs.expo.dev/tutorial/eas/using-github.md)
- [Next steps](https://docs.expo.dev/tutorial/eas/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Configure a development build in cloud

Learn how to configure a development build for a project using EAS Build.

In this chapter, we'll set up and configure a development build with EAS for our example app.

[Watch: How to configure a development build](https://www.youtube.com/watch?v=uQCE9zl3dXU) — Learn how to install expo-dev-client, configure build profiles in eas.json, and create your first development build with EAS Build.

## Understanding development builds

Let's start by learning about what are development builds and why we need them.

A [development build](/develop/development-builds/introduction.md) is a debug version of our project. It is optimized for quick iterations when creating an app. It contains the [`expo-dev-client`](/versions/latest/sdk/dev-client.md) library, which offers a robust and complete development environment. This setup allows us to integrate any native library or change code inside the [native directories](/workflow/overview.md#android-and-ios-native-projects) as required.

### Key highlights

> **Note:** If you are familiar with [Expo Go](/get-started/set-up-your-environment.md), think of a development build as a customizable version of Expo Go that is unique to the requirements of a project.

| Feature | Development builds | Expo Go |
| --- | --- | --- |
| **Development phase** | Offers web-like iteration speed for mobile app development. | Allows for quick iteration and testing of Expo SDK projects using the client app. |
| **Collaboration** | Facilitates team testing with shared native runtime. | Easy project sharing via QR codes on a device. |
| **Third-party libraries support** | Full support for any [third-party library](/workflow/using-libraries.md#third-party-libraries), including those that require custom native code. | Limited to libraries within the Expo SDK, not suitable for custom native dependencies. |
| **Customization** | Extensive customization with [config plugins](/config-plugins/introduction.md) and direct access to native code. | Limited customization with a focus on Expo SDK capabilities without direct native code modification. |
| **Intended use** | Ideal for full-fledged app development aimed at store deployment, offering a complete development environment and tools. | Ideal for learning, prototyping, and experimenting. Not recommended for production apps. |

## Install expo-dev-client library

To initialize our project for a development build, let's [`cd`](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Understanding_client-side_tools/Command_line#basic_built-in_terminal_commands) inside our project directory and run the following command to install the library:

```sh
# npm
npx expo install expo-dev-client

# yarn
yarn expo install expo-dev-client

# pnpm
pnpm expo install expo-dev-client

# bun
bun expo install expo-dev-client
```

### Start the development server

Run the `npx expo start` to start the [development server](/get-started/start-developing.md#start-a-development-server):

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

This command starts the metro bundler. In the terminal window, we see the QR code followed by the `Metro waiting on...` and a manifest URL:

Let's notice the changes installing the `expo-dev-client` library:

-   The manifest URL contains `expo-development-client` along with the app scheme
-   The development server now operates for a development build (instead of Expo Go).

Since we do not have a development build installed on one of our devices or an emulator/simulator, we can't run our project yet.

## Initialize a development build

### Install EAS CLI

We need to install the EAS command-line interface (CLI) tool as a global dependency on our local machine. Run the following command:

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

### Log in or sign up for an Expo account

> If you have an Expo account and are signed in using Expo CLI, skip this step. If you don't have an Expo account, [sign up here](https://expo.dev/signup) and proceed with the login command described below.

To log in, run the following command:

```sh
eas login
```

This command asks for our Expo account email or username and password to complete the login.

### Initialize and link the project to EAS

For any new project, the first step is to initialize and link it to the EAS servers. Run the following command:

```sh
eas init
```

On running, this command:

-   Requests verification of the account owner by entering our Expo account credentials and asks if we want to create a new EAS project:

```sh
✔ Which account should own this project? > your-username
✔ Would you like to create a project for @your-username/sticker-smash? … yes
✔ Created @your-username/sticker-smash
✔ Project successfully linked (ID: XXXX-XX-XX-XXXX) (modified app.json)
```

-   Creates EAS project and provides a link to that project which we can open in the EAS dashboard:

-   Generates a unique `projectId` and links this EAS project to the example app on our development machine.
-   Modifies **app.json** to include [`extra.eas.projectId`](/versions/latest/sdk/constants.md#easconfig) and updates its value with the unique ID created.

#### What is projectId in app.json?

When `eas init` runs, it associates a unique identifier for our project in **app.json** under `extra.eas.projectId`. The value of this property is used to identify our project on EAS servers.

```json
{
  "extra": {
    "eas": {
      "projectId": "0cd3da2d-xxx-xxx-xxx-xxxxxxxxxx"
    }
  }
}
```

## Configure project for EAS Build

To set up our project for EAS Build, run the following command:

```sh
eas build:configure
```

On running, this command:

-   Prompts to select a platform: **Android**, **iOS**, or **All**. Since we are creating Android and iOS apps, let's select **All**.
-   Creates **eas.json** in the root of our project's directory with the following configuration:

```json
{
  "cli": {
    "version": ">= 16.18.0",
    "appVersionSource": "remote"
  },
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal"
    },
    "preview": {
      "distribution": "internal"
    },
    "production": {
      "autoIncrement": true
    }
  },
  "submit": {
    "production": {}
  }
}
```

This is the default configuration for **eas.json** in a new project. It does two things:

-   Defines the current EAS CLI version.
-   Adds three [build profiles](/build/eas-json.md#build-profiles): `development`, `preview`, and `production`.

#### Explore development profile further

**eas.json** is a collection of different build profiles. Each profile is tailored with distinct configurations to produce specific build types. These profiles can also include platform-specific settings for Android or iOS.

Currently, our focus is on the `development` profile, which includes the following configuration:

-   [`developmentClient`](/eas/json.md#developmentclient): Enabled (`true`) for creating a debug build. It loads the app using the `expo-dev-client` library, which provides development tools and generates a build artifact for device or emulator/simulator installation and allows using the app for local development as it supports updating the JavaScript on the fly.
-   [`distribution`](/eas/json.md#distribution): Configured as `internal` to indicate that we want to share the build internally (instead of uploading it on app stores).

> **Note**: Builds offer extensive customization options, including platform-specific settings and the ability to extend configurations across different build profiles. Learn more about [customizing build profiles](/build/eas-json.md#build-profiles).

## Summary

Chapter 1: Configure development build in cloud

We successfully used the EAS CLI to initialize, and configure our project, link it to EAS servers, and prepare a development build.

In the next chapter, let's create a development build for Android, install it on a device and an emulator, and get it running with the development server.

[Next: Chapter 2: Create and run a cloud build for Android](/tutorial/eas/android-development-build.md)
