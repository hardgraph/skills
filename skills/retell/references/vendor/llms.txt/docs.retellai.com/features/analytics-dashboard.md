> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Analytics dashboard

> Build custom Retell analytics dashboards to track call and chat metrics like success rate, latency, cost, and concurrency, with charts, filters, and breakdowns.

The Analytics dashboard charts your call and chat data so you can see how your agents are performing over time. Open it from the **Analytics** tab, build the charts you care about, then filter and break them down without leaving the page.

<Frame caption="A call dashboard with number, line, and donut charts.">
  <div style={{ aspectRatio: '16 / 9', display: 'flex', alignItems: 'center', justifyContent: 'center', width: '100%' }}>
    <img src="https://mintcdn.com/retellai/2HwuM2BExfFOy7pJ/images/analytics/dashboard-overview.png?fit=max&auto=format&n=2HwuM2BExfFOy7pJ&q=85&s=2d27d95f915313e9b2d3ef02b807860a" alt="Analytics page in the Retell dashboard with Call Dashboard and Chat Dashboard tabs, the date range picker, Filter and Breakdown controls, and an Add Chart button, above charts for call counts, call duration, call latency, concurrency used, call successful, disconnection reason, user sentiment, and phone inbound versus outbound." style={{ maxWidth: '100%', maxHeight: '100%', objectFit: 'contain' }} width="1600" height="900" data-path="images/analytics/dashboard-overview.png" />
  </div>
</Frame>

## When to use it

Reach for the analytics dashboard when you want to see a pattern across many sessions rather than the detail of one:

* **Track performance over time.** Watch success rate, duration, or latency by day or week and see whether a prompt change moved them.
* **Compare agents or versions.** Break a metric down by agent or agent version to see which one performs better.
* **Watch spend.** Chart combined cost by agent or by day to see where your money goes.
* **Report on business outcomes.** Chart any [custom post-call analysis](/features/post-call-analysis-create) field, so "booked appointments" sits next to call volume.

It isn't the right tool for everything. To inspect one session's transcript or recording, use [call and chat history](/features/session-history). To be told when a metric crosses a line instead of checking for yourself, set up [alerting](/features/alerting-overview). To follow a call that's still running, use [live monitoring](/features/live-monitoring).

### Example

A clinic runs an appointment-reminder agent. They chart **Call Successful Rate** by day, break it down by agent version, and add a **Call Counts** chart filtered to the `voicemail_reached` disconnection reason. When a new version ships, the two charts show within a day whether more calls are landing in voicemail and whether the success rate followed.

## Create a dashboard

Each dashboard is scoped to either call data or chat data, and the scope is fixed once you create it. You can keep up to **10 dashboards** per workspace; creating an 11th fails with "Dashboard limit reached."

<Steps>
  <Step title="Add the dashboard">
    Open the overflow menu next to **Add Chart**, choose **Add Dashboard**, then pick **Call** or **Chat**.

    <Frame caption="Add Dashboard, where the call or chat scope is set for good.">
      <div style={{ aspectRatio: '16 / 9', display: 'flex', alignItems: 'center', justifyContent: 'center', width: '100%' }}>
        <img src="https://mintcdn.com/retellai/2HwuM2BExfFOy7pJ/images/analytics/add-dashboard.png?fit=max&auto=format&n=2HwuM2BExfFOy7pJ&q=85&s=d48468c06c73da653474e9d12867888c" alt="Dashboard overflow menu open with Duplicate and Delete above Add Dashboard, whose submenu offers Call and Chat." style={{ maxWidth: '100%', maxHeight: '100%', objectFit: 'contain' }} width="1600" height="900" data-path="images/analytics/add-dashboard.png" />
      </div>
    </Frame>
  </Step>

  <Step title="Name it">
    Select the tab name and type a new one. Names are how you'll tell tabs apart, so prefer "Outbound sales" over "Dashboard 2".
  </Step>

  <Step title="Arrange the tabs">
    Drag tabs to reorder them. **Duplicate** in the overflow menu copies a dashboard with all of its charts, which is the quickest way to build a variant. **Delete** appears once you have more than one dashboard.
  </Step>
