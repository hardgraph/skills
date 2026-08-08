> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.rocketchat.md).

# Rocket.Chat

Use the Rocket.Chat node to automate work in Rocket.Chat, and integrate Rocket.Chat with other applications. n8n supports posting messages to channels, and sending direct messages, with Rocket.Chat.

On this page, you'll find a list of operations the Rocket.Chat node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Rocket.Chat credentials](/integrations/builtin/credentials/rocketchat.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Chat
  * Post a message to a channel or a direct message

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Rocket.Chat node documentation integration templates](https://n8n.io/integrations/rocketchat) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
