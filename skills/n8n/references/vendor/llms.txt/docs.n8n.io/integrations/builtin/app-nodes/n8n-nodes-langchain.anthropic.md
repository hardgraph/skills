> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-langchain.anthropic.md).

# Anthropic

Use the Anthropic node to automate work in Anthropic and integrate Anthropic with other applications. n8n has built-in support for a wide range of Anthropic features, including analyzing, uploading, getting, and deleting documents, files, and images, and generating, improving, or templatizing prompts.

On this page, you'll find a list of operations the Anthropic node supports, and links to more resources.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/anthropic.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Document:
  * Analyze Document: Take in documents and answer questions about them.
* File:
  * Upload File: Upload a file to the Anthropic API for later user.
  * Get File Metadata: Get metadata for a file from the Anthropic API.
  * List Files: List files from the Anthropic API.
  * Delete File: Delete a file from the Anthropic API.
* Image:
  * Analyze Image: Take in images and answer questions about them.
* Prompt:
  * Generate Prompt: Generate a prompt for a model.
  * Improve Prompt: Improve a prompt for a model.
  * Templatize Prompt: Templatize a prompt for a model.
* Text:
  * Message a Model: Create a completion with an Anthropic model.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Anthropic node documentation integration templates](https://n8n.io/integrations/anthropic) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Anthropic's documentation](https://docs.anthropic.com/en/api/overview) for more information about the service.

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
