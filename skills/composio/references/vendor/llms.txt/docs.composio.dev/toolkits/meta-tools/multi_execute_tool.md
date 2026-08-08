# Multi Execute Tool

**Slug:** `COMPOSIO_MULTI_EXECUTE_TOOL`
**Tags:** openWorldHint, destructiveHint, important

## Input Parameters

- `tools` (array<object>) *(required)*: List of logically independent tools to execute in parallel.
  - Array items:
    - `tool_slug` (string) *(required)*: The slug of the tool to execute — must be a valid tool slug; never invent.
    - `arguments` (object) *(required)*: The arguments to pass to the tool. Use exact field names and types; do not diverge from the tool's argument schema.
- `thought` (string): One-sentence, concise, high-level rationale (no step-by-step).
- `sync_response_to_workbench` (boolean) *(required)*: Predictively set true when the response may be large or needed for later scripting. Saves the full response to the workbench while returning an inline preview. If the result is small, keep it inline. Default false.
- `current_step` (string): Short enum for current step of the workflow execution. Eg FETCHING_EMAILS, GENERATING_REPLIES. Always include to keep execution aligned with the workflow.
- `current_step_metric` (string): Progress metrics for the current step - use to track how far execution has advanced. Format as a string "done/total units" - example "10/100 emails", "0/n messages", "3/10 pages".
- `session_id` (string): Pass the session_id if you received one from a prior COMPOSIO_SEARCH_TOOLS call.

## Response

- `data` (object) *(required)*: Data from the action execution
  - `error_count` (integer) *(required)*: Number of failed tool executions
  - `results` (array<object>) *(required)*: List of responses from executing the tools. Order matches the input order.
    - Array items:
      - `error` (string): Error message if the tool execution failed
      - `index` (integer) *(required)*: Original index of the tool in the request
      - `response` (object): The response from executing the tool if successful
      - `tool_slug` (string) *(required)*: The slug of the tool that was executed
  - `success_count` (integer) *(required)*: Number of successful tool executions
  - `total_count` (integer) *(required)*: Total number of tools executed
  - `session` (object): Session info echoed back to reinforce reuse: { id: string, instructions: string }
    - `id` (string)
    - `instructions` (string)
  - `remote_file_info` (object): Information about the complete response saved to a remote file when response is too large or when sync_response_to_workbench is true
  - `next_steps` (string): Next steps for the caller
- `error` (string): Error if any occurred during the execution of the action
- `successful` (boolean) *(required)*: Whether or not the action execution was successful or not


---

📚 **More documentation:** [View all docs](https://docs.composio.dev/llms.txt) | [Glossary](https://docs.composio.dev/llms.mdx/reference/glossary) | [Examples](https://docs.composio.dev/llms.mdx/examples) | [API Reference](https://docs.composio.dev/llms.mdx/reference)