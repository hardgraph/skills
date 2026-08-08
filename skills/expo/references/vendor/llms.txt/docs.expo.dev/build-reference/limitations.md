---
modificationDate: July 29, 2026
title: EAS Build limitations
description: Learn about the current limitations of EAS Build.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build-reference/limitations/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build-reference/limitations/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [npx testflight](https://docs.expo.dev/build-reference/npx-testflight.md)
- [Repack app](https://docs.expo.dev/build-reference/repack.md)
- [Limitations](https://docs.expo.dev/build-reference/limitations.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# EAS Build limitations

Learn about the current limitations of EAS Build.

EAS Build is designed to work for any React Native project. However, it is good to be aware of certain limitations that we plan to address since they could prevent you from being able to use the service for your applications or might cause an inconvenience.

## Fixed memory and CPU limits on build worker servers

The resources available might be insufficient to build your app if your build process requires a significant amount of memory. In this case, consider using a [`large` resource class](/eas/json.md#resourceclass) in the **eas.json**. See [Android-specific resource class](/build-reference/infrastructure.md#android-build-server-configurations) and [iOS-specific resource class](/build-reference/infrastructure.md#ios-build-server-configurations).

See [Server infrastructure reference](/build-reference/infrastructure.md) for more information. It contains the most up-to-date information about the current specifications of the Android (Ubuntu) and iOS (macOS) build servers.

## Limited dependency caching

Build jobs for Android install npm and Maven dependencies from a local cache. Build jobs for iOS install npm dependencies from a local cache, and CocoaPods artifacts from a cache server.

Intermediate artifacts like **node_modules** directories are not cached and restored (for example, based on **package-lock.json** or **yarn.lock**), but if you commit them to your Git repository then they will be uploaded to build servers.

See [dependency caching](/build-reference/caching.md) for more information.

## Maximum build duration

If your build takes longer to run than the maximum duration allowed for your plan, it will be canceled. See [EAS pricing](https://expo.dev/pricing#builds) for the build timeout included with each plan, which is subject to change.

## Maximum number of pending builds is 50 per platform per account

If you have more than 50 builds pending for a platform, new builds will be rejected until the number of pending builds drops below the limit.

## Package managers with workspaces support may require special setup

> **Note**: Official guidance for package managers other than Bun, npm, pnpm, and Yarn is limited.

EAS Build supports monorepos managed with package managers supporting workspaces. However, third-party monorepo or workspaces tooling may not work as expected or require additional setup. Increased complexity is common when setting up and configuring monorepos and workspaces. Check whether your tools and libraries work well within a monorepo before setting one up. See [Work with monorepos](/guides/monorepos.md).

## Get notified about changes

To be notified as progress is made on these items, you can subscribe to the EAS newsletter on [expo.dev/eas](https://expo.dev/eas).
