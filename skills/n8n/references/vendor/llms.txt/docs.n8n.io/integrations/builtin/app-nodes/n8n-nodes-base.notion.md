> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.notion.md).

# Notion

Use the Notion node to automate work in Notion, and integrate Notion with other applications. n8n has built-in support for a wide range of Notion features, including getting and searching databases, creating pages, and getting users.

On this page, you'll find a list of operations the Notion node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Notion credentials](/integrations/builtin/credentials/notion.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Block
  * Append After
  * Get Many
  * Get Markdown
* Data Source
  * Get
  * Search
* Database
  * Get
* Database Page
  * Create
  * Get
  * Get Many
  * Update
* Page
  * Archive
  * Create
  * Get Markdown
  * Search
  * Update Markdown
* User
  * Get
  * Get Many

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse n8n-nodes-base.notion integration templates](https://n8n.io/integrations/notion) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

n8n provides an app node for Notion. You can find the trigger node docs [here](/integrations/builtin/trigger-nodes/n8n-nodes-base.notiontrigger.md).

Refer to [Notion's documentation](https://developers.notion.com/) for details about their API.

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.

## Common issues <a href="#common-issues" id="common-issues"></a>

For common errors or issues and suggested resolution steps, refer to [Common issues](/integrations/builtin/app-nodes/n8n-nodes-base.notion/common-issues.md).
