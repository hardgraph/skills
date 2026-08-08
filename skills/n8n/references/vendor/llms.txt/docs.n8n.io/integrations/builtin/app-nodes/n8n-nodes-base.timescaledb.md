> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.timescaledb.md).

# TimescaleDB

Use the TimescaleDB node to automate work in TimescaleDB, and integrate TimescaleDB with other applications. n8n has built-in support for a wide range of TimescaleDB features, including executing an SQL query, as well as inserting and updating rows in a database.

On this page, you'll find a list of operations the TimescaleDB node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [TimescaleDB credentials](/integrations/builtin/credentials/timescaledb.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Execute an SQL query
* Insert rows in database
* Update rows in database

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse TimescaleDB node documentation integration templates](https://n8n.io/integrations/timescaledb) or [search all templates](https://n8n.io/workflows/)

## Specify a column's data type <a href="#specify-a-columns-data-type" id="specify-a-columns-data-type"></a>

To specify a column's data type, append the column name with `:type`, where `type` is the data type you want for the column. For example, if you want to specify the type `int` for the column **id** and type `text` for the column **name**, you can use the following snippet in the **Columns** field: `id:int,name:text`.
