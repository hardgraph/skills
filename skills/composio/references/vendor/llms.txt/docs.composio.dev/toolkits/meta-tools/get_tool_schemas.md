# Get Tool Schemas

**Slug:** `COMPOSIO_GET_TOOL_SCHEMAS`
**Tags:** readOnlyHint

## Input Parameters

- `tool_slugs` (array<string>) *(required)*: Array of tool slugs to retrieve schemas for. Pass valid tool slugs; never invent.
- `include` (array<string>): Schema fields to include. Defaults to ["input_schema"]. Include "output_schema" when calling tools in the workbench to validate response structure. (default: `input_schema`)
- `session_id` (string): Pass the session_id if you received one from a prior COMPOSIO_SEARCH_TOOLS call.

## Response

- `data` (object) *(required)*: Data from the action execution
  - `success` (boolean) *(required)*: Whether all requested tool schemas were found
  - `tool_schemas` (object) *(required)*: Tool definitions keyed by tool_slug for O(1) lookup. Same format as tool_schemas in search response.
  - `not_found` (array<string>): Tool slugs that were not found
  - `suggestions` (object): For each not-found slug, a list of similar existing tool slugs (up to 3). Call again with the correct slugs to get their schemas.
  - `not_found_message` (string): Action message when slugs are not found. Check suggestions for possible matches and call again with correct slugs.
- `error` (string): Error if any occurred during the execution of the action
- `successful` (boolean) *(required)*: Whether or not the action execution was successful or not


---

📚 **More documentation:** [View all docs](https://docs.composio.dev/llms.txt) | [Glossary](https://docs.composio.dev/llms.mdx/reference/glossary) | [Examples](https://docs.composio.dev/llms.mdx/examples) | [API Reference](https://docs.composio.dev/llms.mdx/reference)