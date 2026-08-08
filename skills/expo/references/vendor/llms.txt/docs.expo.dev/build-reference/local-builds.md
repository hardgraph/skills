---
modificationDate: May 23, 2026
title: Run EAS Build locally with local flag
description: Learn how to use EAS Build locally on your machine or a custom infrastructure using the --local flag.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build-reference/local-builds/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build-reference/local-builds/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Run EAS Build locally](https://docs.expo.dev/build-reference/local-builds.md) (this page)
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

# Run EAS Build locally with local flag

Learn how to use EAS Build locally on your machine or a custom infrastructure using the --local flag.

You can run the same build process that is typically run on the EAS Build servers directly on your machine by using the `eas build --local` flag. This is a useful way to debug build failures that are happening on your cloud builds, which you may not be able to reproduce without running the same set of steps.

```sh
eas build --platform android --local
eas build --platform ios --local
```

#### Prerequisites

##### Authenticate with Expo

Run `eas login`, or alternatively, set `EXPO_TOKEN` [using token-based authentication](/accounts/programmatic-access.md).

## Use cases for local builds

-   [Debugging](/build-reference/local-builds.md#use-local-builds-for-debugging) build failures on EAS servers.
-   Company policies that restrict the use of third-party CI/CD services. With local builds, the entire process runs on your infrastructure and the only communication with EAS servers is:
    -   to make sure project `@account/slug` exists
    -   if you are using managed credentials to download them

## Use local builds for debugging

If you encounter build failures on EAS servers and you're unable to determine the cause from inspecting the logs, you may find it helpful to debug the issue locally. To simplify that process we support several environment variables to configure the local build process.

-   `EAS_LOCAL_BUILD_SKIP_CLEANUP=1` - Set this to disable cleaning up the working directory after the build process is finished.
-   `EAS_LOCAL_BUILD_WORKINGDIR` - Specify the working directory for the build process, by default it's somewhere (it's platform dependent) in **/tmp** directory.
-   `EAS_LOCAL_BUILD_ARTIFACTS_DIR` - The directory where artifacts are copied after a successful build. By default, these files are copied to the current directory, which may be undesirable if you are running many consecutive builds.

If you use `EAS_LOCAL_BUILD_SKIP_CLEANUP` and `EAS_LOCAL_BUILD_WORKINGDIR` for iOS builds you should be able to inspect the contents of the `logs` subdirectory of the working directory to read your Xcode logs.

## Limitations

Some of the options available for cloud builds are not available locally. Limitations you should be aware of:

-   You can only build for a specific platform (option `all` is disabled).
-   Customizing versions of software is not supported, fields `node`, `yarn`, `fastlane`, `cocoapods`, `ndk`, `image` in **eas.json** are ignored.
-   Caching is not supported.
-   EAS environment variables with ["Secret" visibility](/eas/environment-variables.md#visibility-settings-for-environment-variables) are not supported (set them in your local environment instead).
-   You are responsible for making sure that the environment has all the necessary tools installed:
    -   Node.js/Yarn/npm
    -   fastlane (iOS only)
    -   CocoaPods (iOS only)
    -   Android SDK and NDK
-   On Windows, you can use [WSL](https://learn.microsoft.com/en-us/windows/wsl/install) for local EAS Builds. However, we do not officially test against this platform and do not support Windows for local builds (macOS and Linux are supported).

## App compilation for development and production builds locally

To compile your app locally for development with Expo CLI, use `npx expo run:android` or `npx expo run:ios` commands instead. If you use [Continuous Native Generation](/workflow/continuous-native-generation.md), you can also run [prebuild](/more/glossary-of-terms.md#prebuild) to generate your **android** and **ios** directories and then proceed to open the projects in the respective IDEs and build them like any native project. For more details, see:

[Local app development](/guides/local-app-development.md) — Learn how to compile and build your Expo app locally.

To create a production build locally, you need Android Studio and Xcode installed on your computer. See the following guide for more information:

[Create a production build locally](/guides/local-app-production.md) — Learn how to create a production build for your Expo app locally on your computer.

With any of the above approaches, you'll be following procedures which are different from creating a build on the cloud with EAS Build — that is what the `eas build --local` flag is for.
