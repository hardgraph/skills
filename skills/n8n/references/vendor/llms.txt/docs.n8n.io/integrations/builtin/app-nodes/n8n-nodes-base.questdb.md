> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.questdb.md).

# QuestDB

Use the QuestDB node to automate work in QuestDB, and integrate QuestDB with other applications. n8n supports executing an SQL query and inserting rows in a database with QuestDB.

On this page, you'll find a list of operations the QuestDB node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [QuestDB credentials](/integrations/builtin/credentials/questdb.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Executes a SQL query.
* Insert rows in database.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse QuestDB node documentation integration templates](https://n8n.io/integrations/questdb) or [search all templates](https://n8n.io/workflows/)

## Node reference <a href="#node-reference" id="node-reference"></a>

### Specify a column's data type <a href="#specify-a-columns-data-type" id="specify-a-columns-data-type"></a>

To specify a column's data type, append the column name with `:type`, where `type` is the data type you want for column. For example, if you want to specify the type `int` for the column **id** and type `text` for the column **name**, you can use the following snippet in the **Columns** field: `id:int,name:text`.
