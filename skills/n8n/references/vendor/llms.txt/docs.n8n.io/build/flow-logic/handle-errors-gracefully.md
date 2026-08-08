> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/flow-logic/handle-errors-gracefully.md).

# Handle errors gracefully

When designing your flow logic, it's a good practice to consider potential errors, and set up methods to handle them gracefully. With an error workflow, you can control how n8n responds to a workflow execution failure.

{% hint style="info" %}
**Investigating errors**

To investigate failed executions, you can:

* Review your [Executions](/build/understand-workflows/understand-executions.md), for a [single workflow](/build/understand-workflows/understand-executions/view-executions-for-a-single-workflow.md) or [all workflows you have access to](/build/understand-workflows/understand-executions/view-all-executions.md). You can [load data from previous execution](/build/understand-workflows/understand-executions/debug-executions.md) into your current workflow.
* Enable [Log streaming](/administer/observe-and-log/stream-logs-to-external-systems.md).
  {% endhint %}

## How do I set up an error workflow? <a href="#create-and-set-an-error-workflow" id="create-and-set-an-error-workflow"></a>

For each workflow, you can set an error workflow in **Workflow Settings**. It runs if an execution fails. This means you can, for example, send email or Slack alerts when a workflow execution errors. The error workflow must start with the [Error Trigger](/integrations/builtin/core-nodes/n8n-nodes-base.errortrigger.md).

You can use the same error workflow for multiple workflows.

1. Create a new workflow, with the Error Trigger as the first node.
2. Give the workflow a name, for example `Error Handler`.
3. Select **Save**.
4. In the workflow where you want to use this error workflow:
   1. Select **Options** <img src="/spaces/GixZThfitWP21x2gQFpD/files/LxEoziLuMlenJM8dOV4r" alt="Options menu icon" data-size="line"> > **Settings**.
   2. In **Error workflow**, select the workflow you just created. For example, if you used the name Error Handler, select **Error handler**.
   3. Select **Save**. Now, when this workflow errors, the related error workflow runs.

## What data does an error workflow receive? <a href="#error-data" id="error-data"></a>

The default error data received by the Error Trigger is:

```json
[
	{
		"execution": {
			"id": "231",
			"url": "https://n8n.example.com/execution/231",
			"retryOf": "34",
			"error": {
				"message": "Example Error Message",
				"stack": "Stacktrace"
			},
			"lastNodeExecuted": "Node With Error",
			"mode": "manual"
		},
		"workflow": {
			"id": "1",
			"name": "Example Workflow"
		}
	}
]

```

All information is always present, except:

* `execution.id`: requires the execution to be saved in the database. Not present if the error is in the trigger node of the main workflow, as the workflow doesn't execute.
* `execution.url`: requires the execution to be saved in the database. Not present if the error is in the trigger node of the main workflow, as the workflow doesn't execute.
* `execution.retryOf`: only present when the execution is a retry of a failed execution.

If the error is caused by the trigger node of the main workflow, rather than a later stage, the data sent to the error workflow is different. There's less information in `execution{}` and more in `trigger{}`:

```json
{
  "trigger": {
    "error": {
      "context": {},
      "name": "WorkflowActivationError",
      "cause": {
        "message": "",
        "stack": ""
      },
      "timestamp": 1654609328787,
      "message": "",
      "node": {
        . . . 
      }
    },
    "mode": "trigger"
  },
  "workflow": {
    "id": "",
    "name": ""
  }
}
```

## How do I make a workflow fail on purpose with the Stop and Error node? <a href="#cause-a-workflow-execution-failure-using-stop-and-error" id="cause-a-workflow-execution-failure-using-stop-and-error"></a>

When you create and set an error workflow, n8n runs it when an execution fails. Usually, this is due to things like errors in node settings, or the workflow running out of memory.

You can add the [Stop And Error](/integrations/builtin/core-nodes/n8n-nodes-base.stopanderror.md) node to your workflow to force executions to fail under your chosen circumstances, and trigger the error workflow.
