---
modificationDate: June 26, 2026
title: Android build process
description: Learn how an Android project is built on EAS Build.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build-reference/android-builds/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build-reference/android-builds/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Android build process](https://docs.expo.dev/build-reference/android-builds.md) (this page)
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

# Android build process

Learn how an Android project is built on EAS Build.

This page describes the process of building Android projects with EAS Build. You may want to read this if you are interested in the implementation details of the build service.

## Build process

Let's take a closer look at the steps for building Android projects with EAS Build. We'll first run some steps on your local machine to prepare the project, and then we'll build the project on a remote service.

### Local steps

The first phase happens on your computer. EAS CLI is in charge of completing the following steps:

1.  If `cli.requireCommit` is set to `true` in **eas.json**, check if the git index is clean - this means that there aren't any uncommitted changes. If it's not clean, EAS CLI will provide an option to commit local changes for you or abort the build process.
    
2.  Prepare the credentials needed for the build unless `builds.android.PROFILE_NAME.withoutCredentials` is set to `true`.
    
    -   Depending on the value of `builds.android.PROFILE_NAME.credentialsSource`, the credentials are obtained from either the local **credentials.json** file or from the EAS servers. If the `remote` mode is selected but no credentials exist yet, you're prompted to generate a new keystore.
3.  Create a tarball containing a copy of the repository. Actual behavior depends on the [VCS workflow](https://expo.fyi/eas-vcs-workflow) you are using.
    
4.  Upload the project tarball to a private Google Cloud Storage (GCS) bucket and send the build request to EAS Build.
    

### Remote steps

Next, this is what happens when EAS Build picks up your request:

1.  Create a new Docker container for the build.
    
    -   Every build gets its own fresh container with all build tools installed there (Java JDK, Android SDK, NDK, and so on).
2.  Download the project tarball from a private GCS bucket and unpack it.
    
3.  [Create **.npmrc**](/build-reference/private-npm-packages.md) if `NPM_TOKEN` is set.
    
4.  Run the `eas-build-pre-install` script from **package.json** if defined.
    
5.  Run `npm install` in the project root (or `yarn install` if `yarn.lock` exists).
    
6.  Run `npx expo-doctor` to diagnose potential issues with your project configuration.
    
7.  Additional step for projects that use [Continuous Native Generation (CNG)](/workflow/continuous-native-generation.md): Run `npx expo prebuild` to generate **android** and **ios** directories. This step will use the versioned Expo CLI.
    
8.  Restore a previously saved cache identified by the `cache.key` value in the [build profile](/build/eas-json.md).
    
9.  Run the `eas-build-post-install` script from **package.json** if defined.
    
10.  Restore the keystore (if it was included in the build request).
     
11.  Inject the signing [configuration into **build.gradle**](/build-reference/android-builds.md#configuring-gradle).
     
12.  Run `./gradlew COMMAND` in the **android** directory inside your project.
     
     -   `COMMAND` is the command defined in your **eas.json** at `builds.android.PROFILE_NAME.gradleCommand`. It defaults to `:app:bundleRelease` which produces the AAB (Android App Bundle).
13.  **Deprecated:** Run the `eas-build-pre-upload-artifacts` script from **package.json** if defined.
     
14.  Store a cache of files and directories defined in the [build profile](/build/eas-json.md). Subsequent builds will restore this cache.
     
15.  Upload the application archive to GCS.
     
     -   The artifact path can be configured in **eas.json** at `builds.android.PROFILE_NAME.applicationArchivePath`. It defaults to `android/app/build/outputs/**/*.{apk,aab}`. We're using [glob patterns](https://github.com/isaacs/node-glob#glob-primer) for pattern matching.
16.  If the build was successful: run the `eas-build-on-success` script from **package.json** if defined.
     
17.  If the build failed: run the `eas-build-on-error` script from **package.json** if defined.
     
18.  Run the `eas-build-on-complete` script from **package.json** if defined. The `EAS_BUILD_STATUS` env variable is set to either `finished` or `errored`.
     
19.  Upload the build artifacts archive to a private GCS bucket if `buildArtifactPaths` is specified in the build profile.
     

## Project auto-configuration

Every time you want to build a new Android app binary, we validate that the project is set up correctly so we can seamlessly run the build process on our servers. This mainly applies to [existing React Native projects](/bare/overview.md), but similar steps are run when building projects that use Continuous Native Generation (CNG).

### Android keystore

Android requires you to sign your application with a certificate. That certificate is stored in your keystore. The Google Play Store identifies applications based on the certificate. This means that if you lose your keystore, you may not be able to update your application in the store. However, with [Play App Signing](https://developer.android.com/studio/publish/app-signing#app-signing-google-play), you can mitigate the risk of losing your keystore.

Your application's keystore should be kept private. **Under no circumstances should you check it in to your repository.** Debug keystores are the only exception because we don't use them for uploading apps to the Google Play Store.

### Configuring Gradle

Your app binary needs to be signed with a keystore. Since we're building the project on a remote server, we had to come up with a way to provide Gradle with credentials which aren't, for security reasons, checked in to your repository. In one of the remote steps, we inject the signing configuration into your **build.gradle**. EAS Build creates the **android/app/eas-build.gradle** file with the following contents:

```groovy
// Build integration with EAS

import java.nio.file.Paths

android {
  signingConfigs {
    release {
      // This is necessary to avoid needing the user to define a release signing config manually
      // If no release config is defined, and this is not present, build for assembleRelease will crash
    }
  }

  buildTypes {
    release {
      // This is necessary to avoid needing the user to define a release build type manually
    }
    debug {
      // This is necessary to avoid needing the user to define a debug build type manually
    }
  }
}

tasks.whenTaskAdded {
  android.signingConfigs.release {
    def credentialsJson = rootProject.file("../credentials.json");
    def credentials = new groovy.json.JsonSlurper().parse(credentialsJson)
    def keystorePath = Paths.get(credentials.android.keystore.keystorePath);
    def storeFilePath = keystorePath.isAbsolute()
      ? keystorePath
      : rootProject.file("..").toPath().resolve(keystorePath);

    storeFile storeFilePath.toFile()
    storePassword credentials.android.keystore.keystorePassword
    keyAlias credentials.android.keystore.keyAlias
    if (credentials.android.keystore.containsKey("keyPassword")) {
      keyPassword credentials.android.keystore.keyPassword
    } else {
      // key password is required by Gradle, but PKCS keystores don't have one
      // using the keystore password seems to satisfy the requirement
      keyPassword credentials.android.keystore.keystorePassword
    }
  }

  android.buildTypes.release {
    signingConfig android.signingConfigs.release
  }

  android.buildTypes.debug {
    signingConfig android.signingConfigs.release
  }
}
```

The most important part is the `release` signing config. It's configured to read the keystore and passwords from the **credentials.json** file at the project root. Even though you're not required to create this file on your own, it's created and populated with your credentials by EAS Build before running the build.

This file is imported in **android/app/build.gradle** like this:

```groovy
// ...

apply from: "./eas-build.gradle"
```
