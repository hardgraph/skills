---
modificationDate: May 24, 2021
title: Expo Structured Field Values
description: Version 0
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/technical-specs/expo-sfv-0/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/technical-specs/expo-sfv-0/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Reference > Technical specs
Pages in this section:
- [Expo Updates v1](https://docs.expo.dev/technical-specs/expo-updates-1.md)
- [Expo Structured Field Values](https://docs.expo.dev/technical-specs/expo-sfv-0.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo Structured Field Values

Version 0

Structured Field Values for HTTP, [IETF RFC 8941](https://tools.ietf.org/html/rfc8941), is a proposal to formalize header syntax and facilitate nested data.

Since it is still a work in progress, Expo maintains a custom version that only implements the following subset of the protocol defined in [IETF RFC 8941](https://tools.ietf.org/html/rfc8941):

-   All key values
-   String, integer, and decimal items
-   Dictionaries
