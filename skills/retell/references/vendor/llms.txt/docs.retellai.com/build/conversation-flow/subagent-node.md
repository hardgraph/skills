> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Subagent node

> Subagent nodes let a Retell conversation flow combine dialogue with on-the-fly tool calls — the LLM decides whether and when to use each attached tool.

The subagent node holds a conversation with the user while letting the agent call tools (also called functions) during the dialogue. The LLM decides whether and when to use each attached tool based on what the user says.

If you only need dialogue without tool calling, use a [conversation node](/build/conversation-flow/conversation-node).

For example, a customer-support agent can use one subagent node to handle order questions: it chats with the caller, and only when the caller provides an order number does it invoke the order-lookup tool — no separate flow branch needed for callers who never ask.

## How it works

When a subagent node has tools attached, the LLM receives both the node instruction and the list of available tools. During the conversation, the LLM determines when a tool should be called based on context, extracts the required parameters, and invokes it while maintaining the dialogue with the user.

* Multiple tools can be added to a single subagent node.
* The agent can continue talking while a tool executes.
* Tool results are available to the LLM for generating follow-up responses.

## Write the instruction

Subagent nodes only support **Prompt** instructions. Unlike a conversation node, **Static Sentence** is not supported.

Write the instruction to define the task, what information the agent should gather, and when it should use the available tools.

```text theme={"dark"}
Help the user check their order status. If the user provides an order number,
use the available order lookup tool to retrieve the latest status.
```

## Subagent node vs function node

Subagent nodes and [function nodes](/build/conversation-flow/function-node) serve different purposes:

|                    | Function node                                                  | Subagent node                                                                           |
| ------------------ | -------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Execution**      | Deterministic — executes on node entry                         | LLM-driven — called when the LLM decides it's appropriate                               |
| **Tools per node** | One                                                            | Multiple                                                                                |
| **Conversation**   | Not intended for dialogue                                      | Full dialogue with tools available                                                      |
| **Best for**       | Always-execute actions (e.g. always look up an order on entry) | Context-dependent actions during dialogue (e.g. look up an order only if the user asks) |

**Use function nodes** when you want guaranteed execution every time the flow reaches that step.

**Use subagent nodes** when the agent should decide whether and when to call a tool based on what the user says.

## Add tools

<Steps>
  <Step title="Select a subagent node">
    Click a subagent node to open its settings panel on the right side.
  </Step>

  <Step title="Add a tool">
    In the settings panel, find the **Tools** section and click **+ Add**, then select the tool type from the dropdown.
  </Step>

  <Step title="Configure the tool">
    Configure the tool based on its type — see the table below for each type's configuration guide.
  </Step>

  <Step title="Update the node instruction">
    Update the node prompt to guide the LLM on when to use the tool.
  </Step>
</Steps>

## Available tool types

| Tool type                   | Description                                             | Configuration guide                                               |
| --------------------------- | ------------------------------------------------------- | ----------------------------------------------------------------- |
| Custom Function             | Make HTTP requests to your external APIs                | [Custom Function](/build/conversation-flow/custom-function)       |
| Code Tool                   | Run JavaScript code directly without an external server | [Code Tool](/build/single-multi-prompt/code-tool)                 |
| Check Calendar Availability | Query available time slots via Cal.com                  | [Check Availability](/build/check-availability)                   |
| Book Appointment            | Book calendar events via Cal.com                        | [Book Calendar](/build/book-calendar)                             |
| End Call                    | Terminate the call                                      | [End Call](/build/single-multi-prompt/end-call)                   |
| Transfer Call               | Transfer to a phone number                              | [Transfer Call](/build/single-multi-prompt/transfer-call)         |
| Transfer Agent              | Transfer to another Retell agent                        | [Transfer Agent](/build/single-multi-prompt/transfer-agent)       |
| Press Digit                 | Send DTMF tones                                         | [Press Digit](/build/single-multi-prompt/press-digit)             |
| Send SMS                    | Send a text message                                     | [Send SMS](/build/single-multi-prompt/send-sms)                   |
| Extract Dynamic Variable    | Extract variables from the conversation                 | [Extract Dynamic Variable](/build/single-multi-prompt/extract-dv) |
| MCP Tool                    | Call tools on your MCP server                           | [MCP Node](/build/conversation-flow/mcp-node)                     |

## Execution speech settings

Each tool has settings that control what the agent says while it runs and after it completes.

### Speak During Execution

When enabled, the agent says a message while the tool is executing, for example `One moment, let me check that for you.` This is recommended when the tool takes over 1 second, including network latency, so the agent remains responsive.

You can configure how the message is generated:

* **Prompt**: The LLM dynamically generates what to say based on a description you provide.
* **Static Sentence**: The agent speaks the exact text you provide.

### Speak After Execution

When enabled (the default), the agent calls the LLM after the tool returns a result so it can speak about the outcome to the user. Turn it off to run the tool silently.

<Note>
  * **Speak During Execution** is available on: Custom Function, Code Tool, End Call, Transfer Call, Transfer Agent, Send SMS, and MCP Tool.
  * **Speak After Execution** is available on: Custom Function, Code Tool, and MCP Tool.
</Note>

## When transitions happen

* After the user finishes speaking, following the [evaluation order](/build/conversation-flow/transition-condition#evaluation-order).
* When **Skip Response** is enabled: as soon as the agent finishes speaking.

Tool execution happens within the subagent node, so the node can stay active across multiple turns and tool calls before it transitions.

## Node settings

* **Tools**: Attach the tools this subagent can use during the conversation.
* **Skip Response**: The node gets a single edge and transitions through it when the agent finishes speaking, without waiting for a reply.
* **Knowledge Base**: Attach node-level knowledge bases to combine topic-specific knowledge with the agent-level knowledge base. Read more at [knowledge base](/build/knowledge-base).
* **Global Node**: Make this node reachable from anywhere in the flow when its condition is met. Read more at [global node](/build/conversation-flow/global-node).
* **LLM**: Choose a different model for this node only. It's used for response generation, tool selection, and tool argument generation.
* **Speech overrides**: Override the agent-level speech settings for this node only — interruption sensitivity (0–1), responsiveness (0–1), voice speed (0.5–2), and whether keypad presses can interrupt the agent (DTMF interruption).
* **Fine-tuning examples**: Add example transcripts to improve this node's responses and transition decisions. Read more at [finetune examples](/build/conversation-flow/finetune-examples).

## Best practices

* **Be explicit in your node instruction.** Tell the agent when each tool should be used.
* **Use function nodes for guaranteed execution.** If a tool must always run at a certain point in the flow, use a [function node](/build/conversation-flow/function-node) instead.
* **Avoid stacking too many tools on one subagent node.** There's no hard limit, but the more tools the LLM can choose from, the more likely it picks the wrong one. Split them across multiple subagent nodes.
