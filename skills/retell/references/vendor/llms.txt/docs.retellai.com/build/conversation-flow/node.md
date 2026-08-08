> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Nodes and edges

> Nodes are the building blocks of Retell conversation flows. Learn every node type, how edges connect them, and how transition conditions advance calls.

A conversation flow is a graph of **nodes** connected by **edges**. Each node handles one step of the call — talking with the user, running a tool, branching on data, transferring, or ending the call. Each edge carries a [transition condition](/build/conversation-flow/transition-condition) that decides when the agent moves to the next node.

Breaking the conversation into nodes lets you control, fine-tune, and debug each step independently — you can change one part of the flow without touching the rest.

## Pick a node type

Each node type plays a specific role. Pick the type that matches what the step needs to do.

### Dialogue nodes

* [**Conversation Node**](/build/conversation-flow/conversation-node): Pure dialogue with the user — no tool calling. The agent can hold a multi-turn conversation within a single node, so you don't need a new node for every line. Supports two instruction modes: **Prompt** (the LLM generates responses dynamically) and **Static Sentence** (the agent says a fixed line first, then continues dynamically if the conversation stays in the node). Transitions are evaluated after each user response.
* [**Subagent Node**](/build/conversation-flow/subagent-node): Dialogue where the agent can also call tools/functions based on context. Unlike a function node (which executes deterministically on entry), the LLM decides **whether and when** to invoke each tool from what the user says. Supports multiple tools per node and stays active across multiple turns and tool calls before transitioning. Use a function node instead when a tool must always run at that step.
* [**Extract DV Node**](/build/conversation-flow/extract-dv-node): Extracts information from the conversation so far and stores it as dynamic variables on entry — not for dialogue. The LLM analyzes the full conversation to capture each value you define by name, description, and type (Text, Number, Enum, or Boolean). Useful for structured data that downstream nodes or post-call analysis need.

### Action nodes

* [**Function Node**](/build/conversation-flow/function-node): Executes a single tool/function deterministically on node entry — not for dialogue. Turn on **Wait for Result** when the next hop depends on the return value, then branch on the outcome directly from this node. The agent can optionally speak while it runs (e.g. "Let me check that for you").
* [**Code Node**](/build/conversation-flow/code-node): Runs JavaScript in Retell's sandbox on entry — no external server needed. Best for data transformation, calculations, formatting, and simple read-only lookups via `fetch()`. Has access to dynamic variables and call metadata, and can store return values into dynamic variables. For anything needing secrets, authentication, or writes, use a custom function instead.
* [**SMS Node**](/build/conversation-flow/sms-node): Sends an SMS during an active phone call, to the caller or another number. Requires an SMS-enabled or SMS-approved Retell number. Transitions once the SMS succeeds or fails. *(Voice agents only.)*
* [**MCP Node**](/build/conversation-flow/mcp-node): Calls a tool on an external MCP (Model Context Protocol) server on entry. Connect your server, select a tool, and optionally extract response values into dynamic variables for later nodes. The agent can speak while it runs.

### Call control nodes

* [**Call Transfer Node**](/build/conversation-flow/call-transfer-node): Transfers the call to another phone number or SIP URI. The agent doesn't speak in this node — put a conversation node with **Skip Response** before it for a line like "Let me transfer you." Supports cold, warm (with human detection, whisper/three-way messages), and agentic warm transfer. Use this node — not a conversation node — whenever the agent should transfer to a human. *(Voice agents only.)*
* [**Transfer Agent Node**](/build/conversation-flow/transfer-agent-node): Hands the conversation to a different Retell agent mid-call. Near-instant (no new phone call), the destination agent inherits the full conversation history, and no separate phone number is needed. Preferred over call transfer when routing between AI agents (e.g. front-desk → booking, or language-based routing).
* [**Press Digit Node**](/build/conversation-flow/press-digit-node): Navigates IVR menus by sending DTMF tones. The agent doesn't speak — it listens to IVR prompts and infers which digit to press from your instruction, evaluating on each IVR utterance. Give clear guidance on which options to choose and which to avoid. *(Voice agents only.)*
* [**End Node**](/build/conversation-flow/end-node): Ends the call. Enable **Speak During Execution** with an instruction so the agent gives a closing message before hanging up; otherwise the call ends abruptly.

### Logic and structure

* [**Logic Split Node**](/build/conversation-flow/logic-split-node): Evaluates conditions and branches the flow immediately on entry — the agent doesn't speak. Useful for splitting on dynamic variables or conditions without stacking everything onto the previous node. Always has an else destination as a fallback so the flow never gets stuck.
* [**Subflow**](/build/conversation-flow/components): Packages a reusable subflow (a group of nodes) that appears as a single node on the main canvas. Subflows can be local to one agent or shared across agents in a library; they run their internal nodes and return control to the main flow via an exit node. Tools defined inside a subflow stay scoped to it.

