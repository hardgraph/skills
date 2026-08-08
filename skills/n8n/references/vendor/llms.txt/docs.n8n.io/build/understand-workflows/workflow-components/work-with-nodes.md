> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/understand-workflows/workflow-components/work-with-nodes.md).

# Work with nodes

Nodes[^1] are the key building blocks of a workflow[^2]. They perform a range of actions, including:

* Starting the workflow.
* Fetching and sending data.
* Processing and manipulating data.

n8n provides a collection of built-in nodes, as well as the ability to create your own nodes. Refer to:

* [Built-in integrations](/integrations/builtin/node-types.md) to browse the node library.
* [Community nodes](/integrations/community-nodes/installation-and-management.md) for guidance on finding and installing community-created nodes.
* [Creating nodes](/connect/create-nodes/overview.md) to start building your own nodes.

## Add a node to your workflow <a href="#add-a-node-to-your-workflow" id="add-a-node-to-your-workflow"></a>

### Add a node to an empty workflow <a href="#add-a-node-to-an-empty-workflow" id="add-a-node-to-an-empty-workflow"></a>

1. Select **Add first step**. n8n opens the nodes panel, where you can search or browse [trigger nodes](#user-content-fn-3)[^3].
2. Select the trigger you want to use.<br>

   <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p><strong>Choose the correct app event</strong></p><p>If you select <strong>On App Event</strong>, n8n shows a list of all the supported services. Use this list to browse n8n's integrations and trigger a workflow in response to an event in your chosen service. Not all integrations have triggers. To see which ones you can use as a trigger, select the node. If a trigger is available, you'll see it at the top of the available operations list.</p><p>For example, this is the trigger for Asana:</p><p><img src="/files/e6YAn8E02yTCOmzuTnkt" alt="Screenshot of the Asana node operations list, showing the Recommended section at the top of the list" data-size="original"></p></div>

### Add a node to an existing workflow <a href="#add-a-node-to-an-existing-workflow" id="add-a-node-to-an-existing-workflow"></a>

Select the **Add node** <img src="/files/eII6Nivmyx7TGIO7AXky" alt="Add node icon" data-size="line"> connector. n8n opens the nodes panel, where you can search or browse all nodes.

### Node operations: Triggers and Actions

When you add a node to a workflow, n8n displays a list of available operations. An operation is something a node does, such as getting or sending data.

There are two types of operation:

* Triggers start a workflow in response to specific events or conditions in your services. When you select a Trigger, n8n adds a trigger node to your workflow, with the Trigger operation you chose pre-selected. When you search for a node in n8n, Trigger operations have a bolt icon <img src="/spaces/GixZThfitWP21x2gQFpD/files/YhXfixCZho94eabo1r6o" alt="Trigger icon" data-size="line">.
* Actions are operations that represent specific tasks within a workflow, which you can use to manipulate data, perform operations on external systems, and trigger events in other systems as part of your workflows. When you select an Action, n8n adds a node to your workflow, with the Action operation you chose pre-selected.

## Node controls <a href="#node-controls" id="node-controls"></a>

To view node controls, hover over the node on the canvas:

* **Execute step** <img src="/files/sZCug6yUQVmzMj2ERcFV" alt="Execute step icon" data-size="line">: Run the node.
* **Deactivate** <img src="/files/31scueK8Bn7yKCQNqFkb" alt="Deactivate node icon" data-size="line">: Deactivate the node.
* **Delete** <img src="/files/hltlsv637aP7cSnyiURR" alt="Delete node icon" data-size="line">: Delete the node.
* **Node context menu** <img src="/files/SXI6SdB9PXxEr377KzgC" alt="Node context menu icon" data-size="line">: Select node actions. Available actions:
  * Open node
  * Execute step
  * Rename node
  * Deactivate node
  * Pin node
  * Copy node
  * Duplicate node
  * Tidy up workflow
  * Convert node to sub-workflow
  * Select all
  * Clear selection
  * Delete node

## Node settings <a href="#node-settings" id="node-settings"></a>

The node settings under the **Settings** tab allow you to control node behaviors and add node notes.

When active or set, they do the following:

* **Always Output Data**: The node returns an empty item even if the node returns no data during execution. Be careful setting this on IF nodes, as it could cause an infinite loop.
* **Execute Once**: The node executes once, with data from the first item it receives. It doesn't process any extra items.
* **Retry On Fail**: When an execution fails, the node reruns until it succeeds.
* **On Error**:
  * **Stop Workflow**: Halts the entire workflow when an error occurs, preventing further node execution.
  * **Continue**: Proceeds to the next node despite the error, using the last valid data.
  * **Continue (using error output)**: Continues workflow execution, passing error information to the next node for potential handling.
* **Custom Span Attributes**: Add custom key-value attributes to a node's OpenTelemetry span. Keys are plain text, and values support expressions. This setting only appears if you enable OpenTelemetry tracing and have an Enterprise license. Refer to [Custom span attributes](/deploy/host-n8n/keep-n8n-running/trace-executions-with-opentelemetry.md#custom-span-attributes) for details.

You can document your workflow using node notes:

* **Notes**: Note to save with the node.
* **Display note in flow**: If active, n8n displays the note in the workflow as a subtitle.

[^1]: In n8n, nodes are individual components that you compose to create workflows. Nodes define when the workflow should run, allow you to fetch, send, and process data, can define flow control logic, and connect with external services.

[^2]: An n8n workflow is a collection of nodes that automate a process. Workflows begin execution when a trigger condition occurs and execute sequentially to achieve complex tasks.

[^3]: A trigger node is a special node responsible for executing the workflow in response to certain conditions. All production workflows need at least one trigger to determine when the workflow should run.
