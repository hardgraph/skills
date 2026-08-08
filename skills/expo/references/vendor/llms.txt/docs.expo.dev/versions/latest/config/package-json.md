---
title: package.json
description: A reference for Expo-specific properties that can be used in the package.json file.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/config/package-json/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/config/package-json/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Reference (v57.0.0) > Configuration files
Pages in this section:
- [app.json / app.config.js](https://docs.expo.dev/versions/latest/config/app.md)
- [babel.config.js](https://docs.expo.dev/versions/latest/config/babel.md)
- [metro.config.js](https://docs.expo.dev/versions/latest/config/metro.md)
- [package.json](https://docs.expo.dev/versions/latest/config/package-json.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# package.json

A reference for Expo-specific properties that can be used in the package.json file.

**package.json** is a JSON file that contains the metadata for a JavaScript project. This is a reference to Expo-specific properties that can be used by adding an `expo` field in the **package.json** file.

## `install.exclude`

The following commands perform a version check for the libraries installed in a project and give a warning when a library's version is different from the version recommended by Expo:

-   `npx expo start` and `npx expo-doctor`
-   `npx expo install` (when installing a new version of that library or using `--check` or `--fix` options)

By adding the library under the `install.exclude` array in the **package.json** file, you can exclude it from the version checks:

```json
{
  "expo": {
    "install": {
      "exclude": ["expo-updates", "expo-splash-screen"]
    }
  }
}
```

## `autolinking`

Allows configuring module resolution behavior by using `autolinking` property in **package.json**.

```json
{
  "expo": {
    "autolinking": {
      "nativeModulesDir": "./modules"
    }
  }
}
```

For complete reference, see [Autolinking configuration](/modules/autolinking.md#configuration).

## `doctor`

Allows configuring the behavior of the [`npx expo-doctor`](/develop/tools.md#expo-doctor) command.

### `reactNativeDirectoryCheck`

By default, Expo Doctor validates your project's packages against the [React Native directory](https://reactnative.directory/). This check throws a warning with a list of packages that are not included in the React Native Directory.

You can customize this check by adding the following configuration in your project's **package.json** file:

```json
{
  "expo": {
    "doctor": {
      "reactNativeDirectoryCheck": {
        "enabled": true,
        "exclude": ["/foo/", "bar"],
        "listUnknownPackages": true
      }
    }
  }
}
```

By default, the check is enabled and unknown packages are listed.

### `appConfigFieldsNotSyncedCheck`

Expo Doctor checks if your project includes native project directories such as **android** or **ios**. If these directories exist but are not listed in your **.gitignore** or [**.easignore**](/build-reference/easignore.md) files, Expo Doctor verifies the presence of an app config file. If this file exists, it means your project is configured to use [Prebuild](/more/glossary-of-terms.md#prebuild).

When the **android** or **ios** directories are present, EAS Build does not sync app config properties to the native projects. Expo Doctor throws a warning if these conditions are true.

You can disable or enable this check by adding the following configuration to your project's **package.json** file:

```json
{
  "expo": {
    "doctor": {
      "appConfigFieldsNotSyncedCheck": {
        "enabled": false
      }
    }
  }
}
```
