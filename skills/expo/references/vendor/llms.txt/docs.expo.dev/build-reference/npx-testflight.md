---
modificationDate: July 21, 2026
title: npx testflight command
description: A single command that allows you to build, sign, and submit your iOS app to TestFlight.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build-reference/npx-testflight/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build-reference/npx-testflight/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Configuration process](https://docs.expo.dev/build-reference/build-configuration.md)
- [Server infrastructure](https://docs.expo.dev/build-reference/infrastructure.md)
- [iOS App Extensions](https://docs.expo.dev/build-reference/app-extensions.md)
- [Ignore files via .easignore](https://docs.expo.dev/build-reference/easignore.md)
- [npx testflight](https://docs.expo.dev/build-reference/npx-testflight.md) (this page)
- [Repack app](https://docs.expo.dev/build-reference/repack.md)
- [Limitations](https://docs.expo.dev/build-reference/limitations.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# npx testflight command

A single command that allows you to build, sign, and submit your iOS app to TestFlight.

[`npx testflight`](https://www.npmjs.com/package/testflight) is a CLI tool that walks you through building, signing, and submitting your iOS app to TestFlight.

#### Prerequisites

##### A React Native iOS project

A React Native iOS project you want to deploy to TestFlight.

##### Apple Developer account

A paid [Apple Developer account](https://developer.apple.com/account/) is required for TestFlight distribution.

##### Expo account

Sign up for an [Expo](https://expo.dev/signup) account, if you haven't already.

## Run `npx testflight` command

Run the following command inside your project's root directory:

```sh
npx testflight
```

The command workflow is interactive and walks you through the following prompts using the latest EAS CLI version:

-   **Initialize or detect a linked EAS project.** If you are running this command in a new project, the CLI will create a new EAS project using the slug from your app config file. If the CLI detects that the project is created on EAS already, it will continue to use the same slug.
-   **Confirm the bundle identifier.** If you are running this command in a new project, you can enter a new identifier, or for subsequent command runs, accept the one the CLI detects. The wizard also asks whether your app uses standard or exempt encryption. When this command is run subsequently, the [`buildNumber`](/tutorial/eas/manage-app-versions.md#understanding-developer-facing-and-user-facing-app-versions) automatically increments.
-   **Sign in to Apple Developer.** Provide your Apple ID, complete two-factor authentication, and allow the CLI to create new or reuse existing distribution certificates or provisioning profiles.
-   **Generate credentials.** If EAS does not already manage credentials for the bundle identifier, the CLI creates or updates the distribution certificate and provisioning profile for you.
-   **Create a production build**. It starts an iOS build using the default EAS [`production` profile](/build/eas-json.md#production-builds) to create an iOS archive (**.ipa**) file.
-   **Verify App Store Connect access.** The submit step checks for an [App Store Connect API key](https://expo.fyi/creating-asc-api-key) and creates one if needed.
-   **Submits the app to TestFlight.** Uploads the resulting **.ipa** file to App Store Connect and enables TestFlight distribution for your team's [internal testing group](/submit/testflight.md#set-up-internal-testing).

You will receive build and submission status updates throughout the process inside the terminal window. Within the App Store Connect dashboard, you can manage testers and distribution.

> **Note**: Every prompt mirrors the EAS Build and EAS Submit flows, so you can answer the same way you would when running eas build or eas submit separately. This means, during the build and submission process, the EAS dashboard links will be generated and you can use them to view the process. After the submission process is successfully completed, you will get the link to the App Store Connect, which you can use to view your submission to TestFlight.

## Why use `npx testflight`

-   Saves developer time without requiring separate build-and-submit steps
-   Handles Apple credentials, provisioning profiles, and App Store Connect API keys through guided prompts with EAS CLI
-   Produces a new build and submits it to TestFlight without running separate commands
-   Works well on shared machines or CI runners where installing global packages is inconvenient

## When to use `npx testflight`

-   Ship a TestFlight build quickly from your local machine
-   Trigger one or many builds to TestFlight without configuring a full CI workflow
-   Have internal test groups and want to make the latest changes in your app available as soon as possible
-   Let EAS handle certificates, provisioning profiles, and API keys automatically

## Common question

#### Can I run npx testflight command in non-interactive mode?

Yes, when you provide `ascAppId` in the `submit.production` profile in the **eas.json**, the `npx testflight` command will bypass the process of ensuring your app exists on App Store Connect.

```json
{
  "submit": {
    "production": {
      "ios": {
        "ascAppId": "your-app-store-connect-app-id"
      }
    }
  }
}
```

To learn more about how to find your ascAppId, see [these steps in the Submit to the Apple App Store](/submit/ios.md#how-to-find-ascappid).
