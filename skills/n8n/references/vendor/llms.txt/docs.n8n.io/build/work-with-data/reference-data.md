> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/work-with-data/reference-data.md).

# Reference data

Referencing data, or data mapping, means accessing information from previous nodes in your workflow. This allows you to use output from earlier steps as input for later nodes, creating dynamic workflows that pass data through multiple operations.

When you reference data, you're not changing it. You're pointing to values that already exist so you can use them in node parameters, expressions, or custom code.

If you want to change the data you're referencing, see [Transforming data](/build/work-with-data/transform-data/approaches-for-transforming-data.md).

## How to reference data <a href="#how-to-reference-data" id="how-to-reference-data"></a>

The main way to reference data is using [expressions](/build/work-with-data/expressions-versus-data-nodes.md#expressions). You can create expressions by typing them in a parameter's field or dragging and dropping fields from the Input panel in the UI. Expressions will automatically figure out the correct item to use using [item linking](/build/work-with-data/reference-data/link-data-items.md).
