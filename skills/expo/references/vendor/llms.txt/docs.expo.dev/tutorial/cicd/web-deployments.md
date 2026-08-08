---
modificationDate: July 08, 2026
title: 'Deploy web apps to EAS Hosting with EAS Workflows'
description: Learn how to deploy web builds to EAS Hosting alongside Android and iOS releases to existing EAS Workflows.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/cicd/web-deployments/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/cicd/web-deployments/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > CI/CD tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/cicd/introduction.md)
- [First EAS Workflows job](https://docs.expo.dev/tutorial/cicd/first-workflow.md)
- [Development builds](https://docs.expo.dev/tutorial/cicd/development-builds.md)
- [Preview builds](https://docs.expo.dev/tutorial/cicd/preview-builds.md)
- [E2E tests](https://docs.expo.dev/tutorial/cicd/e2e-tests.md)
- [Production deployments](https://docs.expo.dev/tutorial/cicd/production.md)
- [Tag-based releases](https://docs.expo.dev/tutorial/cicd/tag-based-releases.md)
- [Web deployments](https://docs.expo.dev/tutorial/cicd/web-deployments.md) (this page)
- [Next steps](https://docs.expo.dev/tutorial/cicd/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Deploy web apps to EAS Hosting with EAS Workflows

Learn how to deploy web builds to EAS Hosting alongside Android and iOS releases to existing EAS Workflows.

Our workflows from previous chapters cover Android and iOS. Let's extend them to also deploy the web build to [EAS Hosting](/eas/hosting/introduction.md) alongside the same release.

## Learning outcomes

-   Configure the Expo project to export a static web build
-   Add a web deployment job to **preview.yml** to generate a non-production web build
-   Add a web deployment job to **production.yml** to generate a production web build

## EAS Hosting and the deploy job

**EAS Hosting** is a service for hosting and deploying web builds of an Expo app. Each deployment gets its own unique URL, and a project has a single production URL that serves whatever we last deployed to production.

The [`deploy`](/eas/workflows/pre-packaged-jobs.md#deploy) pre-packaged job in EAS Workflows runs the web export, uploads the output of the export step to EAS Hosting, and returns the deployment URL as the job output. The `deploy` job has two parameters:

| Parameter | Type | Description |
| --- | --- | --- |
| `prod` | boolean | Optional. If `true`, deploys to the project's production URL. If omitted, deploys as a preview. |
| `alias` | string | Optional. Names a stable alias URL like `awesome-project--staging.expo.app`. |

## Configure web.output for static export

Before we add web deployment jobs to our workflows, we need to configure our Expo project to export a [static web build](/router/web/static-rendering.md). In **app.json**, set the [`web.output`](/versions/latest/config/app.md#web) field to `static`:

```json
{
  "expo": {
    ... 
    "web": {
      "output": "static"
    }
  }
}
```

`static` tells the Expo CLI to produce a directory of static HTML, JavaScript, and asset files that EAS Hosting can serve. Expo also supports `single` and `server` output; `static` produces plain files that any host can serve, which is all we need here.

After updating the **app.json** with the above configuration, commit the change and push it to the repository's `main` branch.

## Add a web preview deployment

Setting `prod: true` is the difference between a preview deployment and a production deployment. Previews get unique per-deployment URLs. We created a preview workflow in [Chapter 3 Step 1](/tutorial/cicd/preview-builds.md#add-a-previewyml). Let's add a `deploy_web` job to that workflow so any time we create a new preview build, we also get a new web deployment.

### Add a deploy_web job

Open **.eas/workflows/preview.yml** and add a `deploy_web` job. This new job runs in parallel with existing build jobs because it has no `needs` dependency.

```yaml
name: Preview builds

jobs:
  # ... existing fingerprint, get-build, build_android, build_ios, notify jobs

  deploy_web:
    name: Deploy web (preview)
    type: deploy
```

With no `prod` parameter, this deploys as a non-production version. Each push gets a unique URL like `https://awesome-project--abc123.expo.app`.

### Push the workflow change to the repository

Commit the change to **preview.yml** and push it to the `main` branch of the GitHub repository. EAS Workflows reads workflow files from the default branch, so this is what makes the change effective.

```sh
git add .eas/workflows/preview.yml
git commit -m "Add web preview deploy job to preview workflow"
git push origin main
```

### Test the workflow

Now, manually run the preview workflow using the following command:

```sh
eas workflow:run .eas/workflows/preview.yml
```

On the EAS dashboard, the preview workflow run shows the `deploy_web` job alongside the build jobs.

Under **Deploy web (preview)**, click the **View Deployment** link to open the deployed web build in the browser.

## Add a web production deployment

The production workflow from [Chapter 6, Step 1](/tutorial/cicd/tag-based-releases.md#change-the-trigger) runs when we push a new Git tag, and it builds or updates the native app. Let's add a `deploy_web` job to that workflow so that releasing a new version of the app also updates the web version on the production URL.

### Add a deploy_web job

Open **.eas/workflows/production.yml** and add a `deploy_web` job. This new job runs in parallel with existing build jobs because it has no `needs` dependency.

```yaml
name: Deploy to production

on:
  push:
    tags: ['v*.*.*']

jobs:
  # ... existing fingerprint, get-build, build, update jobs ...

  deploy_web:
    name: Deploy web (production)
    type: deploy
    params:
      prod: true
```

In the above workflow, the `['v*.*.*']` matches strict three-part semantic versions. The `params.prod: true` deploys the web app to the production URL like `https://awesome-project.expo.app`.

### Create a tag to test the workflow

Commit the workflow change to the `main` branch of the repository:

```sh
git add .eas/workflows/production.yml
git commit -m "Add web production deploy to production workflow"
git push origin main
```

To run the workflow, create a new tag and push it to the repository:

```sh
git tag v0.2.0
git push origin v0.2.0
```

The production workflow now runs on the release of the `v0.2.0` tag. The `deploy_web` job deploys the web build to the production URL.

## Summary

Chapter 7: Web deployments

We configured the Expo project for static web export, added a deploy job to the preview workflow for non-production web URLs, and added a deploy job with \`prod: true\` to the production workflow so every release ships the web build with the native release.

Learn about the next steps to use EAS Workflows.

[Next: Next steps in your journey with EAS Workflows](/tutorial/cicd/next-steps.md)
