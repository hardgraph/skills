---
modificationDate: July 22, 2026
title: Submit to the Apple App Store with EAS Submit
description: Learn how to submit your iOS app to the Apple App Store with EAS Submit.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/submit/ios/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/submit/ios/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Submit
Pages in this section:
- [Submit to Google Play Store](https://docs.expo.dev/submit/android.md)
- [Submit to Apple App Store](https://docs.expo.dev/submit/ios.md) (this page)
- [TestFlight](https://docs.expo.dev/submit/testflight.md)
- [Manual Android submission](https://docs.expo.dev/submit/android-manual.md)
- [Manual iOS submission](https://docs.expo.dev/submit/ios-manual.md)
- [Configure with eas.json](https://docs.expo.dev/submit/eas-json.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Submit to the Apple App Store with EAS Submit

Learn how to submit your iOS app to the Apple App Store with EAS Submit.

[EAS Submit](/deploy/submit-to-app-stores.md) is the recommended way to upload your iOS app to the Apple App Store. The `eas submit` command works the same way on your machine and inside CI/CD. [EAS Workflows](/eas/workflows/introduction.md) is the simplest way to run it automatically after a build. EAS Submit works on macOS, Linux, and Windows, so you don't need a Mac to ship iOS builds.

## Prerequisites

#### Prerequisites

##### Sign up for an Apple Developer account

A paid Apple Developer account is required to submit apps to the Apple App Store. Sign up on the [Apple Developer Portal](https://developer.apple.com/account/).

##### Include a bundle identifier in app config

Include your app's bundle identifier in **app.json**:

```json
{
  "ios": {
    "bundleIdentifier": "com.yourcompany.yourapp"
  }
}
```

##### Install EAS CLI and authenticate with your Expo account

Install EAS CLI and log in with your Expo account:

```sh
# npm
npm install --global eas-cli && eas login

# yarn
yarn global add eas-cli && eas login

# pnpm
pnpm add --global eas-cli && eas login

# bun
bun add --global eas-cli && eas login
```

## Build a production app

You need a production **.ipa** to submit. Create one with [EAS Build](/build/introduction.md):

```sh
eas build --platform ios --profile production
```

Alternatively, build on your own computer with `eas build --platform ios --profile production --local` or with Xcode.

## Submit with `eas submit`

Once the build is ready, submit it to the Apple App Store:

```sh
eas submit --platform ios
```

The command will walk you through selecting a build, prompt for your Apple ID on first run, and upload the binary to App Store Connect. The build appears in [TestFlight](/submit/testflight.md) after processing (usually 10-15 minutes). To release to production, log in to [App Store Connect](https://appstoreconnect.apple.com/) and submit the build for App Review.

### Configure a submission profile

Add a submission profile in **eas.json** with your App Store Connect app ID:

```json
{
  "submit": {
    "production": {
      "ios": {
        "ascAppId": "your-app-store-connect-app-id"
      }
    }
  }
}
```

#### How to find ascAppId

1.  Sign in to [App Store Connect](https://appstoreconnect.apple.com/) and select your team.
2.  Navigate to [Apps](https://appstoreconnect.apple.com/apps).
3.  Click your app.
4.  Ensure the **App Store** tab is active.
5.  On the left pane, under **General**, select **App Information**.
6.  Your `ascAppId` is listed under **General Information** as **Apple ID**.

See the [eas.json reference](/eas/json.md#ios-specific-options-1) for every available option.

### Build and submit in one step

Pass `--auto-submit` to `eas build` to hand the finished build off to EAS Submit automatically:

```sh
eas build --platform ios --auto-submit
```

See [Automate submissions](/build/automate-submissions.md) for details.

## Automate with EAS Workflows

[EAS Workflows](/eas/workflows/introduction.md) runs the same submit step on EAS infrastructure, triggered by a git push or run manually from the CLI. First, configure an App Store Connect API Key so workflows can authenticate with Apple non-interactively:

```sh
eas credentials --platform ios
```

1.  Select the `production` build profile.
2.  Log in with your Apple Developer account and follow the prompts.
3.  Select **App Store Connect: Manage your API Key**.
4.  Select **Set up your project to use an API Key for EAS Submit**.

#### Prefer to bring your own credentials?

**App Store Connect API Key:** Create your own [API Key](https://expo.fyi/creating-asc-api-key) and set it with the `ascApiKeyPath`, `ascApiKeyIssuerId`, and `ascApiKeyId` fields in **eas.json**.

**App-specific password:** Provide your [app-specific password](https://expo.fyi/apple-app-specific-password) via the `EXPO_APPLE_APP_SPECIFIC_PASSWORD` environment variable and set your Apple ID with the `appleId` field in **eas.json**.

Create a workflow file named **.eas/workflows/submit-ios.yml** with the following contents:

```yaml
name: Submit iOS

on:
  push:
    branches: ['main']

jobs:
  build_ios:
    name: Build iOS app
    type: build
    params:
      platform: ios
      profile: production

  submit_ios:
    name: Submit to TestFlight
    needs: [build_ios]
    type: testflight
    params:
      build_id: ${{ needs.build_ios.outputs.build_id }}
```

This builds an iOS app on every push to `main` and submits it to TestFlight. The [pre-packaged `testflight` job](/eas/workflows/pre-packaged-jobs.md#testflight) can also share the build with internal and external testing groups. Trigger the workflow manually with:

```sh
eas workflow:run submit-ios.yml
```

See the [workflow examples guide](/eas/workflows/examples/introduction.md) for more patterns.

## Use other CI/CD services

You can run `eas submit` from any CI/CD service, such as GitHub Actions, GitLab CI, and others:

```sh
eas submit --platform ios --profile production
```

This requires a [personal access token](/accounts/programmatic-access.md#personal-access-tokens) to authenticate with your Expo account. Set the `EXPO_TOKEN` environment variable in your CI service so `eas submit` can run non-interactively.

## Manual fallback

If EAS Submit is temporarily unavailable, you can upload to the Apple App Store manually from a Mac with Xcode.

[Manually submit an iOS app with Xcode](/submit/ios-manual.md) — Archive and upload an iOS app to App Store Connect using Xcode on macOS.
