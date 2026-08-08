> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.awscertificatemanager.md).

# AWS Certificate Manager

Use the AWS Certificate Manager node to automate work in AWS Certificate Manager, and integrate AWS Certificate Manager with other applications. n8n has built-in support for a wide range of AWS Certificate Manager features, including creating, deleting, getting, and renewing SSL certificates.

On this page, you'll find a list of operations the AWS Certificate Manager node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [AWS Certificate Manager credentials](/integrations/builtin/credentials/aws.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Certificate
  * Delete
  * Get
  * Get Many
  * Get Metadata
  * Renew

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse AWS Certificate Manager node documentation integration templates](https://n8n.io/integrations/aws-certificate-manager) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [AWS Certificate Manager's documentation](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html) for more information on this service.

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
