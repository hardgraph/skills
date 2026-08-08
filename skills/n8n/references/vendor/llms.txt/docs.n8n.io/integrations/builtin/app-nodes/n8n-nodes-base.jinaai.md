> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.jinaai.md).

# Jina AI

Use the Jina AI node to automate work in Jina AI and integrate Jina AI with other applications. n8n has built-in support for a wide range of Jina AI features.

On this page, you'll find a list of operations the Jina AI node supports, and links to more resources.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/jinaai.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* **Reader**:
  * **Read**: Fetches content from a URL and converts it to clean, LLM-friendly formats.
  * **Search**: Performs a web search using Jina AI and returns the top results as clean, LLM-friendly formats.
* **Research**:
  * **Deep Research**: Research a topic and generate a structured research report.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Jina AI node documentation integration templates](https://n8n.io/integrations/jina-ai) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Jina AI's reader API documentation](https://r.jina.ai/docs) and [Jina AI's search API documentation](https://s.jina.ai/docs) for more information about the service.

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
