---
modificationDate: March 20, 2025
title: Get started with custom builds
description: Learn how to extend EAS Build with custom builds.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/custom-builds/get-started/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/custom-builds/get-started/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Build > Custom builds
Pages in this section:
- [Get started](https://docs.expo.dev/custom-builds/get-started.md) (this page)
- [Config schema](https://docs.expo.dev/custom-builds/schema.md)
- [TypeScript functions](https://docs.expo.dev/custom-builds/functions.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Get started with custom builds

Learn how to extend EAS Build with custom builds.

Custom builds allow customizing the build process for your project by running commands before, during, or after the build process. Customized builds can run from EAS CLI or when running builds in a React Native CI/CD pipeline, like with [EAS Workflows](/eas/workflows/get-started.md).

## Create a custom build config

To get started, create directories and a file named **.eas/build/hello-world.yml** at the same level as **eas.json**. The location and name of both directories are important for EAS Build to identify that a project contains a custom build config.

Inside the **hello-world.yml**, you'll write your custom build config. The filename is unimportant; you can name it whatever you want. The only requirement is that the file extension uses **.yml**.

Add the following custom build config steps in the file:

```yaml
build:
  name: Hello World!
  steps:
    - run: echo "Hello, world!"
    # A built-in function (optional)
```

In a real world scenario, you will call a [built-in function](/custom-builds/schema.md#built-in-eas-functions) to trigger the build.

## Add `config` property in eas.json

To use the custom build config, add the `config` property in **eas.json** under a build profile.

Let's create a new [build profile](/build/eas-json.md#build-profiles) called `test` under `build` to run the custom config from the **test.yml** file:

```json
{
  "build": {
    ... 
    "test": {
      "config": "test.yml",
    },
}
```

If you wish to use separate configs for each platform, you can create separate YAML config files for Android and iOS. For example:

```json
{
  "build": {
    ... 
    "test": {
      "ios": {
        "config": "hello-ios.yml",
      },
      "android": {
        "config": "hello-android.yml",
      }
    },
}
```

## Run a build to test the custom build config

To test the custom build config, run the following command:

```sh
eas build -p android -e test
```

After the build is complete, you can verify that the `echo "Hello World!"` script was executed by checking the logs on the build's detail page.

## Learn more

Check out the example repository for more detailed examples:

[Custom build example repository](https://github.com/expo/eas-custom-builds-example/tree/main) — A custom EAS Build example that includes examples for custom builds such as setting up functions, using environment variables, uploading artifacts, and more.
