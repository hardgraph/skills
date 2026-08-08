> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Conversation node

> Use Retell conversation nodes for multi-turn dialogue without tool calls — the default building block for capturing info and guiding callers through a flow.

The conversation node is the most common node type in a conversation flow. It holds a spoken conversation with the user — no tool calling.

If the agent should also call tools during the same dialogue, use a [subagent node](/build/conversation-flow/subagent-node). If a tool must always run at a fixed point in the flow, use a [function node](/build/conversation-flow/function-node).

Please note that the agent can have a multi-turn conversation inside a single node, so you don't necessarily need to create a new conversation node for every sentence the agent needs to say. It's recommended to split the node when there's a logic split, or the instruction gets too long.

<Frame caption="A conversation node with its instruction and outgoing transition conditions.">
  <img src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/cf/conversation-node.jpeg?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=2a67c7583585883d1bec9fdcef6b3243" alt="Conversation node on the flow canvas showing an instruction prompt and edges with transition conditions below it" width="800" height="504" data-path="images/cf/conversation-node.jpeg" />
</Frame>

For example, an appointment-booking agent can use one conversation node to collect the caller's preferred date and time — asking follow-up questions across several turns — with transition conditions for "user provided a date and time" and "user asked to speak to a human."

## Write the instruction

Pick how the agent generates what to say in this node:

* **Prompt**: Write instructions and the LLM generates responses dynamically. Use this for anything that depends on what the user says.
* **Static Sentence**: The agent says your exact sentence first. If the conversation stays in the node after that, it continues dynamically based on the static sentence. Use this when the wording must be exact, like disclaimers or greetings.

## When transitions happen

* After the user finishes speaking, following the [evaluation order](/build/conversation-flow/transition-condition#evaluation-order).
* When **Skip Response** is enabled: as soon as the agent finishes speaking.

## Node settings

* **Skip Response**: The node gets a single edge and transitions through it when the agent finishes speaking, without waiting for a reply. Useful for lines that need no response, like a disclaimer before a transfer.
* **Knowledge Base**: Attach node-level knowledge bases to combine topic-specific knowledge with the agent-level knowledge base. Read more at [knowledge base](/build/knowledge-base).
* **Global Node**: Make this node reachable from anywhere in the flow when its condition is met. Read more at [global node](/build/conversation-flow/global-node).
* **LLM**: Choose a different model for this node only. It's used for response generation — for example, a cheaper model for simple routing and a stronger one for complex steps.
* **Speech overrides**: Override the agent-level speech settings for this node only — interruption sensitivity (0–1), responsiveness (0–1), voice speed (0.5–2), and whether keypad presses can interrupt the agent (DTMF interruption).
* **Fine-tuning examples**: Add example transcripts to improve this node's responses and transition decisions. Read more at [finetune examples](/build/conversation-flow/finetune-examples).
