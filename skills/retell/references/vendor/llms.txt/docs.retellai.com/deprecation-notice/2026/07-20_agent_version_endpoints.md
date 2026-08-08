> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Unified publish-agent-version endpoint (07/20/2026)

> On 07/20/2026 Retell deprecates the legacy publish-agent and publish-chat-agent endpoints — migrate to the unified publish-agent-version endpoint.

## Legacy agent publish endpoints are deprecated

**Deprecation date:** 07/20/2026

The following APIs are deprecated as of 07/20/2026. Migrate to the listed replacements.

**Affected APIs:**

* `POST /publish-agent/{agent_id}`
* `POST /publish-chat-agent/{agent_id}`

## Endpoint migrations

* `POST /publish-agent/{agent_id}` -> `POST /publish-agent-version/{agent_id}` ([Publish Agent](/api-references/publish-agent))
* `POST /publish-chat-agent/{agent_id}` -> `POST /publish-agent-version/{agent_id}` ([Publish Chat Agent](/api-references/publish-chat-agent))

## Migration details

* Update client calls to use the replacement method and path listed above.
* Deprecation date: 07/20/2026.
