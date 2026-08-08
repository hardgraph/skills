> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# View QA results

> Read your Retell AI QA results: review aggregate metrics and trends, drill into individual calls, inspect transcripts with QA diagnostics, and calibrate scores.

After you [create a cohort](/ai-qa/create-cohort) and run analysis, the QA results give you two views: **Call QA Overview** for high-level trends and **Detailed Calls** for individual call analysis. From either, you can drill into a single call to review its diagnostics, errors, and transcript.

The dashboard shows summary metrics across all analyzed calls, trend charts over time, individual call records with scores, and call-level QA sheets with transcript analysis. For a definition of any metric named below, see [AI QA metrics](/ai-qa/terminologies).

## Call QA Overview tab

The **Call QA Overview** tab gives a high-level view of the cohort's performance through summary metrics and trend charts.

### Summary metrics

The top section shows key metrics in a grid:

* **Calls Analyzed** — Total calls analyzed in the cohort
* **Average Score** — Overall quality score based on your resolution criteria
* **Call Resolution Rate** — Percentage of calls successfully resolved
* **Transfer Success Rate** — Percentage of calls transferred successfully to another agent or system
* **Transfer Wait Time** — Average time users wait before a transfer completes
* **Average Latency** — Mean response time across all calls
* **LLM Hallucination Rate** — Percentage of calls with AI-generated inaccuracies
* **KB (Knowledge Base) Recall** — How effectively the knowledge base was retrieved
* **Negative Sentiment Rate** — Percentage of interactions with negative sentiment
* **WER (Word Error Rate)** — Transcription accuracy
* **Avg. Overlapping Speech** — Average overlapping-speech instances per call
* **Tool Call Accuracy** — Rate of correct tool invocations
* **Transition Accuracy** — Accuracy of conversation flow transitions
* **Agent Natural Tonality Rate** — Percentage of natural-sounding agent speech
* **Agent Positive Sentiment Rate** — Percentage of agent responses with positive sentiment
* **Avg Custom Tool Latency** — Average time custom tools take to return
* **Custom Tool Success Rate** — Percentage of custom tool calls that completed successfully

<Frame caption="Trend charts show how each metric moves over time.">
  <img src="https://mintcdn.com/retellai/6wkwFDKypdjHQ-22/ai-qa/images/charts.png?fit=max&auto=format&n=6wkwFDKypdjHQ-22&q=85&s=150ace9eac22601499bc40d0b73668c2" alt="Trend charts for average score, resolution rate, and other performance metrics over a date range" width="2872" height="1558" data-path="ai-qa/images/charts.png" />
</Frame>

### Top questions from users

A table shows the questions callers asked most often, alongside each one's resolution rate.

<Frame caption="Top user questions ranked with their resolution rates.">
  <img src="https://mintcdn.com/retellai/6wkwFDKypdjHQ-22/ai-qa/images/top-questions.png?fit=max&auto=format&n=6wkwFDKypdjHQ-22&q=85&s=56635bf4509e8001e8f94db9fdd64bc0" alt="Top questions from users table listing questions with resolution rate and resolved-over-total counts" width="2878" height="600" data-path="ai-qa/images/top-questions.png" />
</Frame>

AI QA groups similar questions into one row — for example, "What are your office hours?" and "What time do you open?" are counted together.

## Detailed Calls tab

The **Detailed Calls** tab is a table of every analyzed call, with sortable columns and per-call metrics.

<Frame caption="The Detailed Calls table, one row per analyzed call.">
  <img src="https://mintcdn.com/retellai/6wkwFDKypdjHQ-22/ai-qa/images/details-tab.png?fit=max&auto=format&n=6wkwFDKypdjHQ-22&q=85&s=46e60b2f866bd3b6aeabd31ed5d3b1cd" alt="Detailed calls table with sortable columns for score, hallucination rate, KB recall, latency, and more" width="2870" height="1832" data-path="ai-qa/images/details-tab.png" />
</Frame>

### Calls table

Each row shows: Call ID, Evaluation Result, Call Start Time, Call Length, LLM Hallucination Rate, KB Recall, Transition Accuracy, User Positive Sentiment Rate, Latency P50, Overlapping Speech Count, WER, Tool Call Accuracy, and Natural Tonality Rate.

### Sorting and filtering

* **Sort** — Click any column header to sort by that metric.
* **Filter** — Use the Filter button to apply date ranges, score thresholds, and more.

<Info>
  Use the ellipsis menu (⋯) in the Action column to rerun QA for a call or delete a call from QA.
</Info>

## Call-level QA sheet

Click any row in the Detailed Calls table to open its **Call QA Sheet**, which shows the full diagnostics for that call.

### QA result overview

The sheet shows:

* **Overall Score** — Pass/fail status with a numerical score
* **Passed metrics** — Metrics that met their thresholds (green checkmarks)
* **Failed metrics** — Metrics that missed their thresholds (orange warning triangles)

For step-by-step fixes when a metric fails, see [Address metric issues](/ai-qa/address-metric-issues).

<Frame caption="A call QA sheet listing passed and failed metrics with notes.">
  <img src="https://mintcdn.com/retellai/6wkwFDKypdjHQ-22/ai-qa/images/call-qa.png?fit=max&auto=format&n=6wkwFDKypdjHQ-22&q=85&s=8c59e7720114e405de1e67a5d17b9e9d" alt="Call QA sheet showing overall score, passed metrics with green checkmarks, failed metrics with warning triangles, and notes" width="1014" height="1750" data-path="ai-qa/images/call-qa.png" />
</Frame>

### Calibrate a call

You can override an individual call's evaluation by [calibrating](/ai-qa/terminologies#calibration) it:

* **Mark a passed metric as failed** if it should have failed
* **Mark a failed metric as passed** if it should have passed

You can also add custom notes to any call QA. Calibration changes the per-call score only; it doesn't change the criteria for future calls.

### Transcript and errors

The sheet also gives you the full call transcript, specific transcription errors with corrections, and highlights marking where errors occurred.

<Frame caption="Transcript errors shown inline with their corrections.">
  <img src="https://mintcdn.com/retellai/4PGGkmtoXvaHaFi1/ai-qa/images/transcript-error.png?fit=max&auto=format&n=4PGGkmtoXvaHaFi1&q=85&s=9c209695c3b92c5ffbdb2096ec00e005" alt="Transcript view highlighting mistranscribed words alongside their corrected text" width="1982" height="550" data-path="ai-qa/images/transcript-error.png" />
</Frame>

<Warning>
  Low scores or high failure rates can point to systemic issues in your agent configuration, prompts, or knowledge base. Review several failed calls to find the pattern before making changes.
</Warning>
