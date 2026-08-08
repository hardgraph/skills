> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Alert rules for call and chat metrics

> Create Retell AI alert rules that email or webhook you when call or chat volume, success rate, cost, latency, or API errors cross a threshold you set.

Alerting watches your call and chat metrics and notifies you the moment one crosses a threshold you set, so you catch a volume spike, a drop in success rate, or a wave of API errors without watching a dashboard. You define rules; Retell evaluates them on a schedule and sends an email or webhook when a rule triggers.

<Frame caption="The Alerting page lists your saved rules on the Alerts tab.">
  <div style={{ aspectRatio: '16 / 9', display: 'flex', alignItems: 'center', justifyContent: 'center', width: '100%' }}>
    <img src="https://mintcdn.com/retellai/GWDpUspWhecmEo9X/images/alerting/rules-list.png?fit=max&auto=format&n=GWDpUspWhecmEo9X&q=85&s=62670e9d951df73617d1fb86d019acb2" alt="Alerting page in the Retell dashboard showing the Alerts tab with three saved alert rules, each listing its metric and filters, and a Create Alert button in the top right corner." style={{ maxWidth: '100%', maxHeight: '100%', objectFit: 'contain' }} width="2508" height="1327" data-path="images/alerting/rules-list.png" />
  </div>
</Frame>

## When to use it

Reach for alerting when you need to catch a change in your call or chat operations fast, without a person watching for it. Common cases:

* **Catch outages early.** Alert when call or chat success rate drops or API errors climb, so you react before customers complain.
* **Detect volume anomalies.** Alert on an unexpected spike or drop in call or chat volume, or when concurrency approaches your limit.
* **Control spend.** Alert when total call or chat cost over a window exceeds a budget.
* **Watch quality.** Alert on rising negative sentiment or a growing count of calls that fail QA.

Alerting evaluates *aggregate* metrics over a time window. It isn't built for reacting to a single call or chat; for per-session automation, send a [webhook](/features/webhook-overview) from the agent instead. To explore the same metrics interactively, use the [analytics dashboard](/features/analytics-dashboard).

<Note>
  Alerting is a dashboard feature. Rules are created and managed in the dashboard, not through the public API.
</Note>

### Example

A support team wants to know the instant call quality slips. They create a rule on **Call success rate**, set it to trigger when the rate **is below 90%** over the last **1 hour**, checked every **5 minutes**, and add a webhook that posts to their on-call Slack channel. When success rate dips, the on-call engineer is paged within minutes instead of hearing about it from customers.

## Create an alert rule

Open the **Alerting** tab in the dashboard and select **Create Alert**.

<Frame caption="The Create Alert dialog, configuring a rule on call volume.">
  <div style={{ aspectRatio: '16 / 9', display: 'flex', alignItems: 'center', justifyContent: 'center', width: '100%' }}>
    <img src="https://mintcdn.com/retellai/ksPrWbndKPAvsxM-/images/alerting/create-rule.png?fit=max&auto=format&n=ksPrWbndKPAvsxM-&q=85&s=114aeb20d61e9feebdf61cf2166fbe52" alt="Create Alert dialog with an alert name field, a Time Configuration row reading 'Check every 5 min for the last 30 min', a metric-condition toggle between 'Compare to certain value' and 'Compare to last cycle', a metric dropdown set to Number of Calls with a 'when sum is above 2' condition, a Filter section with an agent picker defaulting to All Agents, and Notify via fields for email addresses and webhook URLs, each webhook URL having a Test button, with Cancel and Save buttons." style={{ maxWidth: '100%', maxHeight: '100%', objectFit: 'contain' }} width="1610" height="1520" data-path="images/alerting/create-rule.png" />
  </div>
</Frame>

