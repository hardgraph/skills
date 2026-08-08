---
modificationDate: April 28, 2026
title: Create development builds with EAS Workflows
description: Learn how to create development builds with EAS Workflows.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/workflows/examples/create-development-builds/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/workflows/examples/create-development-builds/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Workflows > Examples
Pages in this section:
- [Introduction](https://docs.expo.dev/eas/workflows/examples/introduction.md)
- [Create development builds](https://docs.expo.dev/eas/workflows/examples/create-development-builds.md) (this page)
- [Publish preview updates](https://docs.expo.dev/eas/workflows/examples/publish-preview-update.md)
- [Clean up update branches](https://docs.expo.dev/eas/workflows/examples/branch-cleanup.md)
- [Deploy to production](https://docs.expo.dev/eas/workflows/examples/deploy-to-production.md)
- [Run E2E tests](https://docs.expo.dev/eas/workflows/examples/e2e-tests.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Create development builds with EAS Workflows

Learn how to create development builds with EAS Workflows.

[Development builds](/develop/development-builds/introduction.md) are specialized builds of your project that include Expo's developer tools. These types of builds include all native dependencies inside your project, enabling you to run a production-like build of your project on a simulator, emulator, or a physical device. This workflow allows you to create development builds for each platform and for both physical devices, Android emulators, and iOS simulators, which your team can access with `eas build:dev`.

[Expo Golden Workflow: Automate the creation of development builds](https://www.youtube.com/watch?v=u8MAJ0F18s0) — Learn how to automate development builds for Android, iOS devices, and simulators using EAS Workflows.

## Get started

#### Prerequisites

##### Set up your environment

To get started, you'll need to configure your project and devices to build and run development builds. Learn how to set up your environment for development builds with the following guides:

[Android device setup](/get-started/set-up-your-environment.md?mode=development-build&platform=android&device=physical) — Get your project ready for development builds.

[Android Emulator setup](/get-started/set-up-your-environment.md?mode=development-build&platform=android&device=simulated) — Get your project ready for development builds.

[iOS device setup](/get-started/set-up-your-environment.md?mode=development-build&platform=ios&device=physical) — Get your project ready for development builds.

[iOS Simulator setup](/get-started/set-up-your-environment.md?mode=development-build&platform=ios&device=simulated) — Get your project ready for development builds.

##### Create build profiles

After you've configured your project and devices, add the following build profiles to your **eas.json** file.

```json
{
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal"
    },
    "development-simulator": {
      "developmentClient": true,
      "distribution": "internal",
      "ios": {
        "simulator": true
      }
    }
  }
}
```

The following workflow creates a build for each platform and for both physical devices, Android emulators, and iOS simulators. They all will run in parallel.

```yaml
name: Create development builds

jobs:
  android_development_build:
    name: Build Android
    type: build
    params:
      platform: android
      profile: development
  ios_device_development_build:
    name: Build iOS device
    type: build
    params:
      platform: ios
      profile: development
  ios_simulator_development_build:
    name: Build iOS simulator
    type: build
    params:
      platform: ios
      profile: development-simulator
```

Run the above workflow with:

```sh
eas workflow:run .eas/workflows/create-development-builds.yml
```
