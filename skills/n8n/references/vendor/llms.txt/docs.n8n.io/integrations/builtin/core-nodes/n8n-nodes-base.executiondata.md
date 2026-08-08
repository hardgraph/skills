> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.executiondata.md).

# Execution Data

Use this node to save metadata for workflow executions. You can then search by this data in the **Executions** list.

You can retrieve custom execution data during workflow execution using the Code node. Refer to [Custom executions data](/build/understand-workflows/understand-executions/customize-executions-data.md) for more information.

{% hint style="info" %}
**Feature availability**

Custom executions data is available on:

* Cloud: Pro, Enterprise
* Self-Hosted: Enterprise, registered Community
  {% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Save Execution Data for Search

## Data to Save <a href="#data-to-save" id="data-to-save"></a>

Add a **Saved Field** for each key/value pair of metadata you'd like to save.

## Limitations <a href="#limitations" id="limitations"></a>

The Execution Data node has the following restrictions when storing execution metadata:

* `key`: limited to 50 characters
* `value`: limited to 512 characters

If either the `key` or `value` exceed the above limitations, n8n truncates to their maximum length and outputs a log entry.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Execution Data integration templates](https://n8n.io/integrations/execution-data) or [search all templates](https://n8n.io/workflows/)
