> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Deprecation Overview

> Browse upcoming and past Retell API deprecations and breaking changes by date — subscribe via RSS to get notified when new deprecation notices are posted.

This page tracks every Retell API deprecation and breaking change. Click the RSS button at the top of the page to subscribe and receive notifications when new deprecations are announced.

These changes take effect in the API itself, so staying on an older [SDK](/get-started/sdk#versioning) version doesn't defer them. The SDKs drop a retired endpoint or field in the first release generated after the API removes it.

## Upcoming deprecations

Announced but not yet in effect. Migrate before the date listed on each entry.

<Update label="08/31/2026" tags={["API","Upcoming"]} rss="override_dynamic_variables on Update Call is deprecated and Update Call becomes ended-calls-only on 08/31/2026. Use the new Update Live Call API (PATCH /v2/update-live-call/{call_id}) to override dynamic variables on ongoing calls.">
  The `override_dynamic_variables` input on Update Call is deprecated, and Update Call becomes ended-calls-only. Changes you make through Update Call — including `data_storage_setting`, which previously took effect mid-call — now apply only to ended calls. Use the new [Update Live Call](/api-references/update-live-call) API to override dynamic variables or change `data_storage_setting` on ongoing calls.

  **Impacted API endpoints:**

  * [Update Call](/api-references/update-call) — field: `override_dynamic_variables`

  [Read more](/deprecation-notice/2026/08-31_update_call_ended_calls_only).
</Update>

## Past deprecations

Already in effect. If your integration still uses anything below, it's already broken or running on migrated defaults.

<Update label="07/31/2026" tags={["API"]} rss="The scalar 'multi' value of the agent language field was removed on 07/31/2026. Use the explicit locale array form, for example ['en-US','es-ES']. Existing agents were migrated automatically.">
  The scalar `"multi"` value of the agent `language` field was removed in favor of the explicit locale-array form (for example, `["en-US","es-ES"]`). Existing agents were migrated automatically to the equivalent 10-language array, but client code that sends `"multi"` is now rejected.

  **Impacted API endpoints:**

  * [Create Agent](/api-references/create-agent), [Update Agent](/api-references/update-agent), [Create Chat Agent](/api-references/create-chat-agent), [Update Chat Agent](/api-references/update-chat-agent) — field: `language` (scalar value `"multi"`)

  [Read more](/deprecation-notice/2026/07-31_legacy_multilingual_setting).
</Update>

<Update label="07/31/2026" tags={["API"]} rss="Legacy agent list endpoints GET /list-agents and GET /list-chat-agents were removed on 07/31/2026. Use POST /v2/list-agents with the channel filter for voice or chat agents.">
  Legacy agent list endpoints were removed. Use the unified [List Agents](/api-references/list-agents) API and filter by `channel` for voice or chat agents.

  **Impacted API endpoints:**

  * [List Voice Agents](/api-references/list-agents), [List Chat Agents](/api-references/list-chat-agents) — endpoints replaced by `POST /v2/list-agents`

  [Read more](/deprecation-notice/2026/07-31_agent_list_endpoints).
</Update>

<Update label="07/20/2026" tags={["MCP"]} rss="The legacy hosted MCP server at retell.stlmcp.com was removed on 07/20/2026. Point your MCP client to mcp.retellai.com — see the Retell MCP server guide.">
  The legacy hosted MCP server at `retell.stlmcp.com` was removed. Point your MCP client to `mcp.retellai.com` instead — see the [Retell MCP server](/get-started/mcp-server) guide.

  **Impacted:**

  * `retell.stlmcp.com` — removed 07/20/2026. Use `mcp.retellai.com` (same Bearer API-key auth).

  [Read more](/deprecation-notice/2026/07-20_legacy_mcp_server).
</Update>

<Update label="07/20/2026" tags={["API"]} rss="New API deprecation notice: Legacy agent publish endpoints. Deprecation date: 07/20/2026. See the migration notice for affected APIs and replacement guidance.">
  Legacy agent publish endpoints were removed. See the migration notice for affected APIs and replacement guidance.

  **Impacted API endpoints:**

  * `POST /publish-agent/{agent_id}`
  * `POST /publish-chat-agent/{agent_id}`

  [Read more](/deprecation-notice/2026/07-20_agent_version_endpoints).
</Update>

<Update label="07/12/2026" tags={["Models"]} rss="ElevenLabs Turbo voice model replacements effective 7/12/2026: eleven_turbo_v2 -> eleven_flash_v2; eleven_turbo_v2_5 -> eleven_flash_v2_5.">
  ElevenLabs Turbo voice models were deprecated and automatically migrated to their Flash counterparts on 7/12/2026. We have confirmed with ElevenLabs and verified that Flash delivers the same voice quality at lower latency.

  [Read more](/deprecation-notice/2026/07-12_elevenlabs_turbo_models).
</Update>

<Update label="06/15/2026" tags={["API"]} rss="Legacy list endpoints and analysis prompt fields were removed on 06/15/2026. Migrate to versioned list endpoints (v2/v3) and to post_call_analysis_data / post_chat_analysis_data system presets.">
  Legacy list endpoints and analysis prompt fields were removed. Migrate to the versioned list endpoints (v2/v3) and to `post_call_analysis_data` / `post_chat_analysis_data` system presets.

  **Impacted API endpoints:**

  * [List Batch Tests](/api-references/list-batch-tests), [List Conversation Flow Components](/api-references/list-conversation-flow-components), [List Conversation Flows](/api-references/list-conversation-flows), [List Phone Numbers](/api-references/list-phone-numbers), [List Retell LLMs](/api-references/list-retell-llms), [List Test Case Definitions](/api-references/list-test-case-definitions), [List Test Runs](/api-references/list-test-runs), [List Calls](/api-references/list-calls), [List Chats](/api-references/list-chats) — endpoints replaced by versioned (v2/v3) equivalents
  * [Create Agent](/api-references/create-agent), [Update Agent](/api-references/update-agent), [Create Chat Agent](/api-references/create-chat-agent), [Update Chat Agent](/api-references/update-chat-agent) — fields: `analysis_summary_prompt`, `analysis_successful_prompt`, `analysis_user_sentiment_prompt`

  [Read more](/deprecation-notice/2026/06-15_legacy_list_endpoints).
</Update>

<Update label="05/25/2026" tags={["Models"]} rss="Claude and Gemini model replacements effective 5/25/2026: claude-4.0-sonnet -> claude-4.6-sonnet; gemini-2.0-flash and gemini-2.5-flash -> gemini-3.0-flash; gemini-2.0-flash-lite -> gemini-3.1-flash-lite.">
  Claude and Gemini models were deprecated and automatically migrated to newer versions on 5/25/2026.

  [Read more](/deprecation-notice/2026/05-25_model_replacements).
</Update>

<Update label="04/18/2026" tags={["API"]} rss="tools and tool_ids on conversation nodes are deprecated. Use type: 'subagent' nodes instead. Existing conversation nodes with tools were auto-migrated to subagent nodes on 04/18/2026.">
  The `tools` and `tool_ids` fields on `type: "conversation"` nodes are deprecated in favor of the `subagent` node type.

  **Impacted API endpoints:**

  * [Create Conversation Flow](/api-references/create-conversation-flow), [Update Conversation Flow](/api-references/update-conversation-flow), [Get Conversation Flow](/api-references/get-conversation-flow), [List Conversation Flows](/api-references/list-conversation-flows), [Create Conversation Flow Component](/api-references/create-conversation-flow-component), [Update Conversation Flow Component](/api-references/update-conversation-flow-component), [Get Conversation Flow Component](/api-references/get-conversation-flow-component), [List Conversation Flow Components](/api-references/list-conversation-flow-components) — fields: `tools`, `tool_ids` (on `type: "conversation"` nodes)

  [Read more](/deprecation-notice/2026/04-18_conversation_node_tools).
</Update>

<Update label="04/03/2026" tags={["Models"]} rss="OpenAI Realtime and Cartesia Sonic model replacements effective 4/3/2026: gpt-4o-realtime -> gpt-realtime-1.5; gpt-4o-mini-realtime -> gpt-realtime-mini; sonic-2 and sonic-turbo -> sonic-3.">
  OpenAI Realtime and Cartesia Sonic models were deprecated and replaced with newer versions. The deprecated models are no longer available.

  [Read more](/deprecation-notice/2026/04-03_model_replacements).
</Update>

<Update label="03/31/2026" tags={["API"]} rss="Phone number single-agent fields (inbound_agent_id, outbound_agent_id, inbound_sms_agent_id, outbound_sms_agent_id and their *_version siblings) are deprecated. Use the weighted *_agents lists instead.">
  The single-agent fields on phone number configuration are deprecated in favor of the weighted `*_agents` lists.

  **Impacted API endpoints:**

  * [Create Phone Number](/api-references/create-phone-number), [Import Phone Number](/api-references/import-phone-number), [Update Phone Number](/api-references/update-phone-number), [Get Phone Number](/api-references/get-phone-number), [List Phone Numbers](/api-references/list-phone-numbers) — fields: `inbound_agent_id`, `inbound_agent_version`, `outbound_agent_id`, `outbound_agent_version`, `inbound_sms_agent_id`, `inbound_sms_agent_version`, `outbound_sms_agent_id`, `outbound_sms_agent_version`

  [Read more](/deprecation-notice/2026/03-31_phone_number_agent_fields).
</Update>

<Update label="01/23/2026" tags={["API"]} rss="show_transferee_as_caller no longer toggles SIP REFER vs SIP INVITE. Use the new cold_transfer_mode parameter (sip_refer or sip_invite). show_transferee_as_caller now only controls caller ID display under sip_invite.">
  `show_transferee_as_caller` no longer toggles SIP REFER vs SIP INVITE in Cold Transfer options. Use the `cold_transfer_mode` parameter instead; `show_transferee_as_caller` now only controls caller ID display under `sip_invite`.

  **Impacted API endpoints:**

  * [Create Retell LLM](/api-references/create-retell-llm), [Update Retell LLM](/api-references/update-retell-llm), [Create Conversation Flow](/api-references/create-conversation-flow), [Update Conversation Flow](/api-references/update-conversation-flow), [Create Conversation Flow Component](/api-references/create-conversation-flow-component), [Update Conversation Flow Component](/api-references/update-conversation-flow-component) — field: `show_transferee_as_caller`

  [Read more](/deprecation-notice/2026/01-23_cold_transfer_mode_selection).
</Update>