## How nodes connect

Every node except the end node moves the call forward through edges. A node can have several kinds:

* **Regular edges** each carry a transition condition — a prompt the LLM evaluates, or an equation over dynamic variables — plus a destination node.
* **Else edge** is the fallback: it fires when no other condition matches, so the flow never gets stuck.
* **Always edge** transitions unconditionally after the user responds.
* **Skip Response edge** appears when the node's Skip Response setting is on: the node transitions as soon as the agent finishes speaking, without waiting for a reply.

How conditions are written, and the exact order they're evaluated in, is covered in [transition conditions](/build/conversation-flow/transition-condition).

## Add a node

<Steps>
  <Step title="Select node type">
    Click a node type in the left sidebar to add it to the canvas.

    <Frame caption="The node type list in the flow editor's left sidebar.">
      <img src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/cf/add-node.png?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=9f943a03432352f72166c04f780e19ba" alt="Left sidebar of the conversation flow editor showing the list of node types available to add to the canvas" width="298" height="722" data-path="images/cf/add-node.png" />
    </Frame>
  </Step>

  <Step title="Configure the node">
    Click the node to open its settings on the right, and fill in the node instruction inside the node. See the guide for each node type for details.

    <Frame caption="Node settings open on the right after selecting a node.">
      <img src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/cf/configure-node.jpeg?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=217edcea975d2e0578e385ba4fb5df50" alt="Conversation flow editor with a node selected, showing its instruction field and the settings panel on the right" width="736" height="567" data-path="images/cf/configure-node.jpeg" />
    </Frame>
  </Step>

  <Step title="Add transition conditions">
    Click the bottom part of the node to add edges, then write a [transition condition](/build/conversation-flow/transition-condition) for each.
  </Step>

  <Step title="Connect node">
    Click and hold the circle to start a line that connects the node to another node, and another node to this node.

    <Frame caption="Drag from the connector circle to link two nodes.">
      <img src="https://mintcdn.com/retellai/YiYvhov2NLXI5cg3/images/cf/connect-node.png?fit=max&auto=format&n=YiYvhov2NLXI5cg3&q=85&s=fdd6ce5feda5eeba19878c61fe496bb0" alt="Dragging from a node's connector circle to another node to create an edge in the conversation flow editor" width="914" height="874" data-path="images/cf/connect-node.png" />
    </Frame>
  </Step>
</Steps>

## Organize nodes

After adding many nodes, the canvas can get cluttered. Use the **Organize** button to arrange them automatically.

<Frame caption="The Organize button rearranges nodes automatically.">
  <img src="https://mintcdn.com/retellai/AJT6JQMM1II9WOl-/images/cf/organize-node.png?fit=max&auto=format&n=AJT6JQMM1II9WOl-&q=85&s=d55c57293e75058c2336a9eea99c43d7" alt="Organize button in the conversation flow editor that automatically arranges nodes on the canvas" width="2354" height="1674" data-path="images/cf/organize-node.png" />
</Frame>

<Tip>
  Name your nodes. Call transcripts in the history tab show transitions by node name, so descriptive names make past calls much easier to debug.
</Tip>

## Copy and paste nodes

You can copy nodes and paste them elsewhere on the canvas, into another flow, or even into a different browser tab. Retell uses your system clipboard, so a copy stays available across tabs and windows.

<Steps>
  <Step title="Select what to copy">
    Click a node, or click and drag to select multiple nodes and sticky notes.
  </Step>

  <Step title="Copy">
    Press `Cmd/Ctrl + C`. A **Copied** confirmation shows how many items were copied. Any selected sticky notes, and the tools attached to the selected nodes, are copied along with them.
  </Step>

  <Step title="Paste">
    Move your cursor to where you want the items to appear and press `Cmd/Ctrl + V`. A **Pasted** confirmation appears, and the nodes, notes, and tools are added at your cursor, automatically positioned to avoid overlapping existing nodes.
  </Step>
</Steps>

A few things to keep in mind:

* If a copied tool already exists in the destination, it isn't added again — the pasted node reuses the existing tool.
* Component nodes can't be pasted inside a component editor. If you try, Retell shows a **Cannot Paste** message.

## FAQ

<AccordionGroup>
  <Accordion title="When should I break down a node?">
    Consider breaking down a node when:

    * The node handles multiple complex logic paths
    * The LLM struggles with consistency (hallucinations or incorrect responses)
    * You need different settings (model, temperature) for different parts
    * The conversation flow becomes hard to follow or debug

    Breaking complex nodes into smaller, focused nodes often improves reliability.
  </Accordion>

  <Accordion title="How do I zoom the canvas in and out?">
    Use the scroll wheel on a mouse, or pinch on a touchpad.
  </Accordion>

  <Accordion title="Is there a limit on the number of nodes?">
    No, you can add as many nodes as you want.
  </Accordion>
</AccordionGroup>
