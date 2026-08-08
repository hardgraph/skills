---
modificationDate: July 07, 2025
title: Rollouts
description: Learn how to incrementally deploy updates to your users by using a rollout mechanism.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas-update/rollouts/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas-update/rollouts/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Update > Deployment
Pages in this section:
- [Deploy updates](https://docs.expo.dev/eas-update/deployment.md)
- [Downloading updates](https://docs.expo.dev/eas-update/download-updates.md)
- [Rollouts](https://docs.expo.dev/eas-update/rollouts.md) (this page)
- [Rollbacks](https://docs.expo.dev/eas-update/rollbacks.md)
- [Serve bundle diffs](https://docs.expo.dev/eas-update/bundle-diffing.md)
- [Optimize assets](https://docs.expo.dev/eas-update/optimize-assets.md)
- [Alternative deployment patterns](https://docs.expo.dev/eas-update/deployment-patterns.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Rollouts

Learn how to incrementally deploy updates to your users by using a rollout mechanism.

A rollout allows you to roll out a change to a portion of your users to catch bugs or other issues before releasing that change to all your users.

EAS provides **per-update and branch-based rollout mechanisms** depending on your use case.

## Per-update rollouts

This rollout mechanism allows you to specify a percentage of users that should receive a new update when you publish it, and then increase that percentage gradually afterwards.

### Starting

To start an update-based rollout, add the `--rollout-percentage` flag to your normal `eas update` command:

```sh
eas update --rollout-percentage=10
```

In this example, when published, the update will only be available to 10% of your end users.

### Progressing

To edit the percentage of an update-based rollout:

```sh
eas update:edit
```

You will be guided through the process of selecting the update to edit and asked for the new percentage.

### Ending

When ending an update-based rollout, you have two options:

-   **Roll out fully**: To accomplish this end state, progress the rollout as detailed above and set the percentage to 100.
-   **Revert back to previous state**: To accomplish this, run `eas update:revert-update-rollout` which will guide you through reverting back to the previous state.

### Additional notes

-   Only one update can be rolled out on a branch at one time.
-   When a rollout is in progress, it must be ended using one of the options above before a new update (with the same runtime version) can be published. This prevents accidentally clobbering the rollout.
-   To see the state of the rollout, use the `eas update:list` or `eas update:view` commands.
-   Reverting a rollout that is created on a branch with an existing update will republish the control update. This ensures that all clients are reverted back to the previous state.
-   A rollout can be started on a branch with no current update, in which case the first update will be rolled out to the specified percentage of users. When reverted, a rollback-to-embedded update will be created, which will revert the clients to their previous state (embedded update).

## Branch-based rollouts

This rollout mechanism allows you to incrementally roll out a set of updates on a new branch to a percentage of end users and leave the remaining percentage of users on the current branch.

### Starting

To start a branch-based rollout, run the following EAS CLI command:

```sh
eas channel:rollout
```

In the terminal, an interactive guide will assist you in selecting a channel, choosing a branch for the rollout, and setting the percentage of users for the rollout. To increase or decrease the rollout amount, run the command again and choose the `Edit` option to adjust the rollout percentage.

### Ending

Two methods are available to end a rollout when you choose the `End` option in the interactive guide:

-   **Republish and revert:** Use this option when you are confident with the state of the new branch. This will republish the latest update from the new branch to the old branch, and all users will be pointed to the old branch.
-   **Revert:** Choose to disregard the updates on the new branch and return users to the old branch.

### Additional notes

-   Only one branch can be rolled out on a channel at a single time.
-   To see the state of the rollout, use the `eas channel:rollout` command.
-   When a rollout is in progress, you can publish updates to both rolled out and current branches by running `eas update --branch [branch]`, for example.
-   `eas update --channel [channel]` cannot be used when a rollout is in progress since it cannot know which branch in the rollout to associate the update with.
