---
modificationDate: July 08, 2026
title: 'Using Git tags to trigger production deployments'
description: Learn how to trigger production deployments from Git tags on EAS Workflows.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/cicd/tag-based-releases/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/cicd/tag-based-releases/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Tag-based releases](https://docs.expo.dev/tutorial/cicd/tag-based-releases.md) (this page)
- [Web deployments](https://docs.expo.dev/tutorial/cicd/web-deployments.md)
- [Next steps](https://docs.expo.dev/tutorial/cicd/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Using Git tags to trigger production deployments

Learn how to trigger production deployments from Git tags on EAS Workflows.

Pushing to a release branch runs the production workflow on every commit, even ones we never meant as a release.

## Learning outcomes

-   Switch the production workflow trigger from a `release/*` branch to a Git tag matching `v*.*.*`
-   Cut a release by pushing a tag from `main` and watch the workflow run on the EAS dashboard
-   Exclude pre-release tags like `v1.0.0-rc.1` from production and run release candidates in a separate workflow

## Tags as release events

EAS Workflows supports tag-based triggers through [`on.push`](/eas/workflows/syntax.md#onpush) with a `tags` list. The workflow runs only when a tag matching a glob pattern is pushed to the remote. A Git tag is a one-time event tied to a specific commit and version, unlike a branch that can be updated over time. That makes tags well-suited for marking releases.

A tag-based release workflow fits teams that:

-   Want the version number in Git to match the version they ship to app users
-   Release from `main` directly rather than maintaining long-lived release branches
-   Want a clear audit trail of every release commit by version

```
Trigger comparison: a push to a release branch runs the production workflow on every commit, while a push of a version tag runs the workflow only when a tag is pushed.
```

## Switch production.yml to a tag trigger

Open the **.eas/workflows/production.yml** workflow from the [previous chapter](/tutorial/cicd/production.md) and replace the `on.push.branches` trigger with `on.push.tags`. Everything else inside the workflow stays the same.

### Change the trigger

Replace the `branches` trigger with a `tags` trigger. The `tags` glob matches tags like `v1.0.0`, `v2.3.7`, and `v10.0.0-beta.1`.

```yaml
name: Deploy to production

on:
  push:
    tags: ['v*.*.*']

jobs:
  fingerprint:
    name: Fingerprint
    type: fingerprint
    environment: production
  get_android_build:
    name: Check for existing Android build
    needs: [fingerprint]
    type: get-build
    params:
      fingerprint_hash: ${{ needs.fingerprint.outputs.android_fingerprint_hash }}
      profile: production
  get_ios_build:
    name: Check for existing iOS build
    needs: [fingerprint]
    type: get-build
    params:
      fingerprint_hash: ${{ needs.fingerprint.outputs.ios_fingerprint_hash }}
      profile: production
  build_android:
    name: Build Android
    needs: [get_android_build]
    if: ${{ !needs.get_android_build.outputs.build_id }}
    type: build
    params:
      platform: android
      profile: production
  build_ios:
    name: Build iOS
    needs: [get_ios_build]
    if: ${{ !needs.get_ios_build.outputs.build_id }}
    type: build
    params:
      platform: ios
      profile: production
  update_android:
    name: Publish Android update
    needs: [get_android_build]
    if: ${{ needs.get_android_build.outputs.build_id }}
    type: update
    params:
      branch: production
      platform: android
  update_ios:
    name: Publish iOS update
    needs: [get_ios_build]
    if: ${{ needs.get_ios_build.outputs.build_id }}
    type: update
    params:
      branch: production
      platform: ios
```

For tags to trigger the workflow, we need to pick a pattern that matches our team's flow. Common patterns include:

-   `['v*']` matches any tag that starts with `v`
-   `['v*.*.*']` matches strict three-part semantic versions (recommended)

We don't need to change any other jobs. The `fingerprint`, `get-build`, `build`, and `update` jobs work identically because they operate on the commit.

### Commit and push the workflow change

Commit the change to **production.yml** and push it to `main`. This doesn't trigger the workflow because we switched from a branch trigger to a tag trigger.

```sh
git add .eas/workflows/production.yml
git commit -m "Switch production workflow to tag trigger"
git push origin main
```

### Create a tag to test the workflow

To run the workflow, we need to push a tag that matches the glob pattern defined in **production.yml**.

Make a TypeScript/JavaScript change in our example project, commit it, and push a new tag:

```sh
git add .
git commit -m "Fix welcome copy"
git tag v0.1.0
git push origin main --tags
```

The workflow triggers on the `v0.1.0` tag. Open the EAS dashboard, and under **Workflows**, notice "Deploy to production" running with `refs/tags/v0.1.0` as its branch:

Since this is the first production run with the fingerprint generated for a tag-based release, the `build` jobs run for both Android and iOS. Subsequent tags that point to TypeScript/JavaScript changes skip the build jobs and go straight to `update` jobs.

## Pre-release tags for release candidates

The `v*.*.*` glob also matches pre-release tags like `v1.0.0-rc.1`, which would ship a release candidate to app users. To keep release candidates out of production, exclude them from the production workflow's trigger:

```yaml
on:
  push:
    tags: ['v*.*.*', '!v*.*.*-rc.*']
```

To exercise a release candidate before the real release, create a separate workflow that matches only pre-release tags (`tags: ['v*.*.*-rc.*']`) and reuses the `fingerprint`, `get-build`, and `build` jobs without the `update` and `submit` jobs. The same pipeline runs, and nothing reaches app users.

## Summary

Chapter 6: Tag-based releases

We switched the production workflow trigger from a release branch to a Git tag, tested the change by pushing a versioned tag from main, and learned how to gate release candidates with a pre-release tag glob.

In the next chapter, learn how to deploy web builds to EAS Hosting from a workflow.

[Next: Chapter 7: Web deployments](/tutorial/cicd/web-deployments.md)
