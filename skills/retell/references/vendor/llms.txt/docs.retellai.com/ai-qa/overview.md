> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# AI Quality Assurance

> AI QA scores Retell calls on hallucination, knowledge base accuracy, latency, sentiment, and tool usage to surface quality trends and issues.

AI QA automatically evaluates a sampled set of your calls against rules and metrics you configure. It surfaces high-level trends — average score, resolution rate, latency — and call-level diagnostics like hallucinations, knowledge base accuracy, overlapping speech, sentiment, and tool usage, each backed by transcript evidence.

<Frame caption="The Call QA Overview shows summary metrics and trends across a cohort of analyzed calls.">
  <img src="https://mintcdn.com/retellai/6wkwFDKypdjHQ-22/ai-qa/images/template.png?fit=max&auto=format&n=6wkwFDKypdjHQ-22&q=85&s=6734875f09f5ea8461668b42fd663055" alt="Call QA Overview dashboard showing calls analyzed, average score of 87, call resolution rate of 75%, transfer success rate, transfer wait time, and a top questions from users table with resolution rates." width="1483" height="1168" data-path="ai-qa/images/template.png" />
</Frame>

## Use AI QA to

* Track call quality and resolution over time
* Identify failure patterns and their root causes
* Review individual calls with transcript-level evidence

For example, a health clinic runs AI QA on its appointment-booking agent to catch when the agent gives wrong hours or mishears a caller's name, then uses the [top questions](/ai-qa/view-qa-results#top-questions-from-users) view to see which caller intents resolve least often.

## How it works

<Steps>
  <Step title="Create a cohort">
    Group the calls you want to evaluate by agent, date range, and other filters, and set how many to sample. See [Define a QA cohort](/ai-qa/create-cohort).
  </Step>

  <Step title="Define resolution criteria">
    Set the AI conditions and metric thresholds that decide whether a call counts as successful. See [Define resolution criteria](/ai-qa/define-resolution-criteria).
  </Step>

  <Step title="Review results">
    Read aggregate trends, drill into individual calls, and act on flagged issues. See [View QA results](/ai-qa/view-qa-results) and [Address metric issues](/ai-qa/address-metric-issues).
  </Step>
</Steps>

New here? Start with [Access AI QA](/ai-qa/get-started). For definitions of every metric and term, see [AI QA metrics](/ai-qa/terminologies).

## Pricing

AI QA is free for the first 100 minutes of analyzed call time per workspace. After that, it's priced at \$0.10 per minute of analyzed call time.