<Steps>
  <Step title="Name the rule">
    Give it a descriptive name, such as `Call success rate drop`. The name appears in every notification.
  </Step>

  <Step title="Pick the metric and condition">
    Choose the [metric](#metrics-you-can-monitor) to watch; the picker groups them into **API**, **Call**, **Chat**, and **QA**. Then pick a [threshold type](#thresholds): **Compare to certain value** (absolute) or **Compare to last cycle** (relative). Choose the comparator (for example, *is above* or *is below*) and enter the threshold value.
  </Step>

  <Step title="Set the window and frequency">
    Under **Time Configuration**, set how often the rule runs (**Check every**, the frequency) and how far back each check looks (**for the last**, the window). See [Evaluation window and frequency](#evaluation-window-and-frequency) for the valid combinations.
  </Step>

  <Step title="Narrow with filters (optional)">
    Restrict the metric to specific agents, agent [environment tags](/agent/version#environment-tags), disconnection reasons, API error codes, a QA cohort, or post-call analysis fields. See [Filters](#filters).
  </Step>

  <Step title="Add notification channels">
    Add one or more email addresses, webhook URLs, or both. At least one is required. Use **Test** next to a webhook URL to send a sample payload and confirm your endpoint accepts it before saving.
  </Step>

  <Step title="Save">
    Select **Save**. The rule starts evaluating on its next scheduled check.
  </Step>
</Steps>

## Metrics you can monitor

The `metric_type` value is what appears in the webhook payload.

| Metric                       | `metric_type`                   | What it measures                                                              |
| ---------------------------- | ------------------------------- | ----------------------------------------------------------------------------- |
| Number of calls              | `call_count`                    | Count of calls that ended within the window.                                  |
| Concurrency used             | `concurrency_used`              | Peak number of concurrent calls during the window.                            |
| Call success rate            | `call_success_rate`             | Percentage of calls marked successful (0–100).                                |
| Negative sentiment rate      | `negative_sentiment_rate`       | Percentage of calls with negative user sentiment (0–100).                     |
| Custom function latency      | `custom_function_latency`       | Average response time of custom function calls, in milliseconds.              |
| Custom function failures     | `custom_function_failure_count` | Number of custom function calls that failed.                                  |
| Transfer call failures       | `transfer_call_failure_count`   | Number of call transfers that failed.                                         |
| QA not passed                | `qa_not_passed_count`           | Number of analyzed calls in a QA cohort that did not pass. Requires a cohort. |
| Total call cost              | `total_call_cost`               | Total cost of calls in the window, in USD.                                    |
| API error count              | `api_error_count`               | Number of API requests that returned an error status code.                    |
| Number of chats              | `chat_count`                    | Count of chats that ended within the window.                                  |
| Chat success rate            | `chat_success_rate`             | Percentage of chats marked successful (0–100).                                |
| Chat negative sentiment rate | `chat_negative_sentiment_rate`  | Percentage of chats with negative user sentiment (0–100).                     |
| Total chat cost              | `total_chat_cost`               | Total cost of chats in the window, in USD.                                    |

The metric decides which channel the rule watches. Call metrics aggregate over voice calls; chat metrics aggregate over [chat agent](/build/create-chat-agent) sessions, counting each chat when it ends, the same way call metrics count a call.

## Thresholds

Every rule uses one of two threshold types.

### Compare to certain value (absolute)

Compares the metric against a fixed number.

**Example:** trigger when `Number of calls` **is above** `100` in the last hour.

### Compare to last cycle (relative)

Compares the percentage change from the previous window against your threshold. Use it to catch sudden spikes or drops regardless of the baseline.

**Example:** trigger when `Number of calls` **increases by more than** `50%` compared to the previous hour.

The percentage change is calculated as:

```
((currentValue - previousValue) / previousValue) * 100
```

<Note>
  If the previous window had zero activity and the current window has activity, the change is treated as an infinite increase and shown as `+∞%`. Rules that trigger on an increase (*increases by more than* or *increases by at least*) fire in this case.
</Note>

## Evaluation window and frequency

The **window** is how far back each check looks when aggregating the metric. The **frequency** is how often the rule runs. A shorter window can only be paired with faster frequencies:

| Window     | Supported frequencies         |
| ---------- | ----------------------------- |
| 1 minute   | 1 minute                      |
| 5 minutes  | 1 minute, 5 minutes           |
| 30 minutes | 5 minutes, 30 minutes         |
| 1 hour     | 5 minutes, 30 minutes, 1 hour |
| 12 hours   | 30 minutes, 1 hour, 12 hours  |
| 24 hours   | 1 hour, 12 hours, 24 hours    |
| 3 days     | 12 hours, 24 hours            |
| 7 days     | 24 hours                      |

<Tip>
  Pick a frequency that balances responsiveness against noise. Checking every minute catches issues fast but is more likely to trigger on a brief spike.
</Tip>

## Filters

Filters narrow what data feeds the metric. Which filters are available depends on the metric.

| Filter               | Applies to                                                              | What it does                                                                                                                                                                                                                                                                 |
| -------------------- | ----------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Agents               | All metrics except Concurrency used, API error count, and QA not passed | Restrict the metric to specific agents, and optionally specific agent versions. The picker matches the metric's channel: chat metrics list chat agents, call metrics list voice agents.                                                                                      |
| Environment tags     | Same metrics as the Agents filter                                       | Restrict the metric to calls or chats handled by versions carrying the selected [environment tags](/agent/version#environment-tags), such as `prod` or `staging`. Select tags in the **Tags** section of the same agent picker; a tag applies across all agents that use it. |
| Disconnection reason | Number of calls                                                         | Count only calls that ended for the selected reasons (for example, User Hangup or Dial No Answer).                                                                                                                                                                           |
| API error code       | API error count                                                         | Count only requests that returned the selected HTTP status codes: `400`, `401`, `402`, `403`, `404`, `409`, `422`, `429`, `500`.                                                                                                                                             |
| QA cohort            | QA not passed                                                           | Choose which [QA cohort](/ai-qa/create-cohort) to watch. Required for this metric.                                                                                                                                                                                           |
| Post call analysis   | Same metrics as the Agents filter                                       | Filter calls or chats by [post-call analysis](/features/post-call-analysis-overview) fields, such as a boolean field `appointment_booked` set to `false`.                                                                                                                    |

## Notifications

When a rule triggers, Retell notifies every channel you configured. When a later check finds the condition no longer met, the incident resolves and the same channels are notified again.

### Email

Each recipient gets an email whose subject is `[Retell Alert] <rule name>`. The body includes the workspace, the metric, the value that triggered the alert, the threshold that was breached, the trigger time, and a link back to the dashboard. For most metrics it also links to the call history, chat history, or QA results behind the incident; Concurrency used and API error count have no drill-down. When the incident resolves, a follow-up email with subject `[Retell Alert Resolved] <rule name>` confirms it.

### Webhook

Each webhook URL receives a `POST` request. The body wraps the incident under an `alert` object:

```json theme={"dark"}
{
  "event": "alert_triggered",
  "alert": {
    "alert_incident_id": "alert_incident_0b1c2d3e4f5g6h7i8j9k0l1m",
    "org_id": "org_abc123",
    "alert_rule_id": "alert_rule_xyz789abcdef012345678901",
    "name": "High call volume",
    "metric_type": "call_count",
    "filter": {
      "agent": [{ "agent_id": "agent_abc123", "version": [3] }]
    },
    "threshold_type": "absolute",
    "threshold_value": 100,
    "comparator": "gt",
    "frequency": "5m",
    "window": "1h",
    "emails": ["oncall@yourcompany.com"],
    "webhook_urls": ["https://yourcompany.com/hooks/retell-alerts"],
    "current_value": 148,
    "previous_value": null,
    "triggered_timestamp": 1714608475945,
    "resolved_timestamp": null
  }
}
```

Key fields:

* `event` is `alert_triggered` when an incident opens. When it resolves, the same URLs receive a second request with `alert_resolved`.
* `filter` echoes the filters set on the rule. An environment-tag filter appears as `agent_tag`, for example `"agent_tag": { "type": "enum", "op": "in", "value": ["prod", "staging"] }`.
* `comparator` is one of `gt`, `lt`, `ge`, `le` (the symbol forms `>`, `<`, `>=`, `<=` are also accepted).
* `current_value` is the aggregated value that triggered the rule. `previous_value` is populated only for relative thresholds.
* For `total_call_cost` and `total_chat_cost`, `current_value`, `previous_value`, and `threshold_value` are in **cents** (the dashboard displays them as dollars).
* `triggered_timestamp` and `resolved_timestamp` are Unix timestamps in milliseconds. `resolved_timestamp` is `null` while the incident is active.

Each request carries an `X-Retell-Signature` header. Verify it before trusting the payload. Alert webhooks use the same HMAC-SHA256 scheme as other Retell webhooks, so the [webhook verification steps and code](/features/secure-webhook) apply here too. Retell sends each request once and waits up to **10 seconds** for a response; it does not retry, so acknowledge quickly and do heavy work asynchronously.

<Warning>
  Always verify the webhook signature in production so you only act on requests that genuinely come from Retell.
</Warning>

## Alert history

Each time a rule's condition is met, Retell opens an **incident** and sends notifications. Incidents are listed under the **Alert History** tab, with the active period, the rule name, the condition, the value that triggered it, and the channels notified.

<Frame caption="The Alert History tab lists past incidents and the value that triggered each one.">
  <div style={{ aspectRatio: '16 / 9', display: 'flex', alignItems: 'center', justifyContent: 'center', width: '100%' }}>
    <img src="https://mintcdn.com/retellai/GWDpUspWhecmEo9X/images/alerting/incidents.png?fit=max&auto=format&n=GWDpUspWhecmEo9X&q=85&s=410cb741e4a2ed4bfa1557e6ad77caef" alt="Alert History tab listing past alert incidents in a table with columns for active period, alert name, condition, triggered value (for example '0 → 2 (+Infinity%)'), and notification channel." style={{ maxWidth: '100%', maxHeight: '100%', objectFit: 'contain' }} width="2509" height="1328" data-path="images/alerting/incidents.png" />
  </div>
</Frame>

Only one incident stays active per rule at a time. A new incident opens only after the previous one resolves.

## Limits

As of July 2026:

* Up to **10 alert rules** per workspace. Creating an 11th returns a `400` error.
* Up to **100 agents** in a single rule's agent filter.

## FAQ

<AccordionGroup>
  <Accordion title="How often are alerts evaluated?">
    The system checks for due rules every minute and runs each one on its own configured frequency (1 minute to 24 hours). A rule with a 5-minute frequency is evaluated roughly every 5 minutes, not every minute.
  </Accordion>

  <Accordion title="Will I get repeated notifications for the same issue?">
    No. A notification is sent once when a new incident opens, and once more when it resolves. You won't get repeat notifications while the condition persists.
  </Accordion>

  <Accordion title="What happens when I edit a rule?">
    Editing a rule resets its evaluation schedule and clears any active incident, so it starts evaluating fresh on the next check. This prevents a stale incident from lingering after you've changed the condition.
  </Accordion>

  <Accordion title="Can I alert across multiple agents at once?">
    Yes. Select multiple agents in the Agents filter and the metric is aggregated across all of them. Leave the filter empty to include every agent of the metric's channel. A single rule can't mix voice and chat agents; create one rule per channel instead.
  </Accordion>

  <Accordion title="Can I alert on chat agents?">
    Yes. Pick one of the chat metrics (Number of chats, Chat success rate, Chat negative sentiment rate, or Total chat cost) and the rule evaluates chats instead of calls. Filters work the same way: narrow by chat agent, environment tag, or post-call analysis fields.
  </Accordion>
</AccordionGroup>
