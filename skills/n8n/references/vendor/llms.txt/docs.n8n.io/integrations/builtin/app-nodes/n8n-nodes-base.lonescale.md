> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.lonescale.md).

# LoneScale

Use the LoneScale node to automate work in LoneScale and integrate LoneScale with other applications. n8n has built-in support for managing Lists and Items in LoneScale.

On this page, you'll find a list of operations the LoneScale node supports, and links to more resources.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/lonescale.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* List
  * Create
* Item
  * Create

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse LoneScale node documentation integration templates](https://n8n.io/integrations/lonescale) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [LoneScales documentation](https://help-center.lonescale.com/en/articles/6454360-lonescale-public-api) for more information about the service.

n8n provides a trigger node for LoneScale. You can find the trigger node docs [here](/integrations/builtin/trigger-nodes/n8n-nodes-base.lonescaletrigger.md).

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
