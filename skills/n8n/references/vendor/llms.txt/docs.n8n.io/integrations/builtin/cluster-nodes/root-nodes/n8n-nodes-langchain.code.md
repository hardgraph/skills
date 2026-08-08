> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.code.md).

# LangChain Code

Use the LangChain Code node to import LangChain. This means if there is functionality you need that n8n hasn't created a node for, you can still use it. By configuring the LangChain Code node connectors you can use it as a normal node, root node or sub-node.

On this page, you'll find the node parameters, guidance on configuring the node, and links to more resources.

{% hint style="warning" %}
**Deprecated due to security issues**

This node has critical security issues and isn't safe to use. It's deprecated and hidden from the nodes panel. Avoid using it in your workflows.
{% endhint %}

{% hint style="info" %}
**Not available on Cloud**

This node is only available on self-hosted n8n.
{% endhint %}

## Node parameters <a href="#node-parameters" id="node-parameters"></a>

### Add Code <a href="#add-code" id="add-code"></a>

Add your custom code. Choose either **Execute** or **Supply Data** mode. You can only use one mode.

Unlike the [Code node](/integrations/builtin/core-nodes/n8n-nodes-base.code.md), the LangChain Code node doesn't support Python.

* **Execute**: use the LangChain Code node like n8n's own Code node. This takes input data from the workflow, processes it, and returns it as the node output. This mode requires a main input and output. You must create these connections in **Inputs** and **Outputs**.
* **Supply Data**: use the LangChain Code node as a sub-node, sending data to a root node. This uses an output other than main.

By default, you can't load built-in or external modules in this node. Self-hosted users can [enable built-in and external modules](/deploy/host-n8n/configure-n8n/basic-configuration.md).

### Inputs <a href="#inputs" id="inputs"></a>

Choose the input types.

The main input is the normal connector found in all n8n workflows. If you have a main input and output set in the node, **Execute** code is required.

### Outputs <a href="#outputs" id="outputs"></a>

Choose the output types.

The main output is the normal connector found in all n8n workflows. If you have a main input and output set in the node, **Execute** code is required.

## Node inputs and outputs configuration <a href="#node-inputs-and-outputs-configuration" id="node-inputs-and-outputs-configuration"></a>

By configuring the LangChain Code node connectors (inputs and outputs) you can use it as an app node, root node or sub-node.

![Screenshot of a workflow with four LangChain nodes, configured as different node types](/files/4S6PFkADOc4GFzq5zsxM)

| Node type                                                                                      | Inputs                        | Outputs                                                                   | Code mode   |
| ---------------------------------------------------------------------------------------------- | ----------------------------- | ------------------------------------------------------------------------- | ----------- |
| App node. Similar to the [Code node](/integrations/builtin/core-nodes/n8n-nodes-base.code.md). | Main                          | Main                                                                      | Execute     |
| Root node                                                                                      | Main; at least one other type | Main                                                                      | Execute     |
| Sub-node                                                                                       | -                             | A type other than main. Must match the input type you want to connect to. | Supply Data |
| Sub-node with sub-nodes                                                                        | A type other than main        | A type other than main. Must match the input type you want to connect to. | Supply Data |

## Built-in methods <a href="#built-in-methods" id="built-in-methods"></a>

n8n provides these methods to make it easier to perform common tasks in the LangChain Code node.

| Method                                                           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `this.addInputData(inputName, data)`                             | <p>Populate the data of a specified non-main input. Useful for mocking data.</p><ul><li><code>inputName</code> is the input connection type, and must be one of: <code>ai\_agent</code>, <code>ai\_chain</code>, <code>ai\_document</code>, <code>ai\_embedding</code>, <code>ai\_languageModel</code>, <code>ai\_memory</code>, <code>ai\_outputParser</code>, <code>ai\_retriever</code>, <code>ai\_textSplitter</code>, <code>ai\_tool</code>, <code>ai\_vectorRetriever</code>, <code>ai\_vectorStore</code></li><li><code>data</code> contains the data you want to add. Refer to <a href="/spaces/rPN1zU5jaYNvwH7RzxqA/pages/4w4R7N2ysLi9xAC7Jfde">Data structure</a> for information on the data structure expected by n8n.</li></ul>   |
| `this.addOutputData(outputName, data)`                           | <p>Populate the data of a specified non-main output. Useful for mocking data.</p><ul><li><code>outputName</code> is the input connection type, and must be one of: <code>ai\_agent</code>, <code>ai\_chain</code>, <code>ai\_document</code>, <code>ai\_embedding</code>, <code>ai\_languageModel</code>, <code>ai\_memory</code>, <code>ai\_outputParser</code>, <code>ai\_retriever</code>, <code>ai\_textSplitter</code>, <code>ai\_tool</code>, <code>ai\_vectorRetriever</code>, <code>ai\_vectorStore</code></li><li><code>data</code> contains the data you want to add. Refer to <a href="/spaces/rPN1zU5jaYNvwH7RzxqA/pages/4w4R7N2ysLi9xAC7Jfde">Data structure</a> for information on the data structure expected by n8n.</li></ul> |
| `this.getInputConnectionData(inputName, itemIndex, inputIndex?)` | <p>Get data from a specified non-main input.</p><ul><li><code>inputName</code> is the input connection type, and must be one of: <code>ai\_agent</code>, <code>ai\_chain</code>, <code>ai\_document</code>, <code>ai\_embedding</code>, <code>ai\_languageModel</code>, <code>ai\_memory</code>, <code>ai\_outputParser</code>, <code>ai\_retriever</code>, <code>ai\_textSplitter</code>, <code>ai\_tool</code>, <code>ai\_vectorRetriever</code>, <code>ai\_vectorStore</code></li><li><code>itemIndex</code> should always be <code>0</code> (this parameter will be used in upcoming functionality)</li><li>Use <code>inputIndex</code> if there is more than one node connected to the specified input.</li></ul>                         |
| `this.getInputData(inputIndex?, inputName?)`                     | Get data from the main input.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `this.getNode()`                                                 | Get the current node.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `this.getNodeOutputs()`                                          | Get the outputs of the current node.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `this.getExecutionCancelSignal()`                                | Use this to stop the execution of a function when the workflow stops. In most cases n8n handles this, but you may need to use it if building your own chains or agents. It replaces the [Cancelling a running LLMChain](https://js.langchain.com/docs/modules/chains/foundational/llm_chain#cancelling-a-running-llmchain) code that you'd use if building a LangChain application normally.                                                                                                                                                                                                                                                                                                                                                   |

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse LangChain Code node documentation integration templates](https://n8n.io/integrations/langchain-code) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

View n8n's [Advanced AI](/build/integrate-ai.md) documentation.
