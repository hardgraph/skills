> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Legacy list endpoints removed for v2/v3 (06/15/2026)

> On 06/15/2026 Retell removes legacy list endpoints (batch tests, conversation flows, phone numbers, Retell LLMs) — migrate to versioned v2 and v3 endpoints.

## Legacy list endpoints are deprecated

The following list endpoints are deprecated in favor of newer versioned endpoints with unified pagination patterns.

**Affected APIs:**

* [List Batch Tests](/api-references/list-batch-tests)
* [List Conversation Flow Components](/api-references/list-conversation-flow-components)
* [List Conversation Flows](/api-references/list-conversation-flows)
* [List Phone Numbers](/api-references/list-phone-numbers)
* [List Retell LLMs](/api-references/list-retell-llms)
* [List Test Case Definitions](/api-references/list-test-case-definitions)
* [List Test Runs](/api-references/list-test-runs)
* [List Calls](/api-references/list-calls)
* [List Chats](/api-references/list-chats)

## Endpoint migrations

* `GET /list-batch-tests` -> `GET /v2/list-batch-tests` ([List Batch Tests](/api-references/list-batch-tests))
* `GET /list-conversation-flow-components` -> `GET /v2/list-conversation-flow-components` ([List Conversation Flow Components](/api-references/list-conversation-flow-components))
* `GET /list-conversation-flows` -> `GET /v2/list-conversation-flows` ([List Conversation Flows](/api-references/list-conversation-flows))
* `GET /list-phone-numbers` -> `GET /v2/list-phone-numbers` ([List Phone Numbers](/api-references/list-phone-numbers))
* `GET /list-retell-llms` -> `GET /v2/list-retell-llms` ([List Retell LLMs](/api-references/list-retell-llms))
* `GET /list-test-case-definitions` -> `GET /v2/list-test-case-definitions` ([List Test Case Definitions](/api-references/list-test-case-definitions))
* `GET /list-test-runs/{test_case_batch_job_id}` -> `GET /v2/list-test-runs/{test_case_batch_job_id}` ([List Test Runs](/api-references/list-test-runs))
* `POST /v2/list-calls` -> `POST /v3/list-calls` ([List Calls](/api-references/list-calls))
* `GET /list-chat` -> `POST /v3/list-chats` ([List Chats](/api-references/list-chats))

**What's changing on 06/15/2026:**

1. The legacy list endpoints above will no longer be supported.
2. `GET /list-chat` changes both method and path to `POST /v3/list-chats`.
3. Versioned list endpoints return unified pagination fields: `items`, `pagination_key`, and `has_more`.

**Migration:**

* Update each client call to the new method/path listed above.
* Update response handling to read `items` from the paginated response object (instead of expecting a top-level array from legacy endpoints).
* Keep using `pagination_key` and `has_more` for page traversal.

**Effective date:** After 06/15/2026, the legacy endpoints listed above will no longer be supported.

## Analysis prompt fields are deprecated

The following top-level analysis prompt fields on voice and chat agent configurations are deprecated in favor of the `post_call_analysis_data` and `post_chat_analysis_data` arrays.

**Deprecated fields:**

* `analysis_summary_prompt`
* `analysis_successful_prompt`
* `analysis_user_sentiment_prompt`

These fields exist on both voice agent and chat agent endpoints:

* [Create Agent](/api-references/create-agent) / [Update Agent](/api-references/update-agent)
* [Create Chat Agent](/api-references/create-chat-agent) / [Update Chat Agent](/api-references/update-chat-agent)

**What's changing on 06/15/2026:**

1. The three prompt fields above will be removed from the API.
2. Use system preset items inside `post_call_analysis_data` (voice agents) or `post_chat_analysis_data` (chat agents) to customize prompts for summary, success, and sentiment analysis.

**Migration:**

Replace each deprecated field with a system preset entry in the corresponding analysis data array. Set `type` to `system-presets`, `name` to the preset identifier, and `description` to your custom prompt.

| Deprecated field                 | Preset `name` (voice) | Preset `name` (chat) |
| -------------------------------- | --------------------- | -------------------- |
| `analysis_summary_prompt`        | `call_summary`        | `chat_summary`       |
| `analysis_successful_prompt`     | `call_successful`     | `chat_successful`    |
| `analysis_user_sentiment_prompt` | `user_sentiment`      | `user_sentiment`     |

For example, if you currently set `analysis_summary_prompt` on a voice agent:

<CodeGroup>
  ```json Before theme={"dark"}
  {
    "analysis_summary_prompt": "Summarize the outcome of the conversation in two sentences."
  }
  ```

  ```json After theme={"dark"}
  {
    "post_call_analysis_data": [
      {
        "type": "system-presets",
        "name": "call_summary",
        "description": "Summarize the outcome of the conversation in two sentences."
      }
    ]
  }
  ```
</CodeGroup>
