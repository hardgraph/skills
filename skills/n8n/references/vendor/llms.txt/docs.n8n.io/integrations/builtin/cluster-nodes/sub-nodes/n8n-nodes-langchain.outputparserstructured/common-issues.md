> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.outputparserstructured/common-issues.md).

# Common issues

Here are some common errors and issues with the [Structured Output Parser node](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.outputparserstructured.md) and steps to resolve or troubleshoot them.

## Processing parameters <a href="#processing-parameters" id="processing-parameters"></a>

The Structured Output Parser node is a sub-node[^1]. Sub-nodes behave differently than other nodes when processing multiple items using expressions.

Most nodes, including [root nodes](#user-content-fn-2)[^2], take any number of items as input, process these items, and output the results. You can use expressions to refer to input items, and the node resolves the expression for each item in turn. For example, given an input of five name values, the expression `{{ $json.name }}` resolves to each name in turn.

In sub-nodes, the expression always resolves to the first item. For example, given an input of five name values, the expression `{{ $json.name }}` always resolves to the first name.

## Adding the structured output parser node to AI nodes <a href="#adding-the-structured-output-parser-node-to-ai-nodes" id="adding-the-structured-output-parser-node-to-ai-nodes"></a>

You can attach output parser nodes to select [AI root nodes](/integrations/builtin/cluster-nodes/root-nodes.md).

To add the Structured Output Parser to a node, enable the **Require Specific Output Format** option in the AI root node you wish to format. Once the option is enabled, a new **output parser** attachment point is displayed. Click the **output parser** attachment point to add the Structured Output Parser node to the node.

## Using the structured output parser to format intermediary steps <a href="#using-the-structured-output-parser-to-format-intermediary-steps" id="using-the-structured-output-parser-to-format-intermediary-steps"></a>

The Structured Output Parser node structures the final output from AI agents. It's not intended to structure intermediary output to pass to other AI tools or stages.

To request a specific format for intermediary output, include the response structure in the **System Message** for the **AI Agent**. The message can include either a schema or example response for the agent to use as a template for its results.

## Structuring output from agents <a href="#structuring-output-from-agents" id="structuring-output-from-agents"></a>

Structured output parsing is often not reliable when working with [agents](/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent.md).

If your workflow uses agents, n8n recommends using a separate [LLM-chain](/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.chainllm.md) to receive the data from the agent and parse it. This leads to better, more consistent results than parsing directly in the agent workflow.

[^1]: n8n cluster nodes consist of one or more sub nodes connected to a root node. Sub nodes extend the functionality of the root node, providing access to specific services or resources or offering specific types of dedicated processing, like calculator functionality, for example.

[^2]: Each n8n cluster node contains a single root nodes that defines the main functionality of the cluster. One or more sub nodes attach to the root node to extend its functionality.
