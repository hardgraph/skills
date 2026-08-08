---
modificationDate: July 22, 2026
title: Publish your web app
description: Learn how to deploy your web app using EAS Hosting.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/deploy/web/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/deploy/web/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > Deploy
Pages in this section:
- [Build project for app stores](https://docs.expo.dev/deploy/build-project.md)
- [Submit to app stores](https://docs.expo.dev/deploy/submit-to-app-stores.md)
- [App stores metadata](https://docs.expo.dev/deploy/app-stores-metadata.md)
- [Send over-the-air updates](https://docs.expo.dev/deploy/send-over-the-air-updates.md)
- [Deploy web apps](https://docs.expo.dev/deploy/web.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Publish your web app

Learn how to deploy your web app using EAS Hosting.

If you are building a universal app, you can quickly deploy your web app using [EAS Hosting](/eas/hosting/introduction.md). It is a service for deploying web apps built with Expo Router and React.

#### Prerequisites

##### Set expo.web.output in app.json

In your project's **app.json**, ensure that the [`expo.web.output`](/versions/latest/config/app.md#output) property is either `static` or `server`.

## Export your web project

To deploy your web app, you need to create a static build of your web project. Export your web project into a **dist** directory by running the following command:

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

> Remember to re-run this command every time before deploying when you make changes to your web app.

## Initial deployment

To publish your web app, run the following [EAS CLI](/develop/tools.md#eas-cli) command:

```sh
eas deploy
```

After running this command for the first time, you'll be prompted to select a preview subdomain for your project. This subdomain is a prefix used to create a preview URL and is used for production deployments. For example, in `https://test-app--1234.expo.app`, `test-app` is the preview subdomain.

Once your deployment is complete, the EAS CLI will output a preview URL to access your deployed app.

## Production deployment

To create a production deployment, run the following [EAS CLI](/develop/tools.md#eas-cli) command:

```sh
eas deploy --prod
```

Once your deployment is complete, the EAS CLI will output a production URL to access your deployed app.

## Deploy automatically

You can automatically deploy your app to the web with [EAS Workflows](/eas/workflows/introduction.md). First, you'll need to [configure your project](/eas/workflows/get-started.md), add a file named **.eas/workflows/deploy-web.yml** at the root of your project, then add the following workflow configuration:

```yaml
name: Deploy web

on:
  push:
    branches: ['main']

jobs:
  deploy_web:
    name: Deploy web
    type: deploy
    params:
      prod: true
```

The workflow above will create a web deployment on every commit to your project's `main` branch. You can also run this workflow manually with the following EAS CLI command:

```sh
eas workflow:run deploy-web.yml
```

Learn more about common patterns with the [workflows examples guide](/eas/workflows/examples/introduction.md).

### Expo Skills for AI agents

If you use an AI agent, install [Expo Skills](/skills.md) to teach it how to deploy your web app:

[eas-hosting](https://github.com/expo/skills/blob/main/plugins/expo/skills/eas-hosting/SKILL.md) — Deploy Expo websites and Expo Router API routes to EAS Hosting - export the web bundle, run eas deploy for production and PR preview URLs, manage environment secrets and custom domains, and work within the Cloudflare Workers runtime.

## Learn more

You can learn more about setting up [deployment aliases](/eas/hosting/deployments-and-aliases.md), using a [custom domain](/eas/hosting/custom-domain.md), or [deploying an API Route](/router/web/api-routes.md#deployment).
