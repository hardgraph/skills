---
modificationDate: February 20, 2026
title: Configure EAS Submit with eas.json
description: Learn how to configure your project for EAS Submit with eas.json.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/submit/eas-json/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/submit/eas-json/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Submit
Pages in this section:
- [Submit to Google Play Store](https://docs.expo.dev/submit/android.md)
- [Submit to Apple App Store](https://docs.expo.dev/submit/ios.md)
- [TestFlight](https://docs.expo.dev/submit/testflight.md)
- [Manual Android submission](https://docs.expo.dev/submit/android-manual.md)
- [Manual iOS submission](https://docs.expo.dev/submit/ios-manual.md)
- [Configure with eas.json](https://docs.expo.dev/submit/eas-json.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Configure EAS Submit with eas.json

Learn how to configure your project for EAS Submit with eas.json.

**eas.json** is the configuration file for EAS CLI and services. It is generated when the [`eas build:configure` command](/build/setup.md#configure-the-project) runs for the first time in your project and is located next to **package.json** at the root of your project. Even though **eas.json** is not mandatory for using EAS Submit, it makes your life easier if you need to switch between different configurations.

## Production profile

Running `eas submit` without specifying a profile name will use the `production` profile if it is already defined in **eas.json** to configure the submission. If no values exist in the `production` profile, EAS CLI will prompt you to provide the values interactively.

The `production` profile shown below is required to run Android and iOS submissions in a CI/CD process, like with [EAS Workflows](/eas/workflows/introduction.md):

```json
{
  "cli": {
    "version": ">= 0.34.0"
  },
  "submit": {
    "production": {
      "android": {
        "track": "internal"
      },
      "ios": {
        "ascAppId": "your-app-store-connect-app-id"
      }
    }
  }
}
```

Learn more about the values you can set with the [Android specific options](/eas/json.md#android-specific-options-1) and the [iOS specific options](/eas/json.md#ios-specific-options-1). You can also learn how to submit to the [Apple App Store](/submit/ios.md) and the [Google Play Store](/submit/android.md).

## Multiple profiles

The JSON object under `submit` can contain multiple submit profiles. Each profile under `submit` can have an arbitrary name as shown in the example below:

```json
{
  "cli": {
    "version": "SEMVER_RANGE",
    "requireCommit": boolean
  },
  "build": {
    // EAS Build configuration
    ... 
  },
  "submit": {
    "SUBMIT_PROFILE_NAME_1": {
      "android": {
        ...ANDROID_OPTIONS
      },
      "ios": {
        ...IOS_OPTIONS
      }
    },
    "SUBMIT_PROFILE_NAME_2": {
      "extends": "SUBMIT_PROFILE_NAME_1",
      "android": {
        ...ANDROID_OPTIONS
      }
    },
    ... 
  }
}
```

When you select a build for submission, it chooses the profile that is used for the selected build. If the profile does not exist, it selects the default `production` profile.

You can also use EAS CLI to pick up another `submit` profile by specifying it with a parameter, for example:

```sh
eas submit --platform ios --profile
```

## Share configuration between `submit` profiles

A `submit` profile can extend another profile using the `extends` key.

For example, in the `preview` profile you may have `"extends": "production"`. This makes the `preview` profile inherit the configuration of the `production` profile.

You can keep chaining profile extensions up to the depth of 5 as long as you avoid making circular dependencies.

## Next step

[EAS Submit schema reference](/eas/json.md#eas-submit) — Learn about available properties for EAS Submit to configure and override their default behavior from within your project.
