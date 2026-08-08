> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Conversation node tools replaced by subagent (04/18/2026)

> On 04/18/2026 Retell deprecates `tools` and `tool_ids` on conversation nodes in favor of the new subagent node for dialogue that calls tools mid-call.

## Deprecating tools on conversation nodes — use subagent nodes instead

The `tools` and `tool_ids` fields on conversation nodes are deprecated in favor of the new `subagent` node type. The conversation node type itself is **not** deprecated — it continues to work for nodes that don't use tools. The new `subagent` node type provides the same conversation-with-tools functionality — the agent converses with the user while being able to call tools — but as a dedicated node type.

**Affected APIs:**

* [Create Conversation Flow](/api-references/create-conversation-flow)
* [Update Conversation Flow](/api-references/update-conversation-flow)
* [Get Conversation Flow](/api-references/get-conversation-flow)
* [List Conversation Flows](/api-references/list-conversation-flows)
* [Create Conversation Flow Component](/api-references/create-conversation-flow-component)
* [Update Conversation Flow Component](/api-references/update-conversation-flow-component)
* [Get Conversation Flow Component](/api-references/get-conversation-flow-component)
* [List Conversation Flow Components](/api-references/list-conversation-flow-components)

**Deprecated fields on `type: "conversation"` nodes:**

* `tools`
* `tool_ids`

**Use instead:** Set `type: "subagent"` on nodes that need tools.

**What's changing on 04/18/2026:**

1. Existing conversation nodes with tools will be automatically migrated to `type: "subagent"` — API responses will return the new type for these nodes.
2. Conversation nodes with tools that use static text instructions will be converted to subagent nodes with prompt text instructions.
3. The API will stop accepting `tools` or `tool_ids` fields on `type: "conversation"` nodes.

**Migration:**

* For conversation nodes **with** tools: change `type` from `"conversation"` to `"subagent"`. If the node uses a `static_text` instruction, change it to `prompt`. All other fields remain the same.
* For conversation nodes **without** tools: no changes needed — these remain `type: "conversation"`.

**Example:**

* Before:
  ```json theme={"dark"}
  {
    "id": "handle_order",
    "type": "conversation",
    "instruction": {
      "text": "Help the user check their order status.",
      "type": "prompt"
    },
    "tools": [
      {
        "type": "custom_function",
        "name": "check_order_status",
        "description": "Look up order by order number",
        "parameters": { "type": "object", "properties": { "order_number": { "type": "string" } } }
      }
    ]
  }
  ```
* After:
  ```json theme={"dark"}
  {
    "id": "handle_order",
    "type": "subagent",
    "instruction": {
      "text": "Help the user check their order status.",
      "type": "prompt"
    },
    "tools": [
      {
        "type": "custom_function",
        "name": "check_order_status",
        "description": "Look up order by order number",
        "parameters": { "type": "object", "properties": { "order_number": { "type": "string" } } }
      }
    ]
  }
  ```

**Effective date:** After 04/18/2026, the API will no longer accept `tools` or `tool_ids` on `type: "conversation"` nodes. Use `type: "subagent"` instead.
