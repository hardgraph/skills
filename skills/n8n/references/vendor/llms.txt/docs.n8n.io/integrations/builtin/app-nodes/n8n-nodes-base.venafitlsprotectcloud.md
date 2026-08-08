> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.venafitlsprotectcloud.md).

# Venafi TLS Protect Cloud

Use the Venafi TLS Protect Cloud node to automate work in Venafi TLS Protect Cloud, and integrate Venafi TLS Protect Cloud with other applications. n8n has built-in support for a wide range of Venafi TLS Protect Cloud features, including deleting and downloading certificates, as well as creating certificates requests.

On this page, you'll find a list of operations the Venafi TLS Protect Cloud node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Venafi TLS Protect Cloud credentials](/integrations/builtin/credentials/venafitlsprotectcloud.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Certificate
  * Delete
  * Download
  * Get
  * Get Many
  * Renew
* Certificate Request
  * Create
  * Get
  * Get Many

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Venafi TLS Protect Cloud node documentation integration templates](https://n8n.io/integrations/venafi-tls-protect-cloud) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Venafi's REST API documentation](https://docs.venafi.cloud/api/vaas-rest-api/) for more information on this service.

n8n also provides:

* A [trigger node](/integrations/builtin/trigger-nodes/n8n-nodes-base.venafitlsprotectcloudtrigger.md) for Venafi TLS Protect Cloud.
* A [node](/integrations/builtin/app-nodes/n8n-nodes-base.venafitlsprotectdatacenter.md) for Venafi TLS Protect Datacenter.

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
