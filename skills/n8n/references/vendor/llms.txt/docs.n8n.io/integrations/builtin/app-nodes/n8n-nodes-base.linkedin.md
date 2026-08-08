> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.linkedin.md).

# LinkedIn

Use the LinkedIn node to automate work in LinkedIn, and integrate LinkedIn with other applications. n8n supports creating posts.

On this page, you'll find a list of operations the LinkedIn node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [LinkedIn credentials](/integrations/builtin/credentials/linkedin.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Post
  * Create

## Parameters <a href="#parameters" id="parameters"></a>

* **Post As**: choose whether to post as a **Person** or **Organization**.
* **Person Name or ID** and **Organization URN**: enter an identifier for the person or organization.<br>

  <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p><strong>Posting as organization</strong></p><p>If posting as an Organization enter the organization number in the URN field. For example, <code>03262013</code> not <code>urn:li:company:03262013</code>.</p></div>
* **Text**: the post contents.
* **Media Category**: use this when including images or article URLs in your post.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse LinkedIn node documentation integration templates](https://n8n.io/integrations/linkedin) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [LinkedIn's API documentation](https://learn.microsoft.com/en-us/linkedin/) for more information about the service.

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
