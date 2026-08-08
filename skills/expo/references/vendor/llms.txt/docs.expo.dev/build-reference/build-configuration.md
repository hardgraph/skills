---
modificationDate: July 22, 2026
title: Build configuration process
description: Learn how EAS CLI configures a project for EAS Build.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build-reference/build-configuration/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build-reference/build-configuration/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Install app variants on the same device](https://docs.expo.dev/build-reference/variants.md)
- [iOS capabilities](https://docs.expo.dev/build-reference/ios-capabilities.md)
- [Run EAS Build locally](https://docs.expo.dev/build-reference/local-builds.md)
- [Cache dependencies](https://docs.expo.dev/build-reference/caching.md)
- [Android build process](https://docs.expo.dev/build-reference/android-builds.md)
- [iOS build process](https://docs.expo.dev/build-reference/ios-builds.md)
- [Configuration process](https://docs.expo.dev/build-reference/build-configuration.md) (this page)
- [Server infrastructure](https://docs.expo.dev/build-reference/infrastructure.md)
- [iOS App Extensions](https://docs.expo.dev/build-reference/app-extensions.md)
- [Ignore files via .easignore](https://docs.expo.dev/build-reference/easignore.md)
- [npx testflight](https://docs.expo.dev/build-reference/npx-testflight.md)
- [Repack app](https://docs.expo.dev/build-reference/repack.md)
- [Limitations](https://docs.expo.dev/build-reference/limitations.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Build configuration process

Learn how EAS CLI configures a project for EAS Build.

In this guide, you will learn what happens when EAS CLI configures your project with `eas build:configure` (or `eas build`, which runs this same process if the project is not yet configured).

EAS CLI performs the following steps when configuring your project:

## Ask you about the platform(s) to configure

When you run the command for the first time, it will initialize your EAS project and ask you to select the platform(s) you want to configure. If you only want to use EAS Build for a single platform, that's fine. If you change your mind, you can come back and configure the other later.

## Create eas.json

The command will create an **eas.json** file in the root directory with the default configuration. It looks something like this:

```json
{
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal"
    },
    "preview": {
      "distribution": "internal"
    },
    "production": {}
  }
}
```

If you have an [existing React Native project](/bare/overview.md), it will look a bit different.

This is your EAS Build configuration. It defines three build profiles named `"development"`, `"preview"`, and `"production"` (you can have multiple build profiles like `"production"`, `"debug"`, `"testing"`, and so on) for each platform. If you want to learn more about **eas.json** see the [Configuration with **eas.json**](/build/eas-json.md) page.

## Configure the project

This step varies depending on the project type you have.

3.1

### Initialization complete

This completes the initialization of your project to be compatible with EAS Build.

3.2

### Expo project

If you haven't configured your **app.json** with `android.package` and/or `ios.bundleIdentifier` yet, EAS CLI will prompt you to specify them when you create your first build.

-   `android.package` will be used as the Android application ID which is used to identify your app on the Google Play Store
-   `ios.bundleIdentifier` will be used to identify you app on the Apple App Store

In the example above, the `eas build --platform android` command prompts to set the Android application ID. If you run the command with `--platform ios`, it will prompt you to set the iOS bundle identifier.

3.3

### Existing React Native project

There are no additional steps for existing React Native projects.

## Next steps

That's all there is to configuring a project to be compatible with EAS Build. There is one more step, if you set `cli.requireCommit` to `true` in your **eas.json** — you'll be prompted to commit all the changes we made for you. You can choose to review them before committing, and you can either specify the git commit message or use a default message.