</Steps>

## Add a chart

<Steps>
  <Step title="Open the chart editor">
    Select **Add Chart**.
  </Step>

  <Step title="Choose the graph type">
    Set **Graph Type** to column, bar, line, donut, or number. Line suits trends over time; number suits a single headline figure.
  </Step>

  <Step title="Pick the metric and measurement">
    Choose a [metric](#metrics-you-can-chart) under **Call Source Metrics**, then how to measure it. The measurement list changes with the metric: counts offer only **count**, while durations and costs offer **avg**, **median**, **p90**, **sum**, **min**, and **max**. Use **+ Add** to plot more than one metric on the same chart.
  </Step>

  <Step title="Set the time range and grouping">
    Pick the chart's own range, or leave it on **Follow dashboard time range** so it moves with the picker at the top. The **Hour / Day / Week / Month** toggle sets the time bucket. Tick **Previous period comparison** to overlay the preceding window.
  </Step>

  <Step title="Narrow it (optional)">
    Add filters and up to five breakdowns that apply to this chart only. See [Filters and breakdowns](#filters-and-breakdowns).
  </Step>

  <Step title="Save">
    Save the chart. It lands at the end of the dashboard, where you can drag it into place.
  </Step>
</Steps>

<Frame caption="The chart editor, with a live preview of the chart you're building.">
  <img src="https://mintcdn.com/retellai/2HwuM2BExfFOy7pJ/images/analytics/chart-editor.png?fit=max&auto=format&n=2HwuM2BExfFOy7pJ&q=85&s=fce2c285ddc025ab7e3f70f5295b6450" alt="Chart editor showing Graph Type set to Column, Call Source Metrics set to Call Counts, Filter and Breakdown sections with Add buttons, a Last 4 weeks range with an Hour, Day, Week, Month toggle and a Previous period comparison checkbox, and a live preview of the resulting chart." width="1600" height="1037" data-path="images/analytics/chart-editor.png" />
</Frame>

## Metrics you can chart

Which metrics you get depends on whether the dashboard is scoped to calls or chats.

### Call metrics

| Metric                       | Measurements                    | What it covers                                                                      |
| ---------------------------- | ------------------------------- | ----------------------------------------------------------------------------------- |
| Call Counts                  | count                           | Number of calls.                                                                    |
| Call Successful/Unsuccessful | count                           | Calls split by whether analysis marked them successful.                             |
| Call Status                  | count                           | Calls split by status.                                                              |
| Phone Inbound/Outbound       | count                           | Calls split by direction.                                                           |
| Disconnection Reason         | count                           | Calls split by why they ended.                                                      |
| User Sentiment               | count                           | Calls split by the sentiment analysis assigned.                                     |
| Call Successful Rate         | avg                             | Share of calls marked successful.                                                   |
| Call Picked Up Rate          | avg                             | Share of calls answered.                                                            |
| Call Transfer Rate           | avg                             | Share of calls transferred.                                                         |
| Voicemail Rate               | avg                             | Share of calls that reached [voicemail](/build/handle-voicemail).                   |
| Call Duration                | avg, median, p90, sum, min, max | Length of calls.                                                                    |
| End to End Latency           | avg, median, p90, min, max      | Per-call p50 [latency](/reliability/check-actual-latency), aggregated across calls. |
| Concurrency Used             | max                             | Peak [concurrency](/deploy/concurrency) in the period.                              |
| Combined cost                | avg, sum, median, p90, min, max | Total cost of the call.                                                             |

### Chat metrics

| Metric                       | Measurements                    | What it covers                                          |
| ---------------------------- | ------------------------------- | ------------------------------------------------------- |
| Chat Counts                  | count                           | Number of chats.                                        |
| Chat Successful/Unsuccessful | count                           | Chats split by whether analysis marked them successful. |
| Chat Status                  | count                           | Chats split by status.                                  |
| User Sentiment               | count                           | Chats split by the sentiment analysis assigned.         |
| Chat Successful Rate         | avg                             | Share of chats marked successful.                       |
| Combined cost                | avg, sum, median, p90, min, max | Total cost of the chat.                                 |

## Set the time range

The date range picker at the top of the page sets the window for every chart that follows the dashboard. Pick a preset (**Today**, **Last 7 days**, **Last 4 weeks**, **Last 3 months**, **Week to date**, **Month to date**, **Year to date**, **All time**) or select start and end dates on the calendar, then **Apply**. Each end of the range carries its own time and timezone, so buckets line up with your working day. Future dates are disabled.

<Frame caption="The dashboard date range, with presets, a calendar, and per-end timezones.">
  <div style={{ aspectRatio: '16 / 9', display: 'flex', alignItems: 'center', justifyContent: 'center', width: '100%' }}>
    <img src="https://mintcdn.com/retellai/2HwuM2BExfFOy7pJ/images/analytics/date-range.png?fit=max&auto=format&n=2HwuM2BExfFOy7pJ&q=85&s=0b7e9af211209109d65791a636304f85" alt="Date range picker open on the analytics dashboard showing presets from Today to All time down the left, two month calendars with a selected range, and start and end time fields each with a UTC-07 timezone selector, above Cancel and Apply buttons." style={{ maxWidth: '100%', maxHeight: '100%', objectFit: 'contain' }} width="1600" height="900" data-path="images/analytics/date-range.png" />
  </div>
</Frame>

A chart can override that window with its own range: **Today**, **Last 1 week**, **Last 4 weeks**, **Last 3 months**, **Week to date**, **Month to date**, **Year to date**, or **All time**.

## Filters and breakdowns

Filters and breakdowns set in the header apply to every chart on the dashboard. Charts can add their own on top.

The filter list is split across four tabs. **Base** holds the built-in fields; **Post Call Analysis**, **Metadata**, and **Dynamic Variables** hold whatever your agents produce.

On a call dashboard, Base offers agent, call ID, batch call ID, type, duration, from number, to number, user sentiment, disconnection reason, call status, call successful, end-to-end latency, and combined cost. A chat dashboard offers agent, chat ID, chat status, chat successful, combined cost, and user sentiment.

<Frame caption="Filters, grouped into base fields and everything your agents produce.">
  <div style={{ aspectRatio: '16 / 9', display: 'flex', alignItems: 'center', justifyContent: 'center', width: '100%' }}>
    <img src="https://mintcdn.com/retellai/2HwuM2BExfFOy7pJ/images/analytics/filter.png?fit=max&auto=format&n=2HwuM2BExfFOy7pJ&q=85&s=f2f92afb29ad16d3b188f4a967414205" alt="Filter panel open on the analytics dashboard with tabs for Base, Post Call Analysis, Metadata, and Dynamic Variables, and base filters listed for agent, call ID, batch call ID, type, duration, from, to, user sentiment, disconnection reason, call status, call successful, and E2E latency." style={{ maxWidth: '100%', maxHeight: '100%', objectFit: 'contain' }} width="1600" height="900" data-path="images/analytics/filter.png" />
  </div>
</Frame>

**Breakdowns** group every chart by one or more dimensions: agent, agent version, disconnection reason, call status, call successful, call type, and post-call analysis fields. Chat dashboards offer agent, agent version, disconnection reason, chat status, chat successful, and post-chat analysis fields.

<Frame caption="Breakdowns split each chart by a dimension such as agent or version.">
  <div style={{ aspectRatio: '16 / 9', display: 'flex', alignItems: 'center', justifyContent: 'center', width: '100%' }}>
    <img src="https://mintcdn.com/retellai/2HwuM2BExfFOy7pJ/images/analytics/breakdown.png?fit=max&auto=format&n=2HwuM2BExfFOy7pJ&q=85&s=2487694a69a5748a40bbb0981a1753a9" alt="Breakdown menu open on the analytics dashboard listing Agent, Agent Version, Disconnection Reason, Call Status, Call Successful, Call Type, and Post Call Analysis Field." style={{ maxWidth: '100%', maxHeight: '100%', objectFit: 'contain' }} width="1600" height="900" data-path="images/analytics/breakdown.png" />
  </div>
</Frame>

Filter and breakdown changes aren't saved for you. When you change one, **Save** and **Cancel** replace the usual controls in the header: save to keep the state for next time, cancel to revert to the last saved state. Moving and resizing charts saves on its own.

<Frame caption="Changing a filter or breakdown puts the header into its unsaved state.">
  <div style={{ aspectRatio: '16 / 9', display: 'flex', alignItems: 'center', justifyContent: 'center', width: '100%' }}>
    <img src="https://mintcdn.com/retellai/2HwuM2BExfFOy7pJ/images/analytics/save-cancel.png?fit=max&auto=format&n=2HwuM2BExfFOy7pJ&q=85&s=675d27c85d7b3961a82addac1ae6d43f" alt="Analytics dashboard after adding an agent breakdown, with Cancel and Save buttons shown in the header where Add Chart normally sits." style={{ maxWidth: '100%', maxHeight: '100%', objectFit: 'contain' }} width="1600" height="900" data-path="images/analytics/save-cancel.png" />
  </div>
</Frame>

### Per-chart filters and breakdowns

Each chart can carry its own filters and up to five breakdowns, set in the chart editor. Chart filters are merged with the dashboard filters rather than replacing them, so both apply. Use them to narrow one chart without touching the rest of the dashboard.

Any field from your [custom post-call analysis](/features/post-call-analysis-create) can drive a chart, which is how you track business-specific outcomes next to the standard metrics.

## Chart types and layout

Five chart types are available: **column** for comparing categories, **bar** for the same comparison horizontally, **line** for trends over time, **donut** for proportions, and **number** for a single figure.

Arrange charts directly on the dashboard, without opening the editor:

* Drag a chart to move it within a row or into another row. A row holds up to five charts.
* Drag the divider between two charts to change their widths.
* Drag the handle at the bottom of a row to change its height.
* Drag a chart into empty space to start a new row.

## Who can use it

Viewing and editing are separate permissions. With view access you can read dashboards and adjust filters for your own session; without edit access, the **Add Chart** and **Save** controls don't appear. See [Access Control](/accounts/access-control) for how roles map to permissions.

## FAQ

<AccordionGroup>
  <Accordion title="How fresh is the data?">
    Charts read your call and chat records directly, so a session shows up once it has ended and its analysis has finished. There's no scheduled refresh to wait for.
  </Accordion>

  <Accordion title="Why does my chart say there are too many data points?">
    A chart renders at most 3,000 points. Narrow the time range, use a coarser **View by** unit, or drop a breakdown to bring the count down.
  </Accordion>

  <Accordion title="How do I find the calls behind a chart?">
    Expand the chart to see its breakdown, then open the matching sessions in [call history](/features/session-history).
  </Accordion>

  <Accordion title="Do chart filters override the dashboard filters?">
    No. The two are merged, so a chart filtered to one agent inside a dashboard filtered to last week shows that agent, last week.
  </Accordion>

  <Accordion title="Can one dashboard show both calls and chats?">
    No. Scope is set when you create the dashboard and can't be changed afterwards. Create a second dashboard for the other channel.
  </Accordion>

  <Accordion title="Is there an API for dashboards?">
    Not at the moment. Dashboards are built and read in the dashboard UI. To pull the underlying data out instead, use the [list calls](/api-references/list-calls) and [list chats](/api-references/list-chats) endpoints.
  </Accordion>
</AccordionGroup>
