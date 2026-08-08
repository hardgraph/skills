> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.datatable.md).

# Data Table

Use the Data Table node to create and manage internal data tables. Data tables allow you to store structured data directly inside n8n and use it across workflows.

You can use the Data Table node to:

* Create, list, and manage data tables
* Insert, update, delete, and upsert rows in data tables
* Query and retrieve rows using matching conditions

{% hint style="info" %}
**Working with data tables**

As well as using the Data Tables node in a workflow, you can view and manage data tables manually from the **Data Tables** tab in your project **Overview**.

For information about working with data tables in this tab, and guidance on when to use data tables and their limitations, see [Data tables](/build/work-with-data/data-tables.md).
{% endhint %}

## Resources <a href="#resources" id="resources"></a>

The Data Table node supports the following resources:

* **Data Table:** Create, list, update, and delete tables.
* **Row:** Insert, retrieve, update, delete, and upsert rows within a table.

### Operations <a href="#operations" id="operations"></a>

See available operations below. For detailed information on parameters for different operation types, refer to the [Table operations](/integrations/builtin/core-nodes/n8n-nodes-base.datatable/tables.md) and [Row operations](/integrations/builtin/core-nodes/n8n-nodes-base.datatable/rows.md) pages.

* **Rows**
  * [**Delete:**](/integrations/builtin/core-nodes/n8n-nodes-base.datatable/rows.md#delete-row) Delete one or more rows.
  * [**Get:**](/integrations/builtin/core-nodes/n8n-nodes-base.datatable/rows.md#get-row) Get one or more rows from your table based on defined filters.
  * [**If Row Exists:**](/integrations/builtin/core-nodes/n8n-nodes-base.datatable/rows.md#if-row-exists) Specify a set of conditions to match input items that exist in the data table.
  * [**If Row Does Not Exist:**](/integrations/builtin/core-nodes/n8n-nodes-base.datatable/rows.md#if-row-does-not-exist) Specify a set of conditions to match input items that don't exist in the data table.
  * [**Insert:**](/integrations/builtin/core-nodes/n8n-nodes-base.datatable/rows.md#insert-row) Insert rows into an existing table.
  * [**Update:**](/integrations/builtin/core-nodes/n8n-nodes-base.datatable/rows.md#update-row) Update one or more rows.
  * [**Upsert:**](/integrations/builtin/core-nodes/n8n-nodes-base.datatable/rows.md#upsert-row) Upsert one or more rows. If the row exists, it's updated; otherwise, a new row is created.
* **Tables**
  * [**Create:**](/integrations/builtin/core-nodes/n8n-nodes-base.datatable/tables.md#create-a-data-table) Create a new data table.
  * [**Delete:**](/integrations/builtin/core-nodes/n8n-nodes-base.datatable/tables.md#delete-a-data-table) Delete an existing data table.
  * [**List:**](/integrations/builtin/core-nodes/n8n-nodes-base.datatable/tables.md#list-data-tables) List existing data tables.
  * [**Update:**](/integrations/builtin/core-nodes/n8n-nodes-base.datatable/tables.md#update-a-data-table) Update an existing data table.

## Related resources <a href="#related-resources" id="related-resources"></a>

[Data tables](/build/work-with-data/data-tables.md) explains how to create and manage data tables.
