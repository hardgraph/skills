---
modificationDate: July 29, 2026
title: Cache dependencies
description: Learn how to speed up your builds by caching dependencies.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build-reference/caching/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build-reference/caching/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Cache dependencies](https://docs.expo.dev/build-reference/caching.md) (this page)
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

# Cache dependencies

Learn how to speed up your builds by caching dependencies.

Before a build job can begin compiling your project, all project dependencies need to be available on disk. The longer it takes to acquire the dependencies, the more you need to wait for your build to complete — so caching dependencies is an important part of speeding up your builds.

> We're actively working on improving caching and other aspects of the build process to make builds reliably fast.

## Custom caching

The `cache` field on build profiles in [eas.json](/build/eas-json.md) can be used to configure caching for specific files and directories. Specified files will be saved to persistent storage after a successful build and restored on subsequent builds after the JavaScript dependencies are installed. Restoring does not overwrite existing files. Changing the `cache.key` value will invalidate the cache. Changing any other property of the `cache` object will also invalidate the cache.

## JavaScript dependencies

EAS Build runs an npm cache server that can speed up downloading JavaScript dependencies for your build jobs. By default, projects using npm or Yarn 2+ will use the cache. However, Yarn 1 (Classic) requires that you apply this [workaround](/build-reference/npm-cache-with-yarn.md) to use the cache in your project's **package.json**.

To disable the npm cache server, set the `EAS_BUILD_DISABLE_NPM_CACHE` environment variable to `"1"` in **eas.json**. If you use EAS Workflows, set it in a job's [`env`](/eas/workflows/syntax.md#jobsjob_idenv) instead.

```json
{
  "build": {
    "production": {
      "env": {
        "EAS_BUILD_DISABLE_NPM_CACHE": "1"
        ... 
      }
      ... 
    }
    ... 
  }
  ... 
}
```

### Immutable lockfiles

By default, Node packages will be installed with your preferred package manager's immutable lockfile flag/command (for example, `yarn --frozen-lockfile` or `npm ci`). If you would like to disable this, you can set the `EAS_NO_FROZEN_LOCKFILE` environment variable to `"1"` in **eas.json**.

## Android dependencies

EAS Build runs a Maven cache server that can speed up downloading Android dependencies for your build jobs.

Currently, we are caching:

-   `maven-central` - [https://repo1.maven.org/maven2/](https://repo1.maven.org/maven2/)
-   `google` - [https://maven.google.com/](https://maven.google.com/)
-   `plugins` - [https://plugins.gradle.org/m2/](https://plugins.gradle.org/m2/)

To disable the Maven cache server, set the `EAS_BUILD_DISABLE_MAVEN_CACHE` environment variable to `"1"` in **eas.json**. If you use EAS Workflows, set it in a job's [`env`](/eas/workflows/syntax.md#jobsjob_idenv) instead.

```json
{
  "build": {
    "production": {
      "env": {
        "EAS_BUILD_DISABLE_MAVEN_CACHE": "1"
        ... 
      }
      ... 
    }
    ... 
  }
  ... 
}
```

## Caching C/C++ compilation artifacts with ccache

[ccache](https://ccache.dev/) is a compiler cache that speeds up recompilation of native code by caching previous compilation results. EAS supports ccache configuration out of the box.

You can configure builds to save and restore ccache cache automatically with these environment variables:

-   `EAS_USE_CACHE`: When set to `1`, enables both restoring and saving the cache results during a build job.
-   `EAS_RESTORE_CACHE`: Controls restoring the cache at the beginning of a build. Set to `1` to enable or `0` to disable. Overrides `EAS_USE_CACHE`.
-   `EAS_SAVE_CACHE`: Controls whether to save the build cache at the end of a build. Set to `1` to enable or `0` to disable. Overrides `EAS_USE_CACHE`.

### EAS Workflows

For EAS Workflows, use [`eas/restore_cache`](/eas/workflows/syntax.md#easrestore_cache) and [`eas/save_cache`](/eas/workflows/syntax.md#eassave_cache).

**Android example:**

```yaml
jobs:
  build_android:
    type: build
    steps:
      - uses: eas/checkout
      - uses: eas/restore_build_cache
      # This is equivalent to the step above. You can also use /restore_cache for other caching purposes by defining your own key pattern and path
      # - uses: eas/restore_cache
      #   with:
      #     key: android-ccache-${{ hashFiles('yarn.lock') }}
      #     restore_keys: android
      #     path: /home/expo/.cache/ccache
      - uses: eas/build
      - uses: eas/save_build_cache
      # - uses: eas/save_cache
      #   with:
      #    key: android-ccache-${{ hashFiles('yarn.lock') }}
      #    path: /home/expo/.cache/ccache
```

**iOS example:**

```yaml
jobs:
  build_ios:
    type: build
    steps:
      - uses: eas/checkout
      - uses: eas/restore_build_cache
      # This is equivalent to the step above. You can also use /restore_cache for other caching purposes by defining your own key pattern and path
      # - uses: eas/restore_cache
      #   with:
      #     key: ios-ccache-${{ hashFiles('yarn.lock') }}
      #     restore_keys: ios
      #     path: /Users/expo/Library/Caches/ccache
      - uses: eas/build
      - uses: eas/save_build_cache
      # - uses: eas/save_cache
      #   with:
      #    key: ios-ccache-${{ hashFiles('yarn.lock') }}
      #    path: /Users/expo/Library/Caches/ccache
```

### Custom builds

In custom builds where you manage the build steps, add [`eas/restore_build_cache`](/custom-builds/schema.md#easrestore_build_cache) and [`eas/save_build_cache`](/custom-builds/schema.md#eassave_build_cache) to enable ccache cache.

```yaml
build:
  name: Build with ccache
  steps:
    - eas/checkout
    - eas/restore_build_cache
    - eas/build
    - eas/save_build_cache
```

The cache key uses a hash of the package manager lock file to create a unique key based on your dependencies. When dependencies change, a new cache will be created while still allowing fallback to previous caches using `restore_keys`.

### Cache key matching

When restoring a cache, the cache system follows a specific search sequence to find matching cache entries. The cache key is either automatically generated (when using `eas/restore_build_cache` or `eas/save_build_cache`) or explicitly provided via the `key` parameter (when using `eas/restore_cache` or `eas/save_cache`).

The search sequence:

1.  **Exact match**: First, it searches for an exact match to the cache key (automatically generated or explicitly provided)
2.  **Restore keys**: If no exact match is found, `restore_keys` will be checked sequentially for the most recent prefix matches

If there is an exact match to the provided `key`, this is considered a direct cache hit and the cache is restored immediately. If there is a partial match or a match from `restore_keys`, the cache is restored but may not perform as effectively

#### Using restore keys

> **Note:** Restore key matching is handled automatically by the cache system and does not require manual configuration. This is a reference to detail expected behavior.

The restore key `android-ccache-` matches any key that starts with the string `android-ccache-`. For example, both of the keys `android-ccache-fd3052de` and `android-ccache-a9b253ff` match the restore key. The cache with the most recent creation date would be used. The keys in this example are searched in the following order:

1.  **`android-ccache-${{ hashFiles('yarn.lock') }}`** matches a specific hash.
2.  **`android-ccache-`** matches cache keys prefixed with `android-ccache-`.
3.  **`android-`** matches any keys prefixed with `android-`.

### Cache restrictions

Access restrictions provide cache isolation and security by creating logical boundaries between different Git branches or users. It is important for security of yours and your users to understand and leverage them when building your app.

#### GitHub run

When a build is run from GitHub, caches get scoped to the branch the build is running from. A build can restore caches created in:

-   The current branch
-   The default branch (`main` or `master`)

#### EAS CLI run

When a build is triggered from `eas-cli`, caches are scoped to the user running the build. These user-scoped caches allow for isolation so that modifications to the build and its cache are not unintentionally shared during development or between users.

#### Default branch cache

If a build doesn't restore a user-scoped cache, it will automatically fallback to restoring caches published from GitHub builds triggered on the default branch. This allows builds to benefit from caches created by trusted sources even when no user-scoped cache exists yet.

#### Shared user behavior

When a single user-actor is shared between multiple people (such as when using access tokens or triggering builds from GitHub Actions), user-scoped cache rules still apply. This means that builds operating under that shared account will no longer have isolated caches and run the risk of sharing unintended artifacts. To avoid this, we recommend not restoring cache for production builds under a shared user, and to have designated jobs to only save clean, new caches.

### Designated jobs for cache creation

You can configure a job to publish clean, new caches by disabling cache restoration and only saving the cache.

**Disable restoring cache for production builds:**

You can disable cache restoration for specific build profiles by configuring these environment variables:

```json
{
  "build": {
    "production": {
      "env": {
        "EAS_RESTORE_CACHE": "0",
        "EAS_SAVE_CACHE": "1"
      }
    },
    "preview": {
      "env": {
        "EAS_USE_CACHE": "1"
      }
    }
  }
}
```

**Only save cache from designated jobs:**

To ensure only trusted sources publish cache, you can configure a workflow to only save cache from specific job on the main branch.

```yaml
jobs:
  build_production:
    type: build
    if: ${{ github.ref_name == 'main' }}
    env:
      EAS_RESTORE_CACHE: '0'
      EAS_SAVE_CACHE: '1'
    params:
      platform: android
      profile: production
```

> **Note:** Setting `EAS_SAVE_CACHE: '1'` does not make cache saving exclusive to this job, other jobs with the same environment variable can still save and overwrite the cache.

## iOS dependencies

EAS Build serves most CocoaPods artifacts from a cache server. This improves the consistency of `pod install` times and generally improves speed. The cache will be bypassed automatically if you provide your own **.netrc** or **.curlrc** files.

To disable the CocoaPods cache server, set the `EAS_BUILD_DISABLE_COCOAPODS_CACHE` environment variable to `"1"` in **eas.json**. If you use EAS Workflows, set it in a job's [`env`](/eas/workflows/syntax.md#jobsjob_idenv) instead.

```json
{
  "build": {
    "production": {
      "env": {
        "EAS_BUILD_DISABLE_COCOAPODS_CACHE": "1"
        ... 
      }
      ... 
    }
    ... 
  }
  ... 
}
```

It is typical to not have your project **Podfile.lock** committed to source control when using [prebuild](/more/glossary-of-terms.md#prebuild) to generate your **ios** directory [remotely at build time](/build-reference/ios-builds.md). It can be useful to cache your **Podfile.lock** to have deterministic builds, but the tradeoff in this case is that, because you don't use the lockfile during local development, your ability to determine when a change is needed and to update specific dependencies is limited. If you cache this file, you may occasionally end up with build errors that require clearing the cache. To cache **Podfile.lock**, add **./ios/Podfile.lock** to the `cache.paths` list in your build profile in **eas.json**.

```json
{
  "build": {
    "production": {
      "cache": {
        "paths": ["./ios/Podfile.lock"]
        ... 
      }
      ... 
    }
    ... 
  }
  ... 
}
```
