> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Post-call analysis overview and categories

> Retell post-call analysis automatically scores and structures customer calls after they end — built-in categories plus custom analysis for your workflow.

Post-call analysis automatically analyzes customer conversations after they end, so you can extract insights from your calls. We provide several built-in analysis categories, and you can create custom categories to match your business needs. To follow or step into calls while they're still in progress, see [Live Monitoring](/features/live-monitoring).

<Frame>
  <img height="200" src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/post-call-analysis-dashboard.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=c5ba16aa27f7b48edff11d06e6928d31" alt="Post-call analysis dashboard showing various analytics categories and metrics" data-path="images/post-call-analysis-dashboard.png" />
</Frame>

<Note>We will not populate custom post-call analysis fields for calls that were not connected or where no conversation took place. Please check whether the field exists before using it.</Note>

## Analysis Categories

You can extract the following types of data from post-call analysis:

* **Boolean** (True/False)
  * Simple yes/no determinations
  * Example: Whether the customer is a first-time caller

* **Text** (String)
  * Detailed textual information
  * Example: Call summaries, action items, or key discussion points

* **Number** (Numerical value)
  * Quantitative measurements
  * Example: Transaction amounts, call duration, or satisfaction scores

* **Selector** (Enum)
  * Categorization from a fixed list
  * Example: Issue types, product categories, or resolution status

## Related

To score call quality across a sampled set of calls — hallucinations, resolution rate, latency, and more — see [AI Quality Assurance](/ai-qa/overview). You can also use post-call analysis fields as filters when [defining a QA cohort](/ai-qa/create-cohort).
