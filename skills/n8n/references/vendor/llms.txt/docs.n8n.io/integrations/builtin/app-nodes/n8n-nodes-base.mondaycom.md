> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.mondaycom.md).

# monday.com

Use the monday.com node to automate work in monday.com, and integrate monday.com with other applications. n8n has built-in support for a wide range of monday.com features, including creating a new board, and adding, deleting, and getting items on the board.

On this page, you'll find a list of operations the monday.com node supports and links to more resources.

{% hint style="warning" %}
**Minimum required version**

This node requires n8n version 1.22.6 or above.
{% endhint %}

{% hint style="info" %}
**Credentials**

Refer to [monday.com credentials](/integrations/builtin/credentials/mondaycom.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Board
  * Archive a board
  * Create a new board
  * Get a board
  * Get all boards
* Board Column
  * Create a new column
  * Get all columns
* Board Group
  * Delete a group in a board
  * Create a group in a board
  * Get list of groups in a board
* Board Item
  * Add an update to an item.
  * Change a column value for a board item
  * Change multiple column values for a board item
  * Create an item in a board's group
  * Delete an item
  * Get an item
  * Get all items
  * Get items by column value
  * Move item to group

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse monday.com node documentation integration templates](https://n8n.io/integrations/mondaycom) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
