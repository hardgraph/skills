> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.workflowtrigger.md).

# Workflow Trigger

The Workflow Trigger node gets triggered when a workflow is updated or activated.

{% hint style="warning" %}
**Deprecated**

n8n has deprecated the Workflow Trigger node and moved its functionality to the [n8n Trigger node](/integrations/builtin/core-nodes/n8n-nodes-base.n8ntrigger.md).
{% endhint %}

{% hint style="info" %}
**Keep in mind**

If you want to use the Workflow Trigger node for a workflow, add the node to the workflow. You don't have to create a separate workflow.
{% endhint %}

The Workflow Trigger node gets triggered for the workflow that it gets added to. You can use the Workflow Trigger node to trigger a workflow to notify the state of the workflow.

## Node parameters <a href="#node-parameters" id="node-parameters"></a>

The node includes a single parameter to identify the **Events** that should trigger it. Choose from these events:

* **Active Workflow Updated**: If you select this event, the node triggers when this workflow is updated.
* **Workflow Activated**: If you select this event, the node triggers when this workflow is activated.

You can select one or both of these events.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Workflow Trigger node documentation integration templates](https://n8n.io/integrations/workflow-trigger) or [search all templates](https://n8n.io/workflows/)
