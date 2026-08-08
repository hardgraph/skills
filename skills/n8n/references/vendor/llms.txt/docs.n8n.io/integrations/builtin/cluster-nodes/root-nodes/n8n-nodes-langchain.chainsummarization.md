> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.chainsummarization.md).

# Summarization Chain

Use the Summarization Chain node to summarize multiple documents.

On this page, you'll find the node parameters for the Summarization Chain node, and links to more resources.

## Node parameters <a href="#node-parameters" id="node-parameters"></a>

Choose the type of data you need to summarize in **Data to Summarize**. The data type you choose determines the other node parameters.

* **Use Node Input (JSON)** and **Use Node Input (Binary)**: summarize the data coming into the node from the workflow.
  * You can configure the **Chunking Strategy**: choose what strategy to use to define the data chunk sizes.
    * If you choose **Simple (Define Below)** you can then set **Characters Per Chunk** and **Chunk Overlap (Characters)**.
    * Choose **Advanced** if you want to connect a splitter sub-node that provides more configuration options.
* **Use Document Loader**: summarize data provided by a document loader sub-node.

## Node Options <a href="#node-options" id="node-options"></a>

You can configure the summarization method and prompts. Select **Add Option** > **Summarization Method and Prompts**.

Options in **Summarization Method**:

* **Map Reduce**: this is the recommended option. Learn more about [Map Reduce](https://js.langchain.com/v0.1/docs/modules/chains/document/map_reduce/) in the LangChain documentation.
* **Refine**: learn more about [Refine](https://js.langchain.com/v0.1/docs/modules/chains/document/refine/) in the LangChain documentation.
* **Stuff**: learn more about [Stuff](https://js.langchain.com/v0.1/docs/modules/chains/document/stuff/) in the LangChain documentation.

You can customize the **Individual Summary Prompts** and the **Final Prompt to Combine**. There are examples in the node. You must include the `"{text}"` placeholder.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Summarization Chain node documentation integration templates](https://n8n.io/integrations/summarization-chain) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [LangChain's documentation on summarization](https://js.langchain.com/docs/tutorials/summarization/) for more information about the service.

View n8n's [Advanced AI](/build/integrate-ai.md) documentation.
