---
modificationDate: July 21, 2026
title: Automate submissions
description: Learn how to enable automatic submissions with EAS Build.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build/automate-submissions/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build/automate-submissions/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Build
Pages in this section:
- [Introduction](https://docs.expo.dev/build/introduction.md)
- [Create your first build](https://docs.expo.dev/build/setup.md)
- [Configure with eas.json](https://docs.expo.dev/build/eas-json.md)
- [Internal distribution](https://docs.expo.dev/build/internal-distribution.md)
- [Automate submissions](https://docs.expo.dev/build/automate-submissions.md) (this page)
- [Using EAS Update](https://docs.expo.dev/build/updates.md)
- [Trigger builds from CI](https://docs.expo.dev/build/building-on-ci.md)
- [Trigger builds from GitHub App](https://docs.expo.dev/build/building-from-github.md)
- [Expo Orbit](https://docs.expo.dev/build/orbit.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Automate submissions

Learn how to enable automatic submissions with EAS Build.

Many mobile deployment processes eventually evolve to the point where the app is automatically submitted to the respective store once an appropriate build is completed. This saves developers from having to wait around for the build to complete, avoids a bit of manual work, and eliminates the need to coordinate providing app store credentials to the team.

EAS Build gives you automatic submissions out of the box with the `--auto-submit` flag. This flag tells EAS Build to pass the build along to EAS Submit with the appropriate submission profile upon completion. Refer to the [EAS Submit documentation](/deploy/submit-to-app-stores.md) for more information on how to set up and configure submissions.

When you run `eas build --auto-submit` you will be provided with a link to a submission details page, where you can track the progress of the submission. You can also find this page at any time on the [submissions dashboard for your project](https://expo.dev/accounts/%5Baccount%5D/projects/%5Bproject%5D/submissions), and it is linked from your build details page.

## Selecting a submission profile

By default, `--auto-submit` will try to use a submission profile with the same name as the selected build profile. If this does not exist, or if you prefer to use a different profile, you can use `--auto-submit-with-profile=<profile-name>` instead.

## Build profile environment variables and submissions

When running `eas build --profile <profile-name> --auto-submit`, the project's **app.config.js** will be evaluated using any environment variables associated with the build profile `<profile-name>`. For example, suppose we ran `eas build -p ios --profile production --auto-submit` with the following configuration:

```json
{
  "build": {
    "production": {
      "env": {
        "APP_ENV": "production"
      }
    },
    "development": {
      "env": {
        "APP_ENV": "development"
      }
    }
  }
}
```

```js
export default () => {
  return {
    name: process.env.APP_ENV === 'production' ? 'My App' : 'My App (DEV)',
    ios: {
      bundleIdentifier: process.env.APP_ENV === 'production' ? 'com.my.app' : 'com.my.app-dev',
    },
    // ... other config here
  };
};
```

The `APP_ENV` variable from the `production` profile will be used when evaluating **app.config.js** for the submission, and therefore the name will be `My App` and the bundle identifier will be `com.my.app`.

## Default submission behavior for app stores

By default, the `--auto-submit` flag will make your build available for internal testing, but will not automatically submit your app to review for public distribution. Sections below describe the default submission behavior for Android and iOS.

### Android submissions

For Android, if sufficient metadata is not provided, the default behavior is to create an internal release for new apps. To control where and how your build is submitted, you can specify the `releaseStatus` and `track` fields in your **eas.json** submission profile:

**Release status options:**

-   `draft`: Creates a draft release that requires manual promotion in the Google Play Console
-   `completed`: Immediately releases to users on the specified track
-   `inProgress`: Staged rollout release (use with `rollout` percentage)
-   `halted`: Halted release

When you explicitly set a track to your submission profile in **eas.json**, the `--auto-submit` flag will submit the build to the chosen track. This also requires the `releaseStatus` to be set to `completed`:

**Track options:**

-   `internal`: Internal testing track (up to 100 testers) (default)
-   `alpha`: Closed testing track
-   `beta`: Open testing track
-   `production`: Production track (public release)

### iOS submissions

For iOS, the default submission behavior is to submit the build to TestFlight, but not for App Store review. This means:

-   The build is submitted to TestFlight and becomes available for internal testing.
-   If you have "Enable automatic distribution" turned on in App Store Connect, TestFlight will automatically create a group and invite all your internal TestFlight users to test the build.
-   You can also specify additional TestFlight groups using the [`groups`](/eas/json.md#groups) field in your **eas.json** submission profile.
-   Using [TestFlight](/submit/testflight.md), you can release a version of your app available for internal and external testing. TestFlight allows sharing with up to 100 testers internally and provides a public link to share with up to 10,000 external testers.
-   The release to Apple App Store review is a manual process. Once you have made a submission to TestFlight, you'll have to manually promote the build to the App Store.

This behavior ensures that all iOS releases go through TestFlight when using `--auto-submit`, allowing you to test the release before deciding to make it available to the public.

### Modifying App Store listing (iOS only)

On its own, EAS Submit does not update store metadata (app description, Apple advisory information, languages, and so on). However, once you upload a build to Testflight with EAS Submit with a new version number, you can update this information with EAS Metadata.

[EAS Metadata](/eas/metadata/getting-started.md) — Learn how to update your iOS app's metadata automatically.
