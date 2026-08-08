> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.embeddingsawsbedrock.md).

# Embeddings AWS Bedrock

Use the Embeddings AWS Bedrock node to generate embeddings[^1] for a given text.

On this page, you'll find the node parameters for the Embeddings AWS Bedrock node, and links to more resources.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/aws.md).

If you route Bedrock through a [VPC interface endpoint (PrivateLink)](https://docs.aws.amazon.com/bedrock/latest/userguide/vpc-interface-endpoints.html) without private DNS, set the **Bedrock Endpoint** and **Bedrock Runtime Endpoint** custom endpoints in the credential.
{% endhint %}

{% hint style="info" %}
**Parameter resolution in sub-nodes**

Sub-nodes behave differently to other nodes when processing multiple items using an expression.

Most nodes, including root nodes, take any number of items as input, process these items, and output the results. You can use expressions to refer to input items, and the node resolves the expression for each item in turn. For example, given an input of five `name` values, the expression `{{ $json.name }}` resolves to each name in turn.

In sub-nodes, the expression always resolves to the first item. For example, given an input of five `name` values, the expression `{{ $json.name }}` always resolves to the first name.
{% endhint %}

## Node parameters <a href="#node-parameters" id="node-parameters"></a>

* **Authentication**: Select the authentication method:
  * **AWS (IAM)**: Use an IAM access key. Select an **AWS** credential.
  * **AWS (Assume Role)**: Temporarily assume an IAM role. Select an **AWS (Assume Role)** credential.
* **Model**: Select the model to use to generate the embedding. If the dropdown is empty, your IAM role may not have the `bedrock:ListFoundationModels` permission. Switch the field to **Expression** mode and enter the model ID directly.

Learn more about available models in the [Amazon Bedrock documentation](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html).

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Embeddings AWS Bedrock node documentation integration templates](https://n8n.io/integrations/embeddings-aws-bedrock) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [LangChains's AWS Bedrock embeddings documentation](https://js.langchain.com/docs/integrations/platforms/aws/#text-embedding-models) and the [AWS Bedrock documentation](https://docs.aws.amazon.com/bedrock/) for more information about AWS Bedrock.

View n8n's [Advanced AI](/build/integrate-ai.md) documentation.

[^1]: Embeddings are numerical representations of data using vectors. They're used by AI to interpret complex data and relationships by mapping values across many dimensions. Vector databases, or vector stores, are databases designed to store and access embeddings.
