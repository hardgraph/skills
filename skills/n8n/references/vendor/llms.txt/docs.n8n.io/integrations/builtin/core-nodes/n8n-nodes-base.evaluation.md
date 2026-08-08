> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.evaluation.md).

# Evaluation

The Evaluation node performs various operations related to [evaluations](/build/integrate-ai/test-and-improve-ai-workflows/understand-why-to-test.md) to validate your AI workflow reliability.

Use the Evaluation node in these scenarios:

* To conditionally execute logic based on whether the workflow is under evaluation
* To write evaluation outcomes back to a Google Sheet datasetor
* To log scoring metrics for your evaluation performance to n8n's evaluations tab

{% hint style="info" %}
**Credentials for Google Sheets**

The Evaluation node's **Set Outputs** operation records evaluation results to data tables or Google Sheets. To use Google Sheets as a recording location, configure a [Google Sheets credential](/integrations/builtin/credentials/google.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

The Evaluation node offers the following operations:

* [**Set Outputs**](#set-outputs): Write the results of an evaluation back to a data table or Google Sheet dataset.
* [**Set Metrics**](#set-metrics): Record metrics scoring the evaluation performance to n8n's **Evaluations** tab.
* [**Check If Evaluating**](#check-if-evaluating): Branches the workflow execution logic depending on whether the current execution is an evaluation.

The parameters and options available depend on the operation you select.

### Set Outputs <a href="#set-outputs" id="set-outputs"></a>

The **Set Outputs** operation has the following parameters:

* **Source:** Select the location to which you want to output the evaluation results. Default value is **Data table**.

  Source settings differ depending on **Source** selection.

  * When **Source** is **Data table**:
    * **Data table:** Select a data table by name or ID
  * When **Source** is **Google Sheets**:
    * **Credential to connect with**: Create or select an existing [Google Sheets credentials](/integrations/builtin/credentials/google.md).
    * **Document Containing Dataset**: Choose the spreadsheet document you want to write the evaluation results to. Usually this is the same document you select in the [Evaluation Trigger](/integrations/builtin/core-nodes/n8n-nodes-base.evaluationtrigger.md) node.
    * Select **From list** to choose the spreadsheet title from the dropdown list, **By URL** to enter the url of the spreadsheet, or **By ID** to enter the `spreadsheetId`.
      * You can find the `spreadsheetId` in a Google Sheets URL: `https://docs.google.com/spreadsheets/d/spreadsheetId/edit#gid=0`.
    * **Sheet Containing Dataset**: Choose the sheet you want to write the evaluation results to. Usually this is the same sheet you select in the [Evaluation Trigger](/integrations/builtin/core-nodes/n8n-nodes-base.evaluationtrigger.md) node.
      * Select **From list** to choose the sheet title from the dropdown list, **By URL** to enter the url of the sheet, **By ID** to enter the `sheetId`, or **By Name** to enter the sheet title.
      * You can find the `sheetId` in a Google Sheets URL: `https://docs.google.com/spreadsheets/d/aBC-123_xYz/edit#gid=sheetId`.

You define the items to write to the data table or Google Sheet in the **Outputs** section. For each output, you set the following:

* **Name**: The Google Sheet column name to write the evaluation results to.
* **Value**: The value to write to the Google Sheet.

### Set Metrics <a href="#set-metrics" id="set-metrics"></a>

The **Set Metrics** operation includes a **Metrics to Return** section where you define the metrics to record and track for your evaluations. You can see the metric results in your workflow's **Evaluations** tab.

For each metric you wish to record, you set the following details:

* **Name**: The name to use for the metric.
* **Value**: The numeric value to record. Once you run your evaluation, you can drag and drop values from previous nodes here. Metric values must be numeric.

### Check If Evaluating <a href="#check-if-evaluating" id="check-if-evaluating"></a>

The **Check If Evaluating** operation doesn't have any parameters. This operation provides branching output connectors so that you can conditionally execute logic depending on whether the current execution is an evaluation or not.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Evaluation node documentation integration templates](https://n8n.io/integrations/evaluation) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

To learn more about n8n evaluations, check out the [evaluations documentation](/build/integrate-ai/test-and-improve-ai-workflows/understand-why-to-test.md)

n8n provides a trigger node for evaluations. You can find the node docs [here](/integrations/builtin/core-nodes/n8n-nodes-base.evaluationtrigger.md).

For common questions or issues and suggested solutions, refer to the evaluations [tips and common issues](/build/integrate-ai/test-and-improve-ai-workflows/fix-common-issues.md) page.
