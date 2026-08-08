> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/plan-execute-agent.md).

# Plan and Execute Agent

{% hint style="info" %}
**Feature removed**

n8n removed this functionality in February 2025.
{% endhint %}

The Plan and Execute Agent is like the [ReAct agent](/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/react-agent.md) but with a focus on planning. It first creates a high-level plan to solve the given task and then executes the plan step by step. This agent is most useful for tasks that require a structured approach and careful planning.

Refer to [AI Agent](/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent.md) for more information on the AI Agent node itself.

## Node parameters <a href="#node-parameters" id="node-parameters"></a>

Configure the Plan and Execute Agent using the following parameters.

### Prompt <a href="#prompt" id="prompt"></a>

Select how you want the node to construct the prompt (also known as the user's query or input from the chat).

Choose from:

* **Take from previous node automatically**: If you select this option, the node expects an input from a previous node called `chatInput`.
* **Define below**: If you select this option, provide either static text or an expression for dynamic content to serve as the prompt in the **Prompt (User Message)** field.

### Require Specific Output Format <a href="#require-specific-output-format" id="require-specific-output-format"></a>

This parameter controls whether you want the node to require a specific output format. When turned on, n8n prompts you to connect one of these output parsers to the node:

* [Auto-fixing Output Parser](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.outputparserautofixing.md)
* [Item List Output Parser](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.outputparseritemlist.md)
* [Structured Output Parser](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.outputparserstructured.md)

## Node options <a href="#node-options" id="node-options"></a>

Refine the Plan and Execute Agent node's behavior using these options:

### Human Message Template <a href="#human-message-template" id="human-message-template"></a>

Enter a message that n8n will send to the agent during each step execution.

Available LangChain expressions:

* `{previous_steps}`: Contains information about the previous steps the agent's already completed.
* `{current_step}`: Contains information about the current step.
* `{agent_scratchpad}`: Information to remember for the next iteration.

### Tracing Metadata <a href="#tracing-metadata" id="tracing-metadata"></a>

Add custom key-value metadata to tracing events for this agent. This is useful for filtering and debugging runs in tracing tools like [LangSmith](https://github.com/n8n-io/n8n-docs/blob/main/advanced-ai/langchain/langsmith.md).

Entries with empty keys or values are ignored.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

Refer to the main AI Agent node's [Templates and examples](/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent.md#templates-and-examples) section.

## Common issues <a href="#common-issues" id="common-issues"></a>

For common questions or issues and suggested solutions, refer to [Common issues](/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/common-issues.md).
