> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.youtube.md).

# YouTube

Use the YouTube node to automate work in YouTube, and integrate YouTube with other applications. n8n has built-in support for a wide range of YouTube features, including retrieving and updating channels, as well as creating and deleting playlists.

On this page, you'll find a list of operations the YouTube node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [YouTube credentials](/integrations/builtin/credentials/google.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Channel
  * Retrieve a channel
  * Retrieve all channels
  * Update a channel
  * Upload a channel banner
* Playlist
  * Create a playlist
  * Delete a playlist
  * Get a playlist
  * Retrieve all playlists
  * Update a playlist
* Playlist Item
  * Add an item to a playlist
  * Delete a item from a playlist
  * Get a playlist's item
  * Retrieve all playlist items
* Video
  * Delete a video
  * Get a video
  * Retrieve all videos
  * Rate a video
  * Update a video
  * Upload a video
* Video Category
  * Retrieve all video categories

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse YouTube node documentation integration templates](https://n8n.io/integrations/youtube) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
