# Search Tools

**Slug:** `COMPOSIO_SEARCH_TOOLS`
**Tags:** important, openWorldHint, readOnlyHint

## Input Parameters

- `queries` (array<object>) *(required)*: Structured English search queries to process in parallel. Split independent app/API actions into separate queries, including hidden prerequisites. Each query returns 4-6 tools.
  - Array items:
    - `use_case` (string) *(required)*: Provide a normalized English description of the complete use case to enable precise planning. Focus on the specific action and intended outcome. Include any specific apps if mentioned by user in each use_case. Do NOT include personal identifiers (names, emails, IDs) here — put those in known_fields.
    - `known_fields` (string): Provide known workflow inputs as a single English string of comma-separated key:value pairs (not an array). Keep 1-2 short, structured items - stable identifiers, names, emails, or settings only. Omit if not relevant. No free-form or long text (messages, notes, descriptions).
- `session` (object): Session context for correlating meta tool calls within a workflow. Always pass this parameter. Use {generate_id: true} for new workflows or {id: "EXISTING_ID"} to continue existing workflows.
  - `id` (string): Existing session identifier for the current workflow to reuse across calls.
  - `generate_id` (boolean): Set to true for the first search call of a new usecase/workflow to generate a new session ID. When user pivots to a different task, set this true. If omitted or false with an existing session.id, the provided session ID will be reused.
- `model` (string): Client LLM model name (recommended). Used to optimize planning/search behavior. Ignored if omitted or invalid.

## Response

- `data` (object) *(required)*: Data from the action execution
  - `results` (array<object>) *(required)*: Per-query search results with tools, reasoning, and memory. One entry per query in request order.
    - Array items:
      - `index` (integer) *(required)*: 1-based index of the query in the request
      - `use_case` (string) *(required)*: The use case that was searched
      - `primary_tool_slugs` (array<string>) *(required)*: List of main tool slugs matching the search criteria
      - `related_tool_slugs` (array<string>) *(required)*: List of related tool slugs that might be useful
      - `toolkits` (array<string>) *(required)*: List of unique toolkit slugs used by tools in this query
      - `reasoning` (string) *(required)*: Reasoning for the search results
      - `task_difficulty` (string) *(required)*: Assessment of task difficulty for this specific query
      - `recommended_plan_steps` (array<string>): Workflow steps from cached plan (only present when cached plan is available)
      - `known_pitfalls` (array<string>): Common pitfalls and considerations (only present when cached plan is available)
      - `reference_workbench_snippets` (array<object>): Reference Python code snippets for processing tool responses in the workbench (only present when cached plan is available)
        - Array items:
          - `description` (string) *(required)*: Description of what the code snippet does
          - `code` (string) *(required)*: Python code snippet for the workbench, wrapped in triple backticks
      - `error` (string): Error message if the search failed, null otherwise
  - `tool_schemas` (object) *(required)*: Deduplicated tool definitions keyed by tool_slug for O(1) lookup. Each tool appears once even if used in multiple queries.
  - `toolkit_connection_statuses` (array<object>) *(required)*: Connection status for all toolkits mentioned across all queries, with descriptions merged in
    - Array items:
      - `toolkit` (string) *(required)*: The toolkit slug identifier (e.g., "gmail", "slack")
      - `description` (string) *(required)*: Description of what the toolkit does and its capabilities
      - `has_active_connection` (boolean) *(required)*: Whether an active connection exists for this toolkit
      - `connection_details` (object) *(required)*: Connection details including auth config and connected account IDs
      - `current_user_info` (object): Information about the currently connected user (email, name, etc.)
      - `account_type` (string): Sharing model for the connected account when has_active_connection is true. PRIVATE is owner-only; SHARED is reachable only when explicitly pinned to the session. — values: `PRIVATE`, `SHARED`
      - `status_message` (string) *(required)*: Human-readable message about the connection status and next steps
  - `time_info` (object) *(required)*: Time information for the query
    - `current_time_utc` (string) *(required)*: Current time in ISO format (UTC)
    - `current_time_utc_epoch_seconds` (integer) *(required)*: Current time as Unix epoch timestamp in seconds
    - `message` (string) *(required)*: Important message about time handling and timezone considerations
  - `session` (object) *(required)*: Session info for correlating meta tool calls
    - `id` (string) *(required)*: Session identifier to be passed to subsequent meta tool calls
    - `generate_id` (boolean) *(required)*: Whether a fresh session id was generated in this call
    - `instructions` (string) *(required)*: LLM-facing guidance on how to reuse this session id
  - `next_steps_guidance` (array<string>) *(required)*: Combined workflow guidance covering connections, planner, and memory usage. Each element is a step instruction.
- `error` (string): Error if any occurred during the execution. Format: "X out of Y searches failed, reasons: <details>"
- `successful` (boolean) *(required)*: Whether all searches completed successfully. False if any query failed


---

📚 **More documentation:** [View all docs](https://docs.composio.dev/llms.txt) | [Glossary](https://docs.composio.dev/llms.mdx/reference/glossary) | [Examples](https://docs.composio.dev/llms.mdx/examples) | [API Reference](https://docs.composio.dev/llms.mdx/reference)