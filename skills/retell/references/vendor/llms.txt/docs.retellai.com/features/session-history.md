> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Call & chat history

> Browse Retell call and chat history in the dashboard: filter sessions, inspect transcripts and analysis, rerun analysis, and export to CSV.

The **Call History** and **Chat History** pages under **Data** in the dashboard show your sessions after they end, including transcripts, analysis results, costs, and outcomes. To follow a call that is still in progress, use [Live Monitoring](/features/live-monitoring).

## When to use call and chat history

Use these pages to:

* Investigate an unsuccessful or disconnected session.
* Review a recording, transcript, post-session analysis, or call logs.
* Compare sessions for an agent or agent version.
* Find sessions that match analysis fields, metadata, dynamic variables, or custom attributes.
* Export a filtered set of calls or chats for offline analysis.

For example, if callers report that an outbound campaign is failing, filter Call History by batch call ID and an unsuccessful outcome. Open individual calls to compare their disconnection reasons, transcripts, and detail logs.

## Browse and filter sessions

Both pages show one row per session with its time, cost, session ID, status, sentiment, phone numbers, and agent details. Call History also includes call-specific columns such as duration, channel type, direction, end reason, outcome, and end-to-end latency.

<Frame caption="Call History controls and session table.">
  <img src="https://mintcdn.com/retellai/2B3R6a7IdWxVRs_t/images/session-history-call-list.png?fit=max&auto=format&n=2B3R6a7IdWxVRs_t&q=85&s=7e72f7f173e5181a7f11287144048c73" alt="Call History page with the Date Range, Filter, Customize View, Show PII, and Actions controls outlined in blue above the session table." width="1600" height="900" data-path="images/session-history-call-list.png" />
</Frame>

Use **Date Range** and **Filter** to narrow the list:

* **Call History** supports agent and version, session ID, batch call ID, transfer agent, call type, direction, duration, phone number, status, outcome, sentiment, disconnection reason, end-to-end latency, cost, analysis fields, custom attributes, dynamic variables, and metadata.
* **Chat History** supports agent and version, session ID, status, outcome, sentiment, disconnection reason, analysis fields, and custom attributes.

