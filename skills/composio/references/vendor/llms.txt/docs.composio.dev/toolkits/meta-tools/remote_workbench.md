# Remote Workbench

**Slug:** `COMPOSIO_REMOTE_WORKBENCH`
**Tags:** destructiveHint, openWorldHint

## Input Parameters

- `code_to_execute` (string) *(required)*: Python to run inside the persistent remote Jupyter sandbox. State (imports, variables, files) is preserved across executions. Keep code concise. Avoid unnecessary comments. Hard 3-minute (180s) execution limit — break large tasks into smaller cells.
- `thought` (string): Brief objective for this step.
- `current_step` (string): Short enum for current step of the workflow execution. Eg FETCHING_EMAILS, GENERATING_REPLIES. Always include to keep execution aligned with the workflow.
- `current_step_metric` (string): Progress metrics for the current step - use to track how far execution has advanced. Format as a string "done/total units" - example "10/100 emails", "0/n messages", "3/10 pages".
- `session_id` (string): Pass the session_id if you received one from a prior COMPOSIO_SEARCH_TOOLS call.

## Response

- `data` (object) *(required)*: Data from the action execution
  - `error` (string): Error message if any
  - `results` (string) *(required)*: The results of the code execution
  - `results_file_path` (string): Path to file containing full results if output was truncated
  - `session` (object): Session info echoed back to reinforce reuse: { id: string, instructions: string }
    - `id` (string) *(required)*
    - `instructions` (string) *(required)*
  - `stderr` (string) *(required)*: The standard error output from the command
  - `stderr_file_path` (string): Path to file containing full stderr if output was truncated
  - `stdout` (string) *(required)*: The standard output from the command
  - `stdout_file_path` (string): Path to file containing full stdout if output was truncated
  - `sandbox_id_suffix` (string): Last 4 digits of the sandbox ID for easier reference
- `error` (string): Error if any occurred during the execution of the action
- `successful` (boolean) *(required)*: Whether or not the action execution was successful or not


---

📚 **More documentation:** [View all docs](https://docs.composio.dev/llms.txt) | [Glossary](https://docs.composio.dev/llms.mdx/reference/glossary) | [Examples](https://docs.composio.dev/llms.mdx/examples) | [API Reference](https://docs.composio.dev/llms.mdx/reference)