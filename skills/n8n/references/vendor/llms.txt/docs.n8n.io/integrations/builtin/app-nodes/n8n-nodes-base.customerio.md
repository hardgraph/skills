> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.customerio.md).

# Customer.io

Use the Customer.io node to automate work in Customer.io, and integrate Customer.io with other applications. n8n has built-in support for a wide range of Customer.io features, including creating and updating customers, tracking events, and getting campaigns.

On this page, you'll find a list of operations the Customer.io node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Customer.io credentials](/integrations/builtin/credentials/customerio.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Customer
  * Create/Update a customer.
  * Delete a customer.
* Event
  * Track a customer event.
  * Track an anonymous event.
* Campaign
  * Get
  * Get All
  * Get Metrics
* Segment
  * Add Customer
  * Remove Customer

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Customer.io node documentation integration templates](https://n8n.io/integrations/customerio) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
