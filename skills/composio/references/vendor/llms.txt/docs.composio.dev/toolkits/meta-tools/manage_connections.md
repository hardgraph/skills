# Manage Connections

**Slug:** `COMPOSIO_MANAGE_CONNECTIONS`
**Tags:** openWorldHint, destructiveHint

## Input Parameters

- `toolkits` (array<string>) *(required)*: Toolkit slugs to check or connect. Must be valid toolkit slugs; never invent. Missing connections initiate auth. Examples: ['gmail', 'github', 'slack', 'googlesheets', 'outlook'].
- `reinitiate_all` (boolean): Force reconnection for all listed toolkits, even if active connections already exist. Use when credentials may be stale, you need fresh credentials/settings, or you are troubleshooting connection issues. This replaces existing active connections with new auth-link flows. Default false. (default: `false`)
- `session_id` (string): Pass the session_id if you received one from a prior COMPOSIO_SEARCH_TOOLS call.

## Response

- `data` (object) *(required)*: Data from the action execution
  - `message` (string) *(required)*: Overall status message
  - `results` (object) *(required)*: Connection results for each toolkit
  - `summary` (object): Summary of results by status type
    - `total_toolkits` (integer)
    - `active_connections` (integer)
    - `initiated_connections` (integer)
    - `failed_connections` (integer)
  - `session` (object): Session info echoed back to reinforce reuse: { id: string, instructions: string }
    - `id` (string) *(required)*
    - `instructions` (string) *(required)*
- `error` (string): Error if any occurred during the execution of the action
- `successful` (boolean) *(required)*: Whether or not the action execution was successful or not


---

📚 **More documentation:** [View all docs](https://docs.composio.dev/llms.txt) | [Glossary](https://docs.composio.dev/llms.mdx/reference/glossary) | [Examples](https://docs.composio.dev/llms.mdx/examples) | [API Reference](https://docs.composio.dev/llms.mdx/reference)