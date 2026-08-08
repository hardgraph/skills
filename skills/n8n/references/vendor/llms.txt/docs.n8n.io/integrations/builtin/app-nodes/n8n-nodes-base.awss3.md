> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.awss3.md).

# AWS S3

Use the AWS S3 node to automate work in AWS S3, and integrate AWS S3 with other applications. n8n has built-in support for a wide range of AWS S3 features, including creating and deleting buckets, copying and downloading files, as well as getting folders.

On this page, you'll find a list of operations the AWS S3 node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [AWS credentials](/integrations/builtin/credentials/aws.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Bucket
  * Create a bucket
  * Delete a bucket
  * Get all buckets
  * Search within a bucket
* File
  * Copy a file
  * Delete a file
  * Download a file
  * Get all files
  * Upload a file
* Folder
  * Create a folder
  * Delete a folder
  * Get all folders

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse AWS S3 node documentation integration templates](https://n8n.io/integrations/aws-s3) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
