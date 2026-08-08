---
modificationDate: July 8th, 2026
title: Build server infrastructure
description: Learn about the current build server infrastructure when using EAS.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build-reference/infrastructure/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build-reference/infrastructure/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Server infrastructure](https://docs.expo.dev/build-reference/infrastructure.md) (this page)
- [iOS App Extensions](https://docs.expo.dev/build-reference/app-extensions.md)
- [Ignore files via .easignore](https://docs.expo.dev/build-reference/easignore.md)
- [npx testflight](https://docs.expo.dev/build-reference/npx-testflight.md)
- [Repack app](https://docs.expo.dev/build-reference/repack.md)
- [Limitations](https://docs.expo.dev/build-reference/limitations.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Build server infrastructure

Learn about the current build server infrastructure when using EAS.

## Builder IP addresses

A list of the IP addresses of the build servers is available [in this file](https://expo.dev/eas-build-worker-ips.txt). We do not expect to change the list often. The list includes "Last-Modified" and "Expires" ISO 8601 timestamps that respectively specify the last time the list was updated and the time until which we commit to not change the list.

Linux runners are hosted in Google Cloud Platform. macOS runners are hosted in our own macOS cloud.

## Configuring build environment

Images for each platform have one specific version of Node.js, Yarn, CocoaPods, Xcode, Ruby, Fastlane, and so on. You can override some of the versions in [eas.json](/build/eas-json.md). If there is no dedicated configuration option you are looking for, you can use [npm hooks](/build-reference/npm-hooks.md) to install or update any system dependencies with `apt-get` or `brew`. Consider that those customizations are applied during the build and will increase your build times.

When selecting an image for the build you can use the full name provided below or one of the aliases: `auto`, `latest`, or for a particular SDK such as `sdk-57`.

-   The use of a specific name guarantees a consistent environment with only minor updates.
-   When using the `auto` alias, the build image will be selected based on the project configuration, Expo SDK version, and React Native version. You can check what image is used for a build in the **Spin up build environment** build logs section.
-   The `latest` alias will be assigned to the image with the most up-to-date versions of the software.
-   The `sdk-57` alias will be assigned to the image best suited for SDK 57 builds.
-   The `sdk-56` alias will be assigned to the image best suited for SDK 56 builds.
-   The `sdk-55` alias will be assigned to the image best suited for SDK 55 builds.
-   The `sdk-54` alias will be assigned to the image best suited for SDK 54 builds.
-   The `sdk-53` alias will be assigned to the image best suited for SDK 53 builds.
-   The `sdk-52` alias will be assigned to the image best suited for SDK 52 builds.
-   SDK aliases will be updated with every new SDK release.
-   The `latest` alias will be updated with every new image release.

> **Note:** If you do not provide `image` in **eas.json**, your build by default will use the `auto` alias.

## Android build server configurations

Android builders run on virtual machines in an isolated environment. Every build gets its own dedicated VM instance.

-   Build resources:
    
    -   [medium](/eas/json.md#resourceclass-1): 4 vCPUs, 16 GB RAM ([n2-standard-4](https://cloud.google.com/compute/docs/general-purpose-machines#n2_machine_types) or [c3d-standard-4](https://cloud.google.com/compute/docs/general-purpose-machines#c3d_machine_types) (default) Google Cloud machine type, depending on the "New Android Builds Infrastructure" setting in project settings)
    -   [large](/eas/json.md#resourceclass-1): 8 vCPUs, 32 GB RAM ([n2-standard-8](https://cloud.google.com/compute/docs/general-purpose-machines#n2_machine_types) or [c3d-standard-8](https://cloud.google.com/compute/docs/general-purpose-machines#c3d_machine_types) (default) Google Cloud machine type, depending on the "New Android Builds Infrastructure" setting in project settings)
-   [npm cache deployed with Kubernetes](/build-reference/caching.md#javascript-dependencies)
    
-   [Maven cache deployed with Kubernetes](/build-reference/caching.md#android-dependencies)
    
-   Gradle JVM args are injected via the `GRADLE_OPTS` environment variable when the build environment is provisioned. See [Gradle JVM args](/build-reference/infrastructure.md#gradle-jvm-args) below.
    
-   Global npm configuration in **~/.npmrc**:
    
    ```ini
    registry=http://npm.production.caches.eas-build.internal
    ```
    
-   Global Yarn configuration in **~/.yarnrc.yml**:
    
    ```yaml
    unsafeHttpWhitelist:
      - '*'
    npmRegistryServer: 'http://npm.production.caches.eas-build.internal'
    enableImmutableInstalls: false
    ```
    

### Gradle JVM args

EAS Build sets the `GRADLE_OPTS` environment variable on the build VM (the worker) before Gradle runs. The values depend on the [resource class](/eas/json.md#resourceclass) you select:

| Resource class | `-Xmx` (max heap) |
| --- | --- |
| `medium` | `4g` |
| `large` | `8g` |

In addition to `-Xmx`, the worker passes the following JVM args to the Gradle build JVM via `-Dorg.gradle.jvmargs`:

-   `-XX:MaxMetaspaceSize=1g`
-   `-XX:+HeapDumpOnOutOfMemoryError`
-   `-Dfile.encoding=UTF-8`

The worker also sets these top-level Gradle properties on `GRADLE_OPTS`:

-   `-Dorg.gradle.parallel=true`
-   `-Dorg.gradle.daemon=false`

> The worker sets `org.gradle.jvmargs` via `GRADLE_OPTS`, which overrides any `org.gradle.jvmargs` defined in your project's **gradle.properties**.

#### Overriding `GRADLE_OPTS`

You can replace the worker default by setting `GRADLE_OPTS` under a build profile's [`env`](/eas/json.md#env) in **eas.json**, in a [workflow file](/eas/workflows/syntax.md#jobsjob_idenv), or with [EAS Environment Variables](/eas/environment-variables.md). Project environment values take precedence over the worker's default values.

### Android server images

#### `ubuntu-26.04-jdk-17-ndk-r27b-sdk-57` (`latest`, `sdk-57`)

#### Details

-   GCE image: `ubuntu-2604-resolute-amd64-v20260624`
-   NDK 27.1.12297006
-   Node.js 22.23.1
-   Bun 1.3.14
-   Yarn 1.22.22
-   pnpm 11.9.0
-   npm 10.9.8
-   Java 17
-   node-gyp 13.0.0
-   Maestro 2.6.1

#### `ubuntu-26.04-jdk-17-ndk-r27b` (`sdk-56`)

#### Details

-   GCE image: `ubuntu-2604-resolute-amd64-v20260505`
-   NDK 27.1.12297006
-   Node.js 22.22.2
-   Bun 1.3.13
-   Yarn 1.22.22
-   pnpm 10.33.3
-   npm 10.9.4
-   Java 17
-   node-gyp 12.3.0
-   Maestro 2.5.1

#### `ubuntu-24.04-jdk-17-ndk-r27b-sdk-55` (`sdk-55`)

#### Details

-   GCE image: `ubuntu-2404-noble-amd64-v20260128`
-   NDK 27.1.12297006
-   Node.js 20.19.4
-   Bun 1.3.8
-   Yarn 1.22.22
-   pnpm 10.28.2
-   npm 10.9.3
-   Java 17
-   node-gyp 12.2.0
-   Maestro 2.1.0

#### `ubuntu-24.04-jdk-17-ndk-r27b` (`sdk-54`)

#### Details

-   GCE image: `ubuntu-2404-noble-amd64-v20250805`
-   NDK 27.1.12297006
-   Node.js 20.19.4
-   Bun 1.2.20
-   Yarn 1.22.22
-   pnpm 10.14.0
-   npm 10.9.3
-   Java 17
-   node-gyp 11.3.0
-   Maestro 2.0.2

#### `ubuntu-22.04-jdk-17-ndk-r26b` (`sdk-53`)

#### Details

-   Docker image: `ubuntu:jammy-v20250112`
-   NDK 26.1.10909125
-   Node.js 20.19.2
-   Bun 1.2.4
-   Yarn 1.22.22
-   pnpm 9.15.5
-   npm 10.8.2
-   Java 17
-   node-gyp 11.1.0

#### Legacy `ubuntu-22.04-jdk-17-ndk-r26b`-like (`sdk-51`, `sdk-52`)

#### Details

-   Docker image: `ubuntu:jammy-v20250112`
-   NDK 26.1.10909125
-   Node.js 20.18.3
-   Bun 1.2.4
-   Yarn 1.22.22
-   pnpm 9.15.5
-   npm 10.8.2
-   Java 17
-   node-gyp 11.1.0

#### `ubuntu-22.04-jdk-17-ndk-r25b` (`sdk-50`)

#### Details

-   Docker image: `ubuntu:jammy-20220810`
-   NDK 25.1.8937393
-   Node.js 18.18.0
-   Bun 1.0.14
-   Yarn 1.22.19
-   pnpm 8.9.2
-   npm 9.8.1
-   Java 17
-   node-gyp 10.0.1

#### `ubuntu-22.04-jdk-11-ndk-r23b` (`sdk-49`)

#### Details

-   Docker image: `ubuntu:jammy-20220810`
-   NDK 23.1.7779620
-   Node.js 18.18.0
-   Bun 1.0.14
-   Yarn 1.22.19
-   pnpm 8.7.5
-   npm 9.8.1
-   Java 11
-   node-gyp 10.0.1

#### `ubuntu-22.04-jdk-17-ndk-r21e`

#### Details

-   Docker image: `ubuntu:jammy-20220810`
-   NDK 21.4.7075529
-   Node.js 18.18.0
-   Bun 1.0.14
-   Yarn 1.22.19
-   pnpm 8.9.2
-   npm 9.8.1
-   Java 17
-   node-gyp 10.0.1

#### `ubuntu-22.04-jdk-11-ndk-r21e`

#### Details

-   Docker image: `ubuntu:jammy-20220810`
-   NDK 21.4.7075529
-   Node.js 18.18.0
-   Bun 1.0.14
-   Yarn 1.22.19
-   pnpm 8.7.5
-   npm 9.8.1
-   Java 11
-   node-gyp 10.0.1

#### `ubuntu-22.04-jdk-8-ndk-r21e` (deprecated)

#### Details

-   Docker image: `ubuntu:jammy-20220810`
-   NDK 21.4.7075529
-   Node.js 18.18.0
-   Bun 1.0.14
-   Yarn 1.22.19
-   pnpm 7.0.0
-   npm 9.8.1
-   Java 8
-   node-gyp 10.0.1

#### `ubuntu-20.04-jdk-11-ndk-r23b` (deprecated)

#### Details

-   Docker image: `ubuntu:focal-20220823`
-   NDK 23.1.7779620
-   Node.js 18.18.0
-   Bun 1.0.14
-   Yarn 1.22.19
-   pnpm 7.0.0
-   npm 9.8.1
-   Java 11
-   node-gyp 10.0.1

#### `ubuntu-20.04-jdk-11-ndk-r21e` (deprecated)

#### Details

-   Docker image: `ubuntu:focal-20220823`
-   NDK 21.4.7075529
-   Node.js 18.18.0
-   Bun 1.0.14
-   Yarn 1.22.19
-   pnpm 7.0.0
-   npm 9.8.1
-   Java 11
-   node-gyp 10.0.1

#### `ubuntu-20.04-jdk-8-ndk-r21e` (deprecated)

#### Details

-   Docker image: `ubuntu:focal-20220823`
-   NDK 21.4.7075529
-   Node.js 18.18.0
-   Bun 1.0.14
-   Yarn 1.22.19
-   pnpm 7.0.0
-   npm 9.8.1
-   Java 8
-   node-gyp 10.0.1

#### `ubuntu-20.04-jdk-11-ndk-r19c` (deprecated)

#### Details

-   Docker image: `ubuntu:focal-20220823`
-   NDK 19.2.5345600
-   Node.js 18.18.0
-   Bun 1.0.14
-   Yarn 1.22.19
-   pnpm 7.0.0
-   npm 9.8.1
-   Java 11
-   node-gyp 10.0.1

#### `ubuntu-20.04-jdk-8-ndk-r19c` (deprecated)

#### Details

-   Docker image: `ubuntu:focal-20220823`
-   NDK 19.2.5345600
-   Node.js 18.18.0
-   Bun 1.0.14
-   Yarn 1.22.19
-   pnpm 7.0.0
-   npm 9.8.1
-   Java 8
-   node-gyp 10.0.1

## iOS build server configurations

iOS builder VMs run on Mac mini hosts in an isolated environment. Every build gets its own fresh macOS VM. For more information, see [iOS-specific resource classes](/eas/json.md#resourceclass-2).

-   Build resources:
    
    -   [medium](/eas/json.md#resourceclass-2): 5 performance cores, 20 GiB RAM, 110 GB SSD
    -   [large](/eas/json.md#resourceclass-2): 10 performance cores, 40 GiB RAM, 110 GB SSD
-   [npm cache](/build-reference/caching.md#javascript-dependencies)
    
-   [CocoaPods cache](/build-reference/caching.md#ios-dependencies)
    
-   Global npm configuration in **~/.npmrc**:
    
    ```ini
    registry=http://npm.caches.eas-build.internal
    ```
    
-   Global Yarn configuration in **~/.yarnrc.yml**:
    
    ```yaml
    unsafeHttpWhitelist:
      - '*'
    npmRegistryServer: 'http://npm.caches.eas-build.internal'
    enableImmutableInstalls: false
    ```
    

### iOS server images

#### `macos-tahoe-26.5-xcode-26.6` (`latest`, `sdk-57`)

#### Details

-   macOS Tahoe 26.5.2
-   Xcode 26.6 (17F113)
-   Node.js 22.23.1
-   Bun 1.3.14
-   Yarn 1.22.22
-   pnpm 11.9.0
-   npm 10.9.8
-   fastlane 2.236.1
-   CocoaPods 1.16.2
-   Ruby 3.2
-   node-gyp 13.0.0
-   Maestro 2.6.1

#### `macos-tahoe-26.4-xcode-26.4` (`sdk-56`)

#### Details

-   macOS Tahoe 26.4.1
-   Xcode 26.4 (17E202)
-   Node.js 22.22.2
-   Bun 1.3.13
-   Yarn 1.22.22
-   pnpm 10.33.3
-   npm 10.9.4
-   fastlane 2.233.1
-   CocoaPods 1.16.2
-   Ruby 3.2
-   node-gyp 12.3.0
-   Maestro 2.5.1

#### `macos-sequoia-15.6-xcode-26.2` (`sdk-55`)

#### Details

-   macOS Sequoia 15.6.1
-   Xcode 26.2 (17C52)
-   Node.js 20.19.4
-   Bun 1.3.8
-   Yarn 1.22.22
-   pnpm 10.28.2
-   npm 10.9.3
-   fastlane 2.231.1
-   CocoaPods 1.16.2
-   Ruby 3.2
-   node-gyp 12.2.0
-   Maestro 2.1.0

#### `macos-sequoia-15.6-xcode-26.1`

#### Details

-   macOS Sequoia 15.6.1
-   Xcode 26.1 (17B55)
-   Node.js 20.19.4
-   Bun 1.3.1
-   Yarn 1.22.22
-   pnpm 10.20.0
-   npm 10.9.3
-   fastlane 2.228.0
-   CocoaPods 1.16.2
-   Ruby 3.2
-   node-gyp 11.5.0
-   Maestro 2.0.9

#### `macos-sequoia-15.6-xcode-26.0` (`sdk-54`, `macos-sequoia-15.5-xcode-26.0`)

#### Details

-   macOS Sequoia 15.6
-   Xcode 26.0 (17A324)
-   Node.js 20.19.4
-   Bun 1.2.22
-   Yarn 1.22.22
-   pnpm 10.16.1
-   npm 10.9.3
-   fastlane 2.228.0
-   CocoaPods 1.16.2
-   Ruby 3.2
-   node-gyp 11.4.2
-   jq 1.8.0
-   Azul Zulu JDK 17.58.21 (OpenJDK 17.0.15)
-   Git 2.49.0
-   Git LFS 3.6.1
-   applesimutils 0.9.12
-   idb-companion 1.1.8
-   Maestro 2.0.3

#### `macos-sequoia-15.6-xcode-16.4` (recommended for SDK 54 if you don't want to use Xcode 26)

#### Details

-   macOS Sequoia 15.6
-   Xcode 16.4 (16F6)
-   Node.js 20.19.4
-   Bun 1.2.20
-   Yarn 1.22.22
-   pnpm 10.14.0
-   npm 10.9.3
-   fastlane 2.228.0
-   CocoaPods 1.16.2
-   Ruby 3.2
-   node-gyp 11.3.0
-   Maestro 1.41.0
-   jq 1.8.0
-   Azul Zulu JDK 17.58.21 (OpenJDK 17.0.15)
-   Git 2.49.0
-   Git LFS 3.6.1
-   applesimutils 0.9.10
-   idb-companion 1.1.8

#### `macos-sequoia-15.5-xcode-16.4` (`sdk-53`)

#### Details

-   macOS Sequoia 15.5
-   Xcode 16.4 (16E140)
-   Node.js 20.19.2
-   Bun 1.2.15
-   Yarn 1.22.22
-   pnpm 9.15.9
-   npm 10.8.2
-   fastlane 2.227.1
-   CocoaPods 1.16.2
-   Ruby 3.2
-   node-gyp 11.2.0
-   jq 1.8.0
-   Azul Zulu JDK 17.58.21 (OpenJDK 17.0.15)
-   Git 2.49.0
-   Git LFS 3.6.1
-   applesimutils 0.9.10
-   idb-companion 1.1.8

#### `macos-sequoia-15.4-xcode-16.3`

#### Details

-   macOS Sequoia 15.4.1
-   Xcode 16.3 (16E140)
-   Node.js 20.19.1
-   Bun 1.2.11
-   Yarn 1.22.22
-   pnpm 9.15.9
-   npm 9.8.1
-   fastlane 2.227.1
-   CocoaPods 1.16.2
-   Ruby 3.2
-   node-gyp 11.2.0
-   jq 1.7.1
-   Azul Zulu JDK 17.58.21 (OpenJDK 17.0.15)
-   Git 2.49.0
-   Git LFS 3.6.1
-   applesimutils 0.9.10
-   idb-companion 1.1.8

#### `macos-sequoia-15.3-xcode-16.2` (`sdk-52`)

#### Details

-   macOS Sequoia 15.3
-   Xcode 16.2 (16C5032a)
-   Node.js 20.18.3
-   Bun 1.2.4
-   Yarn 1.22.22
-   pnpm 9.15.5
-   npm 9.8.1
-   fastlane 2.226.0
-   CocoaPods 1.16.2
-   Ruby 3.2
-   node-gyp 11.1.0

#### `macos-sonoma-14.6-xcode-16.1`

#### Details

-   macOS Sonoma 14.6
-   Xcode 16.1 (16B40)
-   Node.js 18.18.0
-   Bun 1.1.33
-   Yarn 1.22.21
-   pnpm 9.12.3
-   npm 9.8.1
-   fastlane 2.225.0
-   CocoaPods 1.16.2
-   Ruby 3.2
-   node-gyp 10.2.0

#### `macos-sonoma-14.6-xcode-16.0`

#### Details

-   macOS Sonoma 14.6
-   Xcode 16.0 (16A242d)
-   Node.js 18.18.0
-   Bun 1.1.27
-   Yarn 1.22.21
-   pnpm 9.10.0
-   npm 9.8.1
-   fastlane 2.222.0
-   CocoaPods 1.15.2
-   Ruby 3.2
-   node-gyp 10.2.0

#### `macos-sonoma-14.5-xcode-15.4` (`sdk-51`, `sdk-50`, `sdk-49`)

#### Details

-   macOS Sonoma 14.5
-   Xcode 15.4 (15F31d)
-   Node.js 18.18.0
-   Bun 1.1.13
-   Yarn 1.22.21
-   pnpm 9.3.0
-   npm 9.8.1
-   fastlane 2.220.0
-   CocoaPods 1.14.3
-   Ruby 2.7
-   node-gyp 10.1.0

#### `macos-sonoma-14.4-xcode-15.3`

#### Details

-   macOS Sonoma 14.4.1
-   Xcode 15.3 (15E204a)
-   Node.js 18.18.0
-   Bun 1.0.35
-   Yarn 1.22.21
-   pnpm 8.14.1
-   npm 9.8.1
-   fastlane 2.219.0
-   CocoaPods 1.14.3
-   Ruby 2.7
-   node-gyp 10.0.1

#### `macos-ventura-13.6-xcode-15.2`

#### Details

-   macOS Ventura 13.6
-   Xcode 15.2 (15C500b)
-   Node.js 18.18.0
-   Bun 1.0.23
-   Yarn 1.22.21
-   pnpm 8.14.1
-   npm 9.8.1
-   fastlane 2.219.0
-   CocoaPods 1.14.3
-   Ruby 2.7
-   node-gyp 10.0.1

#### `macos-ventura-13.6-xcode-15.1`

#### Details

-   macOS Ventura 13.6
-   Xcode 15.1 (15C65)
-   Node.js 18.18.0
-   Bun 1.0.14
-   Yarn 1.22.19
-   pnpm 8.12.1
-   npm 9.8.1
-   fastlane 2.217.0
-   CocoaPods 1.14.3
-   Ruby 2.7
-   node-gyp 10.0.1

#### `macos-ventura-13.6-xcode-15.0`

#### Details

-   macOS Ventura 13.6
-   Xcode 15.0 (15A240d)
-   Node.js 18.18.0
-   Bun 1.0.14
-   Yarn 1.22.19
-   pnpm 8.7.6
-   npm 9.8.1
-   fastlane 2.216.0
-   CocoaPods 1.13.0
-   Ruby 2.7
-   node-gyp 10.0.1

### Supported Xcode versions

We aim to support all stable Xcode releases that allow you to submit your app to the App Store Connect when used during the build process.

This usually means that we support the latest stable Xcode version and the previous one (until the new [minimal Xcode version requirement](https://developer.apple.com/news/upcoming-requirements/?id=04292024a) is introduced by Apple).
