---
modificationDate: June 22, 2026
title: EAS Workflows insights
description: Track run counts, success rates, and trends for your EAS Workflows in the Workflows tab of EAS Insights.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas-insights/workflows/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas-insights/workflows/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Insights
Pages in this section:
- [Introduction](https://docs.expo.dev/eas-insights/introduction.md)
- [App usage](https://docs.expo.dev/eas-insights/app-usage.md)
- [EAS Workflows](https://docs.expo.dev/eas-insights/workflows.md) (this page)
- [Maestro](https://docs.expo.dev/eas-insights/maestro.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# EAS Workflows insights

Track run counts, success rates, and trends for your EAS Workflows in the Workflows tab of EAS Insights.

> Workflows insights is available on the Production and Enterprise plans. Learn more at [EAS pricing](https://expo.dev/pricing).

The **Workflows** tab in the dashboard shows how your [EAS Workflows](/eas/workflows/introduction.md) are doing over time: how often they run, how often they succeed, and which workflows fail most. Data appears automatically as your workflows run.

-   To view these insights, open your project in the EAS dashboard, select **Insights** from the navigation menu, and then select the **Workflows** tab.
-   Choose a time range to load the data. Every metric also compares against the previous period of equal length so you can see whether a number is trending up or down.

## Overview

The top of the tab summarizes activity for the selected time range:

-   **Total runs**: How many workflow runs started.
-   **Success rate**: The share of runs that succeeded.
-   **Active workflows**: How many distinct workflows ran.
-   **Failed runs**: How many runs failed.

## Runs over time

A chart breaks runs down by day (or by hour or minute for shorter time ranges) into total, successful, failed, and canceled runs. Toggle between a line view and a stacked view, and turn individual statuses on or off to focus on what you care about.

## Workflows table

Below the chart, a table lists each workflow that ran in the time range with:

-   **Runs**: The total count, broken down into succeeded, failed, and canceled.
-   **Success rate**: The share of runs that succeeded.
-   **Last run**: When the workflow last ran.

## Filters

Narrow the data down by workflow, run status (success, failure, canceled), trigger type (manual, schedule, or a GitHub event), or git ref. You can also search by workflow name.

## Export

Export the workflow runs that match your current filters and time range as CSV or newline-delimited JSON (NDJSON) for further analysis.

> Insights are aggregated for trend analysis and can lag behind real time or omit recent runs. Use exports to investigate trends rather than as an authoritative record for billing or auditing.

## More

[Introduction to EAS Workflows](/eas/workflows/introduction.md) — Learn how to automate builds, updates, submissions, and tests with EAS Workflows.
