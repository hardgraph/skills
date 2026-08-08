---
modificationDate: December 09, 2025
title: EAS Workflows limitations
description: Learn about the current limitations of EAS Workflows.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/workflows/limitations/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/workflows/limitations/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Workflows
Pages in this section:
- [Introduction](https://docs.expo.dev/eas/workflows/introduction.md)
- [Get started](https://docs.expo.dev/eas/workflows/get-started.md)
- [Pre-packaged jobs](https://docs.expo.dev/eas/workflows/pre-packaged-jobs.md)
- [Syntax](https://docs.expo.dev/eas/workflows/syntax.md)
- [Environment variables](https://docs.expo.dev/eas/workflows/environment.md)
- [Automating EAS CLI commands](https://docs.expo.dev/eas/workflows/automating-eas-cli.md)
- [REST API](https://docs.expo.dev/eas/workflows/rest-api.md)
- [Troubleshooting](https://docs.expo.dev/eas/workflows/troubleshooting.md)
- [Limitations](https://docs.expo.dev/eas/workflows/limitations.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# EAS Workflows limitations

Learn about the current limitations of EAS Workflows.

EAS Workflows is designed to help you automate your development and release processes. However, it is good to be aware of certain limitations that we plan to address since they could affect your workflow automation strategy.

## No shared workflow configurations

At this time, it's not possible to define shared workflow configurations. Each workflow needs to be defined independently, which may lead to some configuration duplication.

## No matrix support

Matrix builds are not currently supported in EAS Workflows. This means you cannot run multiple variations of the same workflow in parallel with different configurations.

## Get notified about changes

To be notified as progress is made on these items, you can subscribe to the EAS newsletter on [expo.dev/eas](https://expo.dev/eas).
