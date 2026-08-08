---
modificationDate: June 22, 2023
title: Sync credentials between remote and local sources
description: Learn how to sync credentials between remote and local sources.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/app-signing/syncing-credentials/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/app-signing/syncing-credentials/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Build > App signing
Pages in this section:
- [App credentials](https://docs.expo.dev/app-signing/app-credentials.md)
- [Automatically managed credentials](https://docs.expo.dev/app-signing/managed-credentials.md)
- [Local credentials](https://docs.expo.dev/app-signing/local-credentials.md)
- [Existing credentials](https://docs.expo.dev/app-signing/existing-credentials.md)
- [Sync credentials between remote and local sources](https://docs.expo.dev/app-signing/syncing-credentials.md) (this page)
- [Security](https://docs.expo.dev/app-signing/security.md)
- [Apple Developer Program roles and permissions](https://docs.expo.dev/app-signing/apple-developer-program-roles-and-permissions.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Sync credentials between remote and local sources

Learn how to sync credentials between remote and local sources.

If you use automatically managed credentials, your credentials will be hosted remotely on EAS servers, but you may encounter a situation where you want to pull your credentials down to run a build locally. And if you use local credentials, you may find yourself in a position where you want to upload credentials specified in **credentials.json** up to EAS to be managed for you. Both of these are possible using the `eas credentials` command.

## Downloading credentials

To download your automatically managed credentials, run `eas credentials` in the root of your project, pick a platform, choose `"Credentials.json: Upload/Download credentials between EAS servers and your local json"`, and then `"Download credentials from EAS to credentials.json"`. Run the command again to download the credentials for another platform, if needed.

Android credentials will be ready to use immediately because your project will read the credentials from **credentials.json**.

iOS credentials require two steps to set up locally. You will first need to install the distribution certificate into your keychain. Next, open your project Xcode and navigate to the "Signing & Capabilities" section, then import your provisioning profile and select it.

## Uploading credentials

To upload your credentials from **credentials.json** to be managed by EAS, run `eas credentials` in the root of your project, pick a platform, choose `"Credentials.json: Upload/Download credentials between EAS servers and your local json"`, and then `"Upload credentials from credentials.json to EAS"`. Run the command again to upload the credentials for another platform, if needed.
