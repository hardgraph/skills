> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.googlesheets/common-issues.md).

# Common issues

Here are some common errors and issues with the [Google Sheets node](/integrations/builtin/app-nodes/n8n-nodes-base.googlesheets.md) and steps to resolve or troubleshoot them.

## Append an array <a href="#append-an-array" id="append-an-array"></a>

To insert an array of data into Google Sheets, you must convert the array into a valid JSON (key, value) format.

To do so, consider using:

1. The [Split Out](/integrations/builtin/core-nodes/n8n-nodes-base.splitout.md) node.
2. The [AI Transform](/integrations/builtin/core-nodes/n8n-nodes-base.aitransform.md) node. For example, try entering something like:

   ```
   Convert 'languages' array to JSON (key, value) pairs.
   ```
3. The [Code node](/integrations/builtin/core-nodes/n8n-nodes-base.code.md).

## Column names were updated after the node's setup <a href="#column-names-were-updated-after-the-nodes-setup" id="column-names-were-updated-after-the-nodes-setup"></a>

You'll receive this error if the Google Sheet's column names have changed since you set up the node.

To refresh the column names, re-select **Mapping Column Mode**. This should prompt the node to fetch the column names again.

Once the column names refresh, update the node parameters.
