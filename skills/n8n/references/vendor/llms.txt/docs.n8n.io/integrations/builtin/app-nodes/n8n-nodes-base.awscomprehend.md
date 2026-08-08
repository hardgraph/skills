> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.awscomprehend.md).

# AWS Comprehend

Use the AWS Comprehend node to automate work in AWS Comprehend, and integrate AWS Comprehend with other applications. n8n has built-in support for a wide range of AWS Comprehend features, including identifying and analyzing texts.

On this page, you'll find a list of operations the AWS Comprehend node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [AWS Comprehend credentials](/integrations/builtin/credentials/aws.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

**Text**

* Identify the dominant language
* Analyse the sentiment of the text

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse AWS Comprehend node documentation integration templates](https://n8n.io/integrations/aws-comprehend) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
