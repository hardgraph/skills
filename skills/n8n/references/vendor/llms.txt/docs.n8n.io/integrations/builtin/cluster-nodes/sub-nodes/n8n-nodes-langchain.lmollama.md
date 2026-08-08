> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmollama.md).

# Ollama Model

The Ollama Model node allows you use local Llama 2 models.

On this page, you'll find the node parameters for the Ollama Model node, and links to more resources.

This node lacks tools support, so it won't work with the [AI Agent](/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent.md) node. Instead, connect it with the [Basic LLM Chain](/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.chainllm.md) node.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/ollama.md).
{% endhint %}

{% hint style="info" %}
**Parameter resolution in sub-nodes**

Sub-nodes behave differently to other nodes when processing multiple items using an expression.

Most nodes, including root nodes, take any number of items as input, process these items, and output the results. You can use expressions to refer to input items, and the node resolves the expression for each item in turn. For example, given an input of five `name` values, the expression `{{ $json.name }}` resolves to each name in turn.

In sub-nodes, the expression always resolves to the first item. For example, given an input of five `name` values, the expression `{{ $json.name }}` always resolves to the first name.
{% endhint %}

## Node parameters <a href="#node-parameters" id="node-parameters"></a>

* **Model**: Select the model that generates the completion. Choose from:
  * **Llama2**
  * **Llama2 13B**
  * **Llama2 70B**
  * **Llama2 Uncensored**

Refer to the Ollama [Models Library documentation](https://ollama.com/library) for more information about available models.

## Node options <a href="#node-options" id="node-options"></a>

* **Sampling Temperature**: Use this option to control the randomness of the sampling process. A higher temperature creates more diverse sampling, but increases the risk of hallucinations.
* **Top K**: Enter the number of token choices the model uses to generate the next token.
* **Top P**: Use this option to set the probability the completion should use. Use a lower value to ignore less probable options.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse n8n-nodes-langchain.lmollama integration templates](https://n8n.io/integrations/ollama-model) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [LangChains's Ollama documentation](https://js.langchain.com/docs/integrations/llms/ollama/) for more information about the service.

View n8n's [Advanced AI](/build/integrate-ai.md) documentation.

## Common issues <a href="#common-issues" id="common-issues"></a>

For common questions or issues and suggested solutions, refer to [Common issues](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmollama/common-issues.md).

New to working with AI and using self-hosted n8n? Try n8n's [self-hosted AI Starter Kit](/deploy/host-n8n/deploy-with-the-ai-starter-kit.md) to get started with a proof-of-concept or demo playground using Ollama, Qdrant, and PostgreSQL.
