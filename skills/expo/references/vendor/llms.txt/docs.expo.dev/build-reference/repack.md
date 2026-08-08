---
modificationDate: April 28, 2026
title: Repack app
description: Repackage an existing APK, IPA, or .app with an updated JavaScript bundle without a full native rebuild.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build-reference/repack/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build-reference/repack/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Repack app](https://docs.expo.dev/build-reference/repack.md) (this page)
- [Limitations](https://docs.expo.dev/build-reference/limitations.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Repack app

Repackage an existing APK, IPA, or .app with an updated JavaScript bundle without a full native rebuild.

[`@expo/repack-app`](https://www.npmjs.com/package/@expo/repack-app) is a CLI tool that repackages an existing Android APK, iOS IPA, or iOS **.app** bundle without performing a full native build. It updates the existing artifact with a new JavaScript (JS) bundle, assets, and app metadata (such as, app name, version, and package name or bundle identifier). Since there is no native compilation involved, a repack is typically much faster than a full native build.

To learn more about how fingerprint and repack work together to accelerate CI, see [Accelerating Continuous Integration with Fingerprint and Repack in EAS Workflows](https://expo.dev/blog/accelerating-continuous-integration-with-fingerprint-repack-in-eas-workflows) blog post.

#### Prerequisites

##### An existing build artifact

A build artifact produced from your Expo project: an APK for Android, or an IPA or **.app** bundle for iOS.

##### Signing credentials (optional)

Required if the repacked artifact needs to be installable on a device. For more information, see [Signing](/build-reference/repack.md#signing).

## When to use repack

Repack assumes the source binary's native side is unchanged since it was built. If you added a native dependency, changed a [config plugin](/config-plugins/introduction.md), or upgraded the Expo SDK, a repacked artifact will have JS that expects native APIs that do not exist, and will likely crash at runtime. Use [fingerprint](/versions/latest/sdk/fingerprint.md) to compare the source binary's native identity against your current project. If the fingerprints match, then repack is safe. If they differ, do a full native build instead.

Repack is not a replacement for [EAS Update](/eas-update/introduction.md). EAS Update ships a new JS bundle to apps that are already installed, and users see it on the next launch. Repack produces a new installable artifact. A good rule of thumb is to use repack for internal testing (QA devices, testers, CI smoke tests) and use EAS Update to deliver JS changes to production users.

## Use cases

-   **QA cycles**: Distribute one base build to testers, then repack with JS fixes for each iteration without waiting for a native rebuild.
-   **CI optimization**: Build native once per fingerprint, then repack the JS bundle on every subsequent PR to cut wait times from minutes to seconds.
-   **Branch testing**: Test multiple JS branches against the same native binary to isolate JavaScript-only changes.

## Usage

### Standalone CLI

You need to run the commands specified for Android and iOS platforms below from your project's root directory. At minimum, `--platform` and `--source-app` are required. The tool runs `npx expo export:embed` under the hood to produce a fresh JS bundle, swaps it into the source binary, and writes the output artifact.

The output format always matches the `--source-app` input. An APK input produces an APK, an IPA produces an IPA, and a **.app** bundle produces a **.app** bundle.

#### Android

```sh
npx @expo/repack-app --platform android --source-app MyApp.apk
```

#### iOS

Using an IPA:

```sh
npx @expo/repack-app --platform ios --source-app MyApp.ipa
```

Using a **.app** bundle (for simulator builds, the iOS device artifacts are always IPAs):

```sh
npx @expo/repack-app --platform ios --source-app MyApp.app
```

### EAS Workflows

EAS Workflows provides a pre-packaged `repack` job type that handles signing and build management automatically. See [EAS Workflows pre-packaged jobs](/eas/workflows/pre-packaged-jobs.md#repack) for full syntax, parameters, and examples.

## Signing

To produce an artifact installable on a physical device, pass signing credentials alongside `--platform` and `--source-app`. Without them, the Android APK is output unsigned and the iOS IPA cannot be installed on a device. iOS **.app** bundles intended for the simulator do not need signing and can skip this step.

If you do not already have signing credentials locally, see [App credentials](/app-signing/app-credentials.md) for how to obtain a keystore (Android) or a signing identity and provisioning profile (iOS).

#### Android

```sh
npx @expo/repack-app --platform android --source-app MyApp.apk --ks keystore.jks --ks-key-alias my-alias
```

See the [Android-specific options](/build-reference/repack.md#android-specific-options) for keystore password flags and other settings.

#### iOS

Pass a signing identity and provisioning profile:

> **Note**: iOS repacking supports ad-hoc and development signing only.

```sh
npx @expo/repack-app --platform ios --source-app MyApp.ipa --signing-identity "Apple Distribution: ..." --provisioning-profile /path/to/profile.mobileprovision
```

See the [iOS-specific options](/build-reference/repack.md#ios-specific-options) for the full list of signing flags.

## `--js-bundle-only` mode

By default, repack updates the JS bundle, assets, and app metadata (app name, version, bundle identifier, and the expo-updates manifest). Pass `--js-bundle-only` when you intentionally want to update only the JS bundle and leave all native config untouched.

```sh
npx @expo/repack-app --platform android --source-app MyApp.apk --js-bundle-only
```

## Limitations

Repack is not recommended for production Google Play Store or Apple App Store submissions. Production builds should go through the complete build pipeline for correct symbolication and signing.

## CLI reference

```text
Usage: @expo/repack-app [options] [project-root]
```

### Arguments

| Argument | Description |
| --- | --- |
| `[project-root]` | Path to the project root. Defaults to the current working directory. |

### Options

| Option | Description |
| --- | --- |
| `-p, --platform <platform>` | **Required.** Platform to repack the app for (`android` or `ios`). |
| `--source-app <path>` | **Required.** Path to the source app (APK for Android, or IPA or **.app** for iOS). The output format matches the input. |
| `-o, --output <path>` | Path to the output artifact. Defaults to `repacked.apk`, `repacked.ipa`, or `repacked.app` in the project root, matching the source format. |
| `-w, --working-directory <path>` | Path to the working directory for temporary files. |
| `--skip-working-dir-cleanup` | Skip cleaning up the working directory after the repack completes. Useful for debugging. |
| `-v, --verbose` | Enable verbose logging. |
| `--js-bundle-only` | Only update the JS bundle and skip native config updates (such as app name, version, and other metadata). |
| `--embed-bundle-assets` | Force running `npx expo export:embed` to generate the JS bundle and assets, even for debug builds where the bundle is normally loaded from a dev server. |
| `--bundle-assets-sourcemap-output <path>` | Generate a source map at the specified path. Requires `--embed-bundle-assets`. |

### Android-specific options

| Option | Description |
| --- | --- |
| `--ks <path>` | Path to the keystore file. |
| `--ks-pass <password>` | Keystore password. Defaults to `pass:android`. Supported formats: `pass:<password>`, `env:<name>`, `file:<file>`. |
| `--ks-key-alias <alias>` | Keystore key alias. |
| `--ks-key-pass <password>` | Keystore key password. Supported formats: `pass:<password>`, `env:<name>`, `file:<file>`. |
| `--android-build-tools-dir <path>` | Path to the Android SDK build-tools directory. |

### iOS-specific options

| Option | Description |
| --- | --- |
| `--signing-identity <identity>` | Code signing identity. |
| `--provisioning-profile <path>` | Path to a provisioning profile. Alternatively, a JSON-encoded value for multi-target apps (for example, when bundling app extensions). |
