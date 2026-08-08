> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/understand-workflows/understand-executions/stream-real-time-responses.md).

# Stream real-time responses

{% hint style="info" %}
**Feature availability**

Available on all plans.
{% endhint %}

Streaming responses let you send data back to users as an AI Agent node generates it. This is useful for chatbots, where you want to show the user the answer as it's generated to provide a better user experience.

You can enable streaming using either:

* The [Chat Trigger](/integrations/builtin/core-nodes/n8n-nodes-langchain.chattrigger.md)
* The [Webhook node](/integrations/builtin/core-nodes/n8n-nodes-base.webhook.md)

In both cases, set the node's **Response Mode** to **Streaming**.

## Configure nodes for streaming <a href="#configure-nodes-for-streaming" id="configure-nodes-for-streaming"></a>

To stream data, you need to add nodes to the workflow that support streaming output. Not all nodes support this feature.

1. Choose a node that supports streaming, such as:
   * [AI agent](/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent.md)
   * [Respond to Webhook](/integrations/builtin/core-nodes/n8n-nodes-base.respondtowebhook.md)
2. You can disable streaming in the options of these nodes. By default, they stream data whenever the executed trigger has its `Response Mode` set to `Streaming response`.

## Important information <a href="#important-information" id="important-information"></a>

Keep in mind the following details when configuring streaming responses:

* **Trigger**: Your trigger node must support streaming and have streaming configured. Without this, the workflow behaves according to your response mode settings.
* **Node configuration**: Even with streaming enabled on the trigger, you need at least one node configured to stream data. Otherwise, your workflow will send no data.
