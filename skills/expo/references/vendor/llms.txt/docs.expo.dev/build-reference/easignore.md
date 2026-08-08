---
modificationDate: March 01, 2026
title: Ignore files via .easignore
description: Learn how to configure EAS to ignore unnecessary files during the build process.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build-reference/easignore/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build-reference/easignore/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Ignore files via .easignore](https://docs.expo.dev/build-reference/easignore.md) (this page)
- [npx testflight](https://docs.expo.dev/build-reference/npx-testflight.md)
- [Repack app](https://docs.expo.dev/build-reference/repack.md)
- [Limitations](https://docs.expo.dev/build-reference/limitations.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Ignore files via .easignore

Learn how to configure EAS to ignore unnecessary files during the build process.

A **.easignore** file defines which files [EAS](https://expo.dev/eas) should ignore when uploading your project to the [EAS Build](/build/introduction.md) servers.

> Ignoring unnecessary files can help reduce your app's archive size and upload time.

By default, the [EAS CLI](/build/setup.md#install-the-latest-eas-cli) refers to the [**.gitignore**](https://git-scm.com/docs/gitignore) file (if it exists) to determine which files to ignore. If you create a **.easignore** file, the EAS CLI prioritizes it over the **.gitignore** file. When creating a **.easignore** file, include all files and directories from your **.gitignore** file and add additional files you want to ignore.

Create a **.easignore** file in the root of your project.

Copy the content of the **.gitignore** file into the **.easignore** file. Then, add any files that are unnecessary for the build process.

```bash
# Copy everything from your .gitignore file here

# Ignore files and directories that EAS Build doesn't need to build your app
/docs

# Ignore native directories (if you are using EAS Build)
/android
/ios

# Ignore test coverage reports
/coverage
```

If your project does not contain **android** and **ios** directories, [EAS Build will run Prebuild](/workflow/continuous-native-generation.md#usage-with-eas-build) to generate these native directories before compilation.

Save the file and trigger a new build.

```sh
eas build --platform ios --profile development
```

You've successfully configured your **.easignore** file.

## Adding files to your project upload with .easignore

In addition to ignoring additional files beyond what is in your gitignore file, you can also use the **.easignore** file to include files with your EAS Build upload that are not committed to source control. This is useful if you have custom scripts that generate temporary files needed for your build process just before the build. To upload a file not in source control to EAS Build, add it to the **.easignore** file with a `!` prefix, along with the rest of your **.gitignore** contents. The `!` prefixed file should be last, so it takes precedence over any prior rules that would ignore it.

```bash
# Copy everything from your .gitignore file here

/android
/ios

# Include a file not in source control
!temp_file.json
```
