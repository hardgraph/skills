> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Global Node

> Global nodes in a Retell conversation flow can be reached from anywhere in the agent — ideal for handling universal intents like callbacks and human handoff.

Global nodes can be transitioned to from anywhere in the conversation flow, making them ideal for handling universal scenarios like user objections (e.g. `I want to talk to a human / I need to call back later`). Toggle on `Global Node` in the node settings to enable this.

<img src="https://mintcdn.com/retellai/vQCQvVJr44vBuU_v/images/cf/global-node.png?fit=max&auto=format&n=vQCQvVJr44vBuU_v&q=85&s=bf50a33bb1176d166d3da91c48dbcb38" width="1712" height="1340" data-path="images/cf/global-node.png" />

## Configure Global Node

Set a condition for when the global node should be transitioned to. In the example above, the condition is `When user indicates this is not a good time to continue` — so whenever the user says something like `I need to call back later`, the agent transitions to this node.

Since a global node can be transitioned to from anywhere, it does not need to be connected to the rest of the graph.

## Global Node Examples

Add example conversations to help the AI better understand when to jump to this global node. Click `+ Add` to create examples, then set the type to `Jump to global node` or `Not jump to global node` to demonstrate scenarios where the global node should or should not be activated.

Global nodes are only evaluated after a user turn, so each example should end with a user utterance. New examples default to an agent turn followed by a user turn for this reason. See [Finetune Examples](/build/conversation-flow/finetune-examples) for how transcript examples work.

## Go Back to Previous Node

Enable **Go back to previous node** to let the conversation return to where it left off after the global node is handled. Once enabled, a **Go Back Condition** section appears on the node where you define when the agent should navigate back. In the example above, the condition is `User changed their mind and wants to continue the call`.

Go back conditions support both prompt-based and equation-based conditions. You can add multiple conditions and reorder them by dragging.

## Prevent Immediate Re-Trigger

Enable **Prevent Immediate Re-Trigger** to pause the global node for a specified number of node steps after it has been triggered. Set the number of **Node steps** (defaults to 3) during which the global node will not be activated again. This prevents the conversation from looping when the user's phrasing keeps matching the global node condition.
