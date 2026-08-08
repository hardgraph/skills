---
modificationDate: June 29, 2026
title: Asset selection and exclusion
description: Learn how to use the asset selection feature and verify that an update includes all required app assets.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas-update/asset-selection/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas-update/asset-selection/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Update > Reference
Pages in this section:
- [Code signing](https://docs.expo.dev/eas-update/code-signing.md)
- [Asset selection and exclusion](https://docs.expo.dev/eas-update/asset-selection.md) (this page)
- [Using without other EAS services](https://docs.expo.dev/eas-update/standalone-service.md)
- [Request proxying](https://docs.expo.dev/eas-update/request-proxying.md)
- [Migrate from CodePush](https://docs.expo.dev/eas-update/codepush.md)
- [Migrate from Classic Updates](https://docs.expo.dev/eas-update/migrate-from-classic-updates.md)
- [Trace update ID back to the EAS dashboard](https://docs.expo.dev/eas-update/trace-update-id-expo-dashboard.md)
- [Estimate bandwidth usage](https://docs.expo.dev/eas-update/estimate-bandwidth.md)
- [Integrate in existing native apps](https://docs.expo.dev/eas-update/integration-in-existing-native-apps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Asset selection and exclusion

Learn how to use the asset selection feature and verify that an update includes all required app assets.

Experimental **asset selection feature** allows the developer to specify that only certain assets should be included in updates. This can greatly reduce the number of assets that need to be uploaded to and downloaded from the updates server. This feature will work with the EAS Update server or any custom server that complies with the [`expo-updates` protocol](/technical-specs/expo-updates-1.md).

SDK 52 launched this feature to general availability.

## Using asset selection

To use asset selection in SDK versions below 52, include the property `extra.updates.assetPatternsToBeBundled` in your app config. It should define one or more file-matching patterns (regular expressions). For example, an **app.json** file has the patterns defined in the following way:

```json
"expo": {
    ... 
    "extra": {
      "updates": {
        "assetPatternsToBeBundled": [
          "app/images/**/*.png"
        ]
      }
    }
  }
```

To use asset selection in SDK 52 and later, include the property `updates.assetPatternsToBeBundled` in your app config. It should define one or more file-matching patterns (regular expressions). For example, an **app.json** file has the patterns defined in the following way:

```json
"expo": {
    ... 
    "updates": {
      "assetPatternsToBeBundled": [
        "app/images/**/*.png"
      ]
    }
  }
```

After adding this configuration all **.png** files in all subdirectories of **app/images** will be included in updates. You have to also ensure that these assets need to be required in your JavaScript code.

If `assetPatternsToBeBundled` isn't included in the app config, all assets resolved by the bundler will be included in updates (as per SDK 49 and earlier behavior).

> Asset selection controls which assets are eligible for over-the-air updates. It does not change which assets are bundled into the native binary, so it does not reduce app startup time.

## Verifying that an update includes all required app assets

When using the asset selection, assets that do not match any file patterns will resolve in the Metro bundler. However, these assets will not be uploaded to the updates server. You have to be certain that assets not included in updates are built into the native build of the app.

If you are building your app locally or have access to the correct build for publishing updates (with the same [runtime version](/eas-update/runtime-versions.md)), then use the `npx expo-updates assets:verify` command. It allows you to check whether all required assets will be included when you publish an update:

```sh
npx expo-updates assets:verify
```

> This new command is part of the `expo-updates` CLI, which also supports [EAS Update code signing](/eas-update/code-signing.md). It is not part of the [Expo CLI](/more/expo-cli.md) or the [EAS CLI](https://github.com/expo/eas-cli). Only available for ([`expo-updates`](/versions/latest/sdk/updates.md) >= 0.24.10).

You can also use the `--help` option with the command to see the available options:

| Option | Description |
| --- | --- |
| `<dir>` | Directory of the Expo project. Default: Current working directory. |
| `-a, --asset-map-path <path>` | Path to the **assetmap.json** in an export produced by the command `npx expo export --dump-assetmap` . |
| `-e, --exported-manifest-path <path>` | Path to the **metadata.json** in an export produced by the command `npx expo export --dump-assetmap`. |
| `-b, --build-manifest-path <path>` | Path to the **app.manifest** file created by `expo-updates` in an Expo application build (either **android** or **ios**). |
| `-p, --platform <platform>` | Options: ["android", "ios"] |
| `-h, --help` | Usage info. |

## Example

[Working example](https://github.com/expo/UpdatesAPIDemo) — See a working example of using asset selection, the assets:verify command, and other EAS Update features. — assets:verify