Chat statuses are `ongoing`, `ended`, or `error`. An ongoing chat becomes `ended` when the agent or user ends it, when you call the [End Chat API](/api-references/end-chat), or when the [inactivity timeout](/build/create-chat-agent#chat-settings) expires. Calls also include pre-connection statuses such as `registered` and `not_connected`.

Filters persist for each workspace in your browser and appear in the URL, so you can bookmark or share a filtered view with another workspace member. Use **Customize View** to show, hide, and reorder built-in columns, custom analysis fields, and workspace custom attributes. The table shows 50 rows by default, with options for 10, 20, 50, or 100 rows per page.

<Frame caption="Chat History with session filters and view controls.">
  <img src="https://mintcdn.com/retellai/yfI-f20iptEBfJzl/images/chat-history-table.png?fit=max&auto=format&n=yfI-f20iptEBfJzl&q=85&s=8efe59999cac42396c789d54ee378a68" alt="Chat History page showing a session table with time, cost, session ID, status, sentiment, and phone number columns, plus Date Range, Filter, Customize View, Show PII, and Actions controls." width="1600" height="900" data-path="images/chat-history-table.png" />
</Frame>

## Inspect a session

Select a row to open its details. Use the arrows at the top of the panel to move through sessions without closing it.

Every session can include:

* **Conversation Analysis:** session outcome, status, user sentiment, disconnection reason, and custom analysis fields.
* **Summary:** the generated session summary.
* **Transcription:** messages, tool calls and results, state or node transitions, in-call SMS, and knowledge base retrievals. You can copy the transcript or open the conversation in the test playground.
* **Data:** provided, overridden, and extracted dynamic variables, plus metadata.
* **Contact:** the matching [contact](/features/contacts) and their previous conversations. See [contact memory](/integrations/build-contact-memory) for how contacts accumulate context across sessions.

Call details can also include a recording player, end-to-end latency, **Detail Logs** for step-by-step execution, and **Packet Capture** for [debugging SIP connections](/reliability/debug-calls-pcap). Select a transcript timestamp to jump to that point in the recording. If an error disconnection reason links to a detail log, select the reason to open the matching error.

<Frame caption="Call details with analysis and transcript tabs.">
  <img src="https://mintcdn.com/retellai/2B3R6a7IdWxVRs_t/images/session-history-call-details.png?fit=max&auto=format&n=2B3R6a7IdWxVRs_t&q=85&s=73d9ceb4d9b748f0631ad60ba84373c0" alt="Call details panel showing recording playback, Conversation Analysis, Summary, contact history, and the Transcription, Data, Detail Logs, and Packet Capture tabs outlined in blue. Real phone numbers are partially masked; resource IDs remain visible." width="1600" height="900" data-path="images/session-history-call-details.png" />
</Frame>

For the next debugging step, see [debug call disconnections](/reliability/debug-call-disconnect), [check call latency](/reliability/check-actual-latency), or [debug calls with PCAP](/reliability/debug-calls-pcap).

<Frame caption="Chat details with analysis, summary, transcript, and contact history.">
  <img src="https://mintcdn.com/retellai/yfI-f20iptEBfJzl/images/chat-history-detail.png?fit=max&auto=format&n=yfI-f20iptEBfJzl&q=85&s=ec0dca89a905a2eb207652dde899e66e" alt="Chat detail panel showing the agent and session information, Conversation Analysis, Summary, Transcription and Data tabs, messages, and the matched contact's previous conversations." width="1600" height="900" data-path="images/chat-history-detail.png" />
</Frame>

## Rerun analysis

Select **Rerun** in Conversation Analysis to re-extract analysis fields for one session after changing the agent's analysis configuration. Rerunning incurs the analysis cost again and does not resend webhooks.

To rerun analysis for many sessions, filter Call History or Chat History, then open **Actions** and choose **Backfill from Post-Call Data** or **Backfill from Chat Data**. The backfill uses the latest draft agent configuration and the active filters.

You can also rerun analysis through the [Rerun Call Analysis API](/api-references/rerun-call-analysis) or [Rerun Chat Analysis API](/api-references/rerun-chat-analysis). See [rerun post-call and post-chat analysis](/features/rerun-call-analysis) for filtering, transfer-agent behavior, and billing details.

## Export sessions to CSV

Exporting uses the active filters, so narrow the history table before creating the export.

<Steps>
  <Step title="Choose the export fields">
    Open **Actions**, select **Export**, and choose the columns to include. Export-only fields include transcript data and PII-scrubbed transcripts. Call exports can also include recording and public log URLs.
  </Step>

  <Step title="Submit the export">
    Submit the request. Retell generates the CSV in the background. If the request exceeds your workspace's export limit, narrow the filters and try again.
  </Step>

  <Step title="Download the CSV">
    Open **Actions → Export records** to check the request status and download the completed file.
  </Step>
</Steps>

Completed export records are kept for one week.

## Control sensitive data

When [PII scrubbing](/accounts/privacy-disable) is enabled, the table and detail panel show scrubbed content by default. Members with permission to view raw data can use **Show PII** to reveal the original version. The setting applies wherever scrubbed copies are available, including transcripts, recordings, analysis, logs, dynamic variables, and metadata.

Authorized members can open the trash menu in a call's detail panel to:

* **Remove Sensitive Data**, which permanently removes sensitive call artifacts while keeping basic call attributes.
* **Delete**, which permanently removes the call and its associated data.

<Warning>
  Removing sensitive call data and deleting a call cannot be undone.
</Warning>

Chat History does not have the same delete menu. Use the [Delete Chat API](/api-references/delete-chat) to remove an individual chat. The agent's [data storage settings](/accounts/privacy-disable) and [data retention policy](/accounts/data-retention) determine which artifacts remain available for both calls and chats.

## FAQ

<AccordionGroup>
  <Accordion title="How long are calls, chats, and recordings stored?">
    By default, Retell stores session data without automatic deletion. You can set a per-agent [data retention period](/accounts/data-retention) to delete data after 1–730 days. All session history is also permanently deleted if you [delete your account](/accounts/account#delete-your-account).
  </Accordion>

  <Accordion title="Why is a recording, transcript, or log missing?">
    These artifacts become available after the session ends. They can also be unavailable because the call never connected, the agent stores only basic attributes, a retention period expired, sensitive data was removed, or your role cannot view the data.

    Review the agent's [data storage settings](/accounts/privacy-disable) and ask a workspace admin to confirm your role.
  </Accordion>

  <Accordion title="Why does a chat still show as ongoing?">
    A chat stays `ongoing` until the agent or user ends it, you call the [End Chat API](/api-references/end-chat), or the agent's [inactivity timeout](/build/create-chat-agent#chat-settings) expires. Long-running rows usually indicate a longer timeout.
  </Accordion>

  <Accordion title="What format are call recordings in?">
    Retell stores call recordings as WAV audio. A call can include a mixed single-channel recording, a recording with each party on a separate channel, and PII-scrubbed versions when configured.

    Use the [Get Call API](/api-references/get-call) to retrieve the available recording fields.
  </Accordion>

  <Accordion title="Can I retrieve call and chat history through the API?">
    Yes. Use the [List Calls API](/api-references/list-calls) to filter and paginate calls, then the [Get Call API](/api-references/get-call) for one call's complete details. Use [List Chats](/api-references/list-chats) and [Get Chat](/api-references/get-chat) for chats.
  </Accordion>
</AccordionGroup>
