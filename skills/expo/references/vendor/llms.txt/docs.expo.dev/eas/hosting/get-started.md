---
modificationDate: June 30, 2026
title: Deploy your first Expo Router and React app
description: Learn how to deploy your Expo Router and React apps to EAS Hosting.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/hosting/get-started/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/hosting/get-started/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Hosting
Pages in this section:
- [Introduction](https://docs.expo.dev/eas/hosting/introduction.md)
- [Get started with EAS Hosting](https://docs.expo.dev/eas/hosting/get-started.md) (this page)
- [Deployments and aliases](https://docs.expo.dev/eas/hosting/deployments-and-aliases.md)
- [Custom domain](https://docs.expo.dev/eas/hosting/custom-domain.md)
- [Monitor API routes](https://docs.expo.dev/eas/hosting/api-routes.md)
- [Web deployments with EAS Workflows](https://docs.expo.dev/eas/hosting/workflows.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Deploy your first Expo Router and React app

Learn how to deploy your Expo Router and React apps to EAS Hosting.

EAS Hosting is a react hosting service that allows you to deploy an exported Expo web build to a preview or production URL.

This guide will walk you through the process of creating your first web deployment.

[Watch: Deploy your Expo Router web project](https://www.youtube.com/watch?v=NaKsfWciJLo) — Set up EAS Hosting for your Expo Router web project, create your first deployment, and get a preview URL up and running.

#### Prerequisites

##### An Expo account

EAS Hosting is available to anyone with an Expo account, regardless of whether you pay for EAS or use the Free plan. You can sign up at [expo.dev/signup](https://expo.dev/signup).

Paid subscribers can create more deployments, have more bandwidth, storage, and requests, and may set up a custom domain. Learn more about different plans and benefits at [EAS pricing](https://expo.dev/pricing#host).

##### An Expo Router web project

Don't have a project yet? It's quick and easy to create a "Hello world" app you can use with this guide. Run the following command to create a new project:

```sh
# npm
npx create-expo-app@latest my-app --template default@sdk-57

# yarn
yarn create expo-app my-app --template default@sdk-57

# pnpm
pnpm create expo-app my-app --template default@sdk-57

# bun
bun create expo my-app --template default@sdk-57
```

## Install the latest EAS CLI

EAS CLI is the command-line app you will use to interact with EAS services from your terminal. To install it, run the command:

```sh
# npm
npm install --global eas-cli

# yarn
yarn global add eas-cli

# pnpm
pnpm add --global eas-cli

# bun
bun add --global eas-cli
```

You can also use the above command to check if a new version of EAS CLI is available. We encourage you to always stay up to date with the latest version.

> We recommend using `npm` instead of `yarn` for global package installations. You may alternatively use `npx eas-cli@latest`. Remember to use that instead of `eas` whenever it's called for in the documentation.

## Log in to your Expo account

If you are already signed in to an Expo account using Expo CLI, you can skip the steps described in this section. If you are not, run the following command to log in:

```sh
eas login
```

You can check whether you are logged in by running `eas whoami`.

## Prepare your project

For your app config file's [`expo.web.output`](/versions/latest/config/app.md#output), decide whether to set it to either `single`, `static`, or `server`.

-   `single`: Exports your Expo app to a single-page app with only one `index.html` output
-   `static`: Exports your Expo app to a [statically generated web app](/router/web/static-rendering.md)
-   `server`: Supports [server functions](/guides/server-components.md#react-server-functions) and [API routes](/router/web/api-routes.md) as well as static pages for your app

> Don't worry if you're not sure which output mode you need, you can always change this value later and re-deploy.

### Export your app

You need to export your web project into a **dist** directory. To do this, run:

```sh
# npm
npx expo export --platform web

# yarn
yarn expo export --platform web

# pnpm
pnpm expo export --platform web

# bun
bun expo export --platform web
```

> Remember to re-run this command every time before deploying.

### Deploy your app

Now publish your website to EAS Hosting:

```sh
eas deploy
```

The first time you run this command, it will:

1.  Prompt you to connect an EAS project if you haven't done so yet
2.  Ask you to choose a preview subdomain name

> A **preview subdomain name** is a prefix used for the preview URL of your app. For example, if you choose `my-app` as your preview subdomain name, your preview URL would look something like this: `https://my-app--or1170q9ix.expo.app/`, and your production URL would be: `https://my-app.expo.app/`.

Once your deployment is complete, the CLI will output a preview URL for where your deployed app is accessible, as well as a link to the deployment details on the EAS Dashboard.
