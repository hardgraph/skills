> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Define a QA cohort

> Define a Retell AI QA cohort: pick agents, date range, and call filters, then set a sampling rate to control evaluation volume and cost.

A cohort defines which calls AI QA evaluates and how many of them to sample. Configure it in the Create QA flow, then move on to [resolution criteria](/ai-qa/define-resolution-criteria).

<Frame caption="The cohort creation form, where you name the cohort, filter calls, and set sampling.">
  <img height="700" src="https://mintcdn.com/retellai/6wkwFDKypdjHQ-22/ai-qa/images/create-cohort.png?fit=max&auto=format&n=6wkwFDKypdjHQ-22&q=85&s=b9bea42f1fd99e208d4ca79ae7eec9d9" alt="QA cohort creation form with fields for cohort name, agent and call filters, and sampling percentage" data-path="ai-qa/images/create-cohort.png" />
</Frame>

<Steps>
  <Step title="Name the cohort">
    Give the cohort a unique name that indicates its purpose or filters, so it's easy to find on the AI QA dashboard — for example, `High-value customers Q4` or `Support calls - week 1`.
  </Step>

  <Step title="Filter calls by agent and criteria">
    Choose which calls to include based on the following filters:

    * **Agents** — Select one or more agents whose calls you want to analyze. Use this to focus on a single agent or compare performance across several.
    * **Date range** — The **start date** is required. The **end date** is optional; leave it blank to create a dynamic cohort that keeps adding new matching calls as they occur.
    * **Call duration** — Include or exclude calls by length. Filter out very short calls (for example, under 30 seconds) that carry little signal, or focus on longer calls that need more analysis.
    * **Disconnection reason** — Filter by [disconnection reason](/reliability/debug-call-disconnect).
    * **Post-call analysis** — Add custom filters based on your [post-call analysis](/features/post-call-analysis-overview) results.
  </Step>

  <Step title="Set the sampling percentage">
    Sampling controls how many of the filtered calls are actually analyzed, so you can manage volume and cost.

    * **Percentage** — The share of matching calls to include. Setting 50% analyzes half of all calls that match your filters.
    * **Weekly max** — A cap on how many calls are analyzed per week. Setting it to 100 keeps the cohort under 100 calls a week even if the percentage would allow more.

    <Note>
      The weekly max is a ceiling. If the percentage yields fewer calls than the max, the percentage applies; if it yields more, the max applies.
    </Note>
  </Step>

  <Step title="Continue">
    Once your filters and sampling are set, click **Next** to move on to [resolution criteria](/ai-qa/define-resolution-criteria).
  </Step>
</Steps>
