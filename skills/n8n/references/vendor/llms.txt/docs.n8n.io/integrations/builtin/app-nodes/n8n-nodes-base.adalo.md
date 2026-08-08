> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.adalo.md).

# Adalo

Use the Adalo node to automate work in Adalo, and integrate Adalo with other applications. n8n has built-in support for a wide range of Adalo features, including like creating, getting, updating and deleting databases, records, and collections.

On this page, you'll find a list of operations the Adalo node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Adalo credentials](/integrations/builtin/credentials/adalo.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Collection
  * Create
  * Delete
  * Get
  * Get Many
  * Update

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Adalo node documentation integration templates](https://n8n.io/integrations/adalo) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Adalo's documentation](https://help.adalo.com/) for more information on using Adalo. Their [External Collections with APIs](https://help.adalo.com/integrations/external-collections-with-apis) page gives more detail about what you can do with Adalo collections.

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
