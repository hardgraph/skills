> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Legacy agent list endpoints removed (07/31/2026)

> On 07/31/2026 Retell removes GET /list-agents and GET /list-chat-agents. Migrate to the unified POST /v2/list-agents endpoint with filters.

## Legacy agent list endpoints are deprecated

**Deprecation date:** 07/31/2026

The following APIs are deprecated as of 07/31/2026. Migrate to the listed replacements.

**Affected APIs:**

* `GET /list-agents`
* `GET /list-chat-agents`

## Endpoint migrations

* `GET /list-agents` -> `POST /v2/list-agents`
* `GET /list-chat-agents` -> `POST /v2/list-agents`

## Migration details

* Update client calls to use `POST /v2/list-agents`.
* To list only voice agents, set `filter_criteria.channel` to `{ "type": "string", "op": "eq", "value": "voice" }`.
* To list only chat agents, set `filter_criteria.channel` to `{ "type": "string", "op": "eq", "value": "chat" }`.
* Read results from `items` instead of expecting a top-level array.
* Continue pagination with the returned `pagination_key` while `has_more` is `true`.
* Stop sending `pagination_key_version`; the new endpoint does not use it.
* Deprecation date: 07/31/2026.

The new endpoint returns one unified list for voice and chat agents. Use the `channel` filter to preserve the old voice-only or chat-only behavior, and update any array-based response handling to read from `items`.
