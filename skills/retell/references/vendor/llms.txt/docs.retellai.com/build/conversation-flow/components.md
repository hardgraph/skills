> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Reusable subflows for conversation flow agents

> Package parts of a conversation flow into reusable Retell subflows, so you can build complex agents once and share consistent logic across many flows.

## Conversation flow subflows

Make complex agents easier to build, reuse, and maintain by packaging parts of your conversation into subflows. A subflow is a mini flow (a group of nodes) that you can reuse across agents and flows.

<Note>
  Subflows were previously called **Components**. The feature is the same; only the name changed.
</Note>

### Why use subflows?

* Reuse: Build once, drop into many agents and flows.
* Consistency: Keep behavior uniform across use cases (e.g., identity check).
* Clean canvas: Hide detailed logic inside a focused subflow.
* Faster iteration: Update a shared subflow to improve every agent that uses it.

### Where to find it

<Frame>
  <img height="700" src="https://mintcdn.com/retellai/PP8J4G_S-0bVCFYm/images/cf/component-sidebar.png?fit=max&auto=format&n=PP8J4G_S-0bVCFYm&q=85&s=38d4dec599f6cb9bbc59808a4a779812" alt="Conversation flow builder left sidebar with the Subflows tab open, showing the Library Subflows and Agent Subflows sections." data-path="images/cf/component-sidebar.png" />
</Frame>

In the dashboard, open your agent's builder:

* Left sidebar → **Subflows** tab
* Two sections:
  * **Library Subflows**: Account-level, shared across agents.
  * **Agent Subflows**: Local to the current agent.

<Note>
  **Switching between flows:** Use the flow selector at the top of the builder (it shows **Main flow** by default) to move between your main agent, any open subflows, and transfer agents. Each one opens in its own editor tab, and you can return to **Main flow** at any time.
</Note>

## Create a subflow

You can create either a library (shared) subflow or an agent-level (local) one. Both open in a dedicated editor tab.

1. In the **Subflows** tab, click the **+** button next to **Library Subflows** or **Agent Subflows**.
2. You'll start with a "Begin" node, a basic conversation node, and an "Exit Subflow" end node.
3. Add nodes and connect edges to form your subflow.
4. Set a start node by connecting the Begin tag to the first node.
5. Link the "Exit Subflow" node correctly so you won't get stuck inside the subflow.
6. Switch back to the main agent using the flow selector (labeled **Main flow**) at the top of the builder.
7. Rename the subflow by clicking the "..." to the right of its name.

Notes:

* Subflows cannot contain other subflows; you add regular nodes inside a subflow.
* Available node types match your agent's channel (voice vs chat).

<Frame>
  <img height="700" src="https://mintcdn.com/retellai/I1roxGSvtapAS92i/images/cf/component-detail.png?fit=max&auto=format&n=I1roxGSvtapAS92i&q=85&s=23463311fdf2ec896c9cff37222c20f6" alt="Subflow editor tab with a Begin node, a conversation node, and an Exit Subflow end node connected on the canvas." data-path="images/cf/component-detail.png" />
</Frame>

## Add a subflow to your flow

* From the **Subflows** tab, click a subflow. A single subflow node appears on your canvas.
* Connect into the subflow: link any node to the subflow node.
* Connect out of the subflow: select the subflow node and connect its outgoing edge to where the conversation should continue.
* To edit what happens inside, open the subflow's editor tab and modify its internal nodes.
* You can rename the subflow node, which is also reflected in the subflow.

Tip: End nodes inside the subflow hand control back to the main flow. Back on the main canvas, make sure the subflow node's outgoing edge points to the next step.

<Frame>
  <img height="700" src="https://mintcdn.com/retellai/I1roxGSvtapAS92i/images/cf/component-node.png?fit=max&auto=format&n=I1roxGSvtapAS92i&q=85&s=feee9405abd9874531c4a1fa8d7606bc" alt="Main conversation flow canvas with a single subflow node whose outgoing edge connects to the next node." data-path="images/cf/component-node.png" />
</Frame>

## Shared vs local subflows

Every subflow is either shared across your account or local to a single agent. That choice decides who your next edit reaches.

|                         | Library Subflow (shared)                                                                   | Agent Subflow (local)                     |
| ----------------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------- |
| Where it's stored       | Its own account-level record                                                               | Inside the agent's flow                   |
| Which agents can use it | Any agent in the account                                                                   | Only the agent it was created in          |
| Who an edit reaches     | Every agent that uses it, published versions included                                      | That agent only                           |
| Best for                | Steps that should stay identical everywhere, like an identity check or a compliance script | Logic that only makes sense for one agent |

### Convert between shared and local

Select the subflow node, open its **Subflow Settings** panel, then set **Subflow Access** to **Agent Only Subflow** or **Shared Library Subflow**.

<Frame>
  <img height="700" src="https://mintcdn.com/retellai/I1roxGSvtapAS92i/images/cf/component-convert.png?fit=max&auto=format&n=I1roxGSvtapAS92i&q=85&s=543c56f4af5c345362820ea001b9f485" alt="Subflow Settings panel with the Subflow Access radio group showing Agent Only Subflow and Shared Library Subflow options." data-path="images/cf/component-convert.png" />
</Frame>

Going from shared to local, which is the same action as turning off **Sync Updates**, copies the subflow into this agent and stops it from receiving library updates.

### What to watch for

* **Deleting a library subflow** turns every linked instance into a local copy and stops sync updates, so agents that used it keep working.
* **Published agents can reference shared subflows.** The behavior of a [published agent](/agent/version) can still change if it uses a subflow that gets updated. To prevent this, convert to a local subflow before publishing.

## Testing

* To test a subflow under the Test Subflow panel, add the subflow to the main conversation flow first so it initializes properly.
* The global prompt of the main conversation flow applies to all subflow nodes implicitly.
* To test a subflow alone, make it a shared subflow and create a new empty agent with only one subflow node.

## Best practices

* Keep subflows focused: One clear job (e.g., "Collect Shipping Address").
* Name clearly: Use action + outcome (e.g., "Verify Identity").
* Design clean entry/exit: Always set a start node; include an end node to exit cleanly.
* Reuse variables: Use dynamic variables to pass captured data back to the main flow.
* Test in context: Open the Test panel to simulate end-to-end behavior after inserting the subflow.

## FAQ

* How do I update a shared subflow used by many agents?
  * Under any agent, navigate to the subflow's edit page. When you edit it, changes apply everywhere it's used.

* Can I stop changes from affecting an agent?
  * Yes. In that agent, turn off **Sync Updates** to convert its reference to a local copy.

* What happens if I delete a library subflow?
  * Agents keep a local copy; they stop syncing with the deleted library item.

* Can I move a local subflow into the library?
  * Yes. In **Subflow Access**, switch it to **Shared Library Subflow** to create a shared version and update references.

* Can subflows include tools/functions?
  * Yes. Subflows can include function nodes and use your configured tools. Tools behave the same as in the main flow.
  * The tools need to be defined within the subflow and won't be visible outside at the agent level.

* What if I did not link the Begin node in a subflow?
  * It transitions to the next node based on the subflow node edges.

* What if I did not link the Exit Subflow node properly?
  * It stays stuck inside the subflow and cannot transition out.
