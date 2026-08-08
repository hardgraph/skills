---
modificationDate: July 29, 2026
title: Upgrade Expo SDK
description: Learn how to incrementally upgrade the Expo SDK version in your project.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/workflow/upgrading-expo-sdk-walkthrough/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/workflow/upgrading-expo-sdk-walkthrough/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > More
Pages in this section:
- [Upgrade Expo SDK](https://docs.expo.dev/workflow/upgrading-expo-sdk-walkthrough.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Upgrade Expo SDK

Learn how to incrementally upgrade the Expo SDK version in your project.

> We recommend upgrading SDK versions incrementally, one at a time. Doing so will help you pinpoint breakages and issues that arise during the upgrade process.

With a new SDK release, the latest version enters the current release status. This applies to Expo Go as it only supports the latest SDK version and previous versions are no longer supported. We recommend using [development builds](/develop/development-builds/introduction.md) for production apps as the backwards compatibility for older SDK versions on EAS services tends to be much longer, but not forever.

If you are looking to install a specific version of Expo Go, visit [expo.dev/go](https://expo.dev/go) or use [`expo-go` CLI](/develop/tools.md#expo-go-cli). It supports downloads for Android devices/emulators and iOS simulators. However, due to iOS platform restrictions, only the latest version of Expo Go is available for installation on physical iOS devices.

## How to upgrade to the latest SDK version

### Upgrade with an AI coding agent

If you use an AI coding agent, install [Expo Skills](/skills.md) and use the [`expo-upgrade` skill](/skills.md#available-expo-skills). The skill provides guidelines for upgrading Expo SDK versions and fixing dependency issues. Review any proposed changes and check the SDK changelog for version-specific instructions.

[expo-upgrade](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-upgrade/SKILL.md) — Guidelines for upgrading Expo SDK versions and fixing dependency issues.

### Upgrade manually

#### Upgrade the Expo SDK

Install the new version of the Expo package:

```sh
# npm
npm install expo@^57.0.0

# yarn
yarn add expo@^57.0.0

# pnpm
pnpm add expo@^57.0.0

# bun
bun install expo@^57.0.0
```

Depending on which SDK you're upgrading to, substitute `expo@^57.0.0` with the version range of the Expo SDK version you're targeting. For example, `expo@^57.0.0` stands for SDK 57.

#### Upgrade dependencies

Upgrade all dependencies to match the installed SDK version. Then run [`expo-doctor`](/develop/tools.md#expo-doctor) command to check for common problems.

```sh
npx expo install --fix
npx expo-doctor
```

#### Update native projects

-   **If you use [Continuous Native Generation](/workflow/continuous-native-generation.md)**: Delete the **android** and **ios** directories if you generated them for a previous SDK version in your local project directory. They'll be re-generated next time you run a build, either with `npx expo run:ios`, `npx expo prebuild`, or with EAS Build.
-   **If you don't use [Continuous Native Generation](/workflow/continuous-native-generation.md)**: Run `npx pod-install` if you have an **ios** directory. Apply any relevant changes from the [Native project upgrade helper](/bare/upgrade.md). Alternatively, you could consider [adopting prebuild](/guides/adopting-prebuild.md) for easier upgrades in the future.

#### Follow the release notes for any other instructions

Read the [SDK changelogs](/workflow/upgrading-expo-sdk-walkthrough.md#sdk-changelogs) for the SDK version you are upgrading to. They contain important information about breaking changes, deprecations, and other changes that may affect your app. Refer to the "Upgrading your app" section at the bottom of the release notes page for any additional instructions.

## SDK changelogs

Each SDK announcement release notes post contains information about deprecations, breaking changes, and anything else that might be unique to that particular SDK version. When upgrading, be sure to check these out to make sure you don't miss anything.

-   **SDK 57**: [Release notes](https://expo.dev/changelog/sdk-57)
-   **SDK 56**: [Release notes](https://expo.dev/changelog/sdk-56)
-   **SDK 55**: [Release notes](https://expo.dev/changelog/sdk-55)
-   **SDK 54**: [Release notes](https://expo.dev/changelog/sdk-54)

### Deprecated SDK version changelogs

The following blog posts may included outdated information, but they are still useful for reference if you happen to fall far behind on SDK upgrades.

#### See a full list of deprecated SDK release changelogs

-   **SDK 53**: [Release notes](https://expo.dev/changelog/sdk-53)
-   **SDK 52**: [Release notes](https://expo.dev/changelog/2024-11-12-sdk-52)
    -   **React Native 0.77 is available with Expo SDK 52**. To upgrade, see these [Release notes](https://expo.dev/changelog/2025/01-21-react-native-0.77).
-   **SDK 51**: [Release notes](https://expo.dev/changelog/2024-05-07-sdk-51)
-   **SDK 50**: [Release notes](https://expo.dev/changelog/2024-01-18-sdk-50)
-   **SDK 49**: [Release notes](https://blog.expo.dev/expo-sdk-49-c6d398cdf740)
-   **SDK 48**: [Release notes](https://blog.expo.dev/expo-sdk-48-ccb8302e231)
-   **SDK 47**: [Release notes](https://blog.expo.dev/expo-sdk-47-a0f6f5c038af)
-   **SDK 46**: [Release notes](https://blog.expo.dev/expo-sdk-46-c2a1655f63f7)
-   **SDK 45**: [Release notes](https://blog.expo.dev/expo-sdk-45-f4e332954a68)
-   **SDK 44**: [Release notes](https://blog.expo.dev/expo-sdk-44-4c4b8306584a)
-   **SDK 43**: [Release notes](https://blog.expo.dev/expo-sdk-43-aa9b3c7d5541)
-   **SDK 42**: [Release notes](https://blog.expo.dev/expo-sdk-42-579aee2348b6)
-   **SDK 41**: [Release notes](https://blog.expo.dev/expo-sdk-41-12cc5232f2ef)
-   **SDK 40**: [Release notes](https://dev.to/expo/expo-sdk-40-is-now-available-1in0)
-   **SDK 39**: [Release notes](https://dev.to/expo/expo-sdk-39-is-now-available-1lm8)
-   **SDK 38**: [Release notes](https://dev.to/expo/expo-sdk-38-is-now-available-5aa0)
-   **SDK 37**: [Release notes](https://dev.to/expo/expo-sdk-37-is-now-available-69g)
-   **SDK 36**: [Release notes](https://blog.expo.dev/expo-sdk-36-is-now-available-b91897b437fe)
-   **SDK 35**: [Release notes](https://blog.expo.dev/expo-sdk-35-is-now-available-beee0dfafbf4)
