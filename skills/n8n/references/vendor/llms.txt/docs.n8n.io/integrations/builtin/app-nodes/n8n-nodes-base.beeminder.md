> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.beeminder.md).

# Beeminder

Use the Beeminder node to automate work in Beeminder, and integrate Beeminder with other applications. n8n has built-in support for a wide range of Beeminder features, including creating, deleting, and updating data points.

On this page, you'll find a list of operations the Beeminder node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Beeminder credentials](/integrations/builtin/credentials/beeminder.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

**data point**

* Create data point for a goal
* Delete a data point
* Get all data points for a goal
* Update a data point

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Beeminder node documentation integration templates](https://n8n.io/integrations/beeminder) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
