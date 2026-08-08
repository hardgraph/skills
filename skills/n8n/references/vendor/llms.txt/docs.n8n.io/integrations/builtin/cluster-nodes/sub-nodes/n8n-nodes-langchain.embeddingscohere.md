> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.embeddingscohere.md).

# Embeddings Cohere

Use the Embeddings Cohere node to generate embeddings[^1] for a given text.

On this page, you'll find the node parameters for the Embeddings Cohere node, and links to more resources.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/cohere.md).
{% endhint %}

{% hint style="info" %}
**Parameter resolution in sub-nodes**

Sub-nodes behave differently to other nodes when processing multiple items using an expression.

Most nodes, including root nodes, take any number of items as input, process these items, and output the results. You can use expressions to refer to input items, and the node resolves the expression for each item in turn. For example, given an input of five `name` values, the expression `{{ $json.name }}` resolves to each name in turn.

In sub-nodes, the expression always resolves to the first item. For example, given an input of five `name` values, the expression `{{ $json.name }}` always resolves to the first name.
{% endhint %}

## Node parameters <a href="#node-parameters" id="node-parameters"></a>

* **Model**: Select the model to use to generate the embedding. Choose from:
  * **Embed-English-v2.0(4096 Dimensions)**
  * **Embed-English-Light-v2.0(1024 Dimensions)**
  * **Embed-Multilingual-v2.0(768 Dimensions)**

Learn more about available models in [Cohere's models documentation](https://docs.cohere.com/docs/models).

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Embeddings Cohere node documentation integration templates](https://n8n.io/integrations/embeddings-cohere) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Langchain's Cohere embeddings documentation](https://js.langchain.com/docs/integrations/text_embedding/cohere/) for more information about the service.

View n8n's [Advanced AI](/build/integrate-ai.md) documentation.

[^1]: Embeddings are numerical representations of data using vectors. They're used by AI to interpret complex data and relationships by mapping values across many dimensions. Vector databases, or vector stores, are databases designed to store and access embeddings.
