---
modificationDate: June 22, 2026
title: Maestro insights
description: Track flow health, flaky flows, and failure patterns for your Maestro end-to-end tests in the Maestro tab of EAS Insights.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas-insights/maestro/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas-insights/maestro/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Insights
Pages in this section:
- [Introduction](https://docs.expo.dev/eas-insights/introduction.md)
- [App usage](https://docs.expo.dev/eas-insights/app-usage.md)
- [EAS Workflows](https://docs.expo.dev/eas-insights/workflows.md)
- [Maestro](https://docs.expo.dev/eas-insights/maestro.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Maestro insights

Track flow health, flaky flows, and failure patterns for your Maestro end-to-end tests in the Maestro tab of EAS Insights.

> Maestro insights is available on the Production and Enterprise plans. Learn more at [EAS pricing](https://expo.dev/pricing).

The **Maestro** tab in the dashboard surfaces results from the [Maestro](https://maestro.dev/) end-to-end (E2E) tests you run in EAS Workflows with the [`maestro` job](/eas/workflows/pre-packaged-jobs.md#maestro). It helps you spot which flows fail or flake the most so you can keep your test suite healthy.

-   To view insights, open your project in the EAS dashboard, select **Insights** from the navigation menu, and then select the **Maestro** tab.
-   To start sending data, set up E2E tests by following [Run E2E tests on EAS Workflows with Maestro](/eas/workflows/examples/e2e-tests.md). Results appear automatically as your tests run.

> Insights are collected from the JUnit test report, which the `maestro` job produces by default (`output_format: junit`). If you set `output_format` to another value such as `html`, results aren't reported and won't appear here.

Each flow run falls into one of three states:

-   **Passed**: Passed on the first attempt, with no retries.
-   **Flaky**: Passed, but only after one or more retries.
-   **Failed**: Did not pass.

## Overview

The top of the tab summarizes activity for the selected time range, with each metric compared against the previous period of equal length:

-   **Maestro runs**: How many flow runs completed.
-   **Pass rate**: The share of runs that passed (a flaky run still counts as a pass).
-   **Flaky flows**: How many distinct flows flaked at least once.
-   **Avg duration**: Average run time.

## Runs over time

A chart breaks runs down by day (or by hour or minute for shorter time ranges) into passed, flaky, and failed.

## All Maestro flows

A table lists each flow with its **Runs**, **Pass rate**, **Fails**, **Flake rate**, **P90** (90th percentile run duration), and **Last run**. Sort by any column to find the flows that need attention, and search by flow path.

## Filters

Narrow the data down by workflow, status (passed, flaky, failed), tag, or branch.

## Flow details

Select a flow to drill into its history, including its Maestro runs, pass rate, flaky runs, and P90 duration, plus:

-   **Error patterns**: Failures grouped by error message, with a sample message and how often each pattern occurred.
-   **Recent runs**: Individual runs with their status, duration, whether they flaked, and the workflow run, branch, and commit they came from.

## More

[Run E2E tests on EAS Workflows with Maestro](/eas/workflows/examples/e2e-tests.md) — Set up and run Maestro end-to-end tests in EAS Workflows.

[Maestro documentation](https://docs.maestro.dev/) — Learn more about Maestro flows and how to write them.
