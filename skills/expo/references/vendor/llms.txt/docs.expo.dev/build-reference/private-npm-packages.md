---
modificationDate: February 28, 2026
title: Using private npm packages
sidebar_label: Private npm packages
description: Learn how to configure EAS Build to use private npm packages.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build-reference/private-npm-packages/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build-reference/private-npm-packages/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Build > Reference
Pages in this section:
- [Build lifecycle hooks](https://docs.expo.dev/build-reference/npm-hooks.md)
- [Using private npm packages](https://docs.expo.dev/build-reference/private-npm-packages.md) (this page)
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
- [Limitations](https://docs.expo.dev/build-reference/limitations.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Using private npm packages

Learn how to configure EAS Build to use private npm packages.

EAS Build has full support for using private npm packages in your project. These can either be published to npm (if you have [the Pro/Teams plan](https://www.npmjs.com/products)) or to a private registry (for example, using self-hosted [Verdaccio](https://verdaccio.org/)).

Before starting the build, you will need to configure your project to provide EAS Build with your npm token.

## Default npm configuration

By default, EAS Build uses a self-hosted npm cache that speeds up installing dependencies for all builds. Every EAS Build builder is configured with a **.npmrc** file for each platform:

### Android

```ini
registry=http://npm-cache-service.worker-infra-production.svc.cluster.local:4873
```

### iOS

```ini
registry=http://10.254.24.8:4873
```

## Private packages published to npm

If your project is using private packages published to npm, you need to provide EAS Build with [a read-only npm token](https://docs.npmjs.com/about-access-tokens) so that it can install your dependencies successfully.

The recommended way is to add the `NPM_TOKEN` secret to your account or project's secrets:

For more information on how to do that, see [secret environment variables](/eas/environment-variables/manage.md#create-variables-in-the-dashboard).

When EAS detects that the `NPM_TOKEN` environment variable is available during a build, it automatically creates the following **.npmrc**:

```ini
//registry.npmjs.org/:_authToken=${NPM_TOKEN}
registry=https://registry.npmjs.org/
```

However, this only happens when **.npmrc** is not in your project's root directory. If you already have this file, you need to update it manually.

You can verify if it worked by viewing build logs and looking for the **Prepare project** build phase:

## Packages published to a private registry

If you're using a private npm registry such as self-hosted [Verdaccio](https://verdaccio.org/), you will need to configure the **.npmrc** manually.

Create a **.npmrc** file in your project's root directory with the following contents:

```ini
registry=__REPLACE_WITH_REGISTRY_URL__
```

If your registry requires authentication, you will need to provide the token. For example, if your registry URL is `https://registry.johndoe.com/`, then update the file with:

```ini
//registry.johndoe.com/:_authToken=${NPM_TOKEN}
registry=https://registry.johndoe.com/
```

## Both private npm packages and private registry

> This is an advanced example.

Private npm packages are always [scoped](https://docs.npmjs.com/about-scopes#scopes-and-package-visibility). For example, if your npm username is `johndoe`, the private self-hosted registry URL is `https://registry.johndoe.com/`. If you want to install dependencies from both sources, create a **.npmrc** in your project's root directory with the following:

```ini
//registry.npmjs.org/:_authToken=${NPM_TOKEN}
@johndoe:registry=https://registry.npmjs.org/
registry=https://registry.johndoe.com/
```

## Submodules in private repositories

If you have a submodule in a private repository, you will need to initialize it by setting up an SSH key. For more information, see [submodules initialization](/build-reference/git-submodules.md#submodules-initialization).
