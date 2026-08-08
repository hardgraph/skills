---
modificationDate: July 08, 2026
title: Overview of Expo and EAS tutorials
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/overview/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/overview/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn
Pages in this section:
- [Overview](https://docs.expo.dev/tutorial/overview.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Overview of Expo and EAS tutorials

The **Learn** section is a collection of tutorials and other guides that help you learn about Expo and Expo Application Services (EAS). It covers the following:

[Expo tutorial](/tutorial/introduction.md) — If you are new to Expo and want to learn to write the code yourself, we recommend starting with this tutorial. It provides a step-by-step guide on how to build an Expo app that runs on Android, iOS, and web.

[Build with AI tutorial](/tutorial/build-with-ai/introduction.md) — Build your first app by directing an AI coding agent, with no programming experience required. It covers setting up your tools from scratch and verifying the app on your phone at every step.

[EAS tutorial](/tutorial/eas/introduction.md) — If you are looking to learn about building your Android and iOS apps using Expo Application Services (EAS), this tutorial covers the EAS Build, Update, and Submit workflows.

[CI/CD tutorial](/tutorial/cicd/introduction.md) — Automate building, testing, and releasing your Expo app with EAS Workflows. This tutorial covers development, preview, and production pipelines, from your first workflow file to tag-based releases and web deployments.
