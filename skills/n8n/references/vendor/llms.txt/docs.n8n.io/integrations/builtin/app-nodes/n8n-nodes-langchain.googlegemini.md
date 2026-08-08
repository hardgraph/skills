> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-langchain.googlegemini.md).

# Google Gemini

Use the Google Gemini node to automate work in Google Gemini and integrate Google Gemini with other applications. n8n has built-in support for a wide range of Google Gemini features, including working with audio, videos, images, documents, and files to analyze, generate, and transcribe.

On this page, you'll find a list of operations the Google Gemini node supports, and links to more resources.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/googleai.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Audio:
  * Analyze Audio: Take in audio and answer questions about it.
  * Transcribe a Recording: Transcribes audio into text.
* Document:
  * Analyze Document: Take in documents and answer questions about them.
* File Search:
  * Create File Search Store: Create a new File Search store for RAG (Retrieval Augmented Generation)
  * Delete File Search Store: Delete File Search Store
  * List File Search Stores: List all File Search stores owned by the user
  * Upload to File Search Store: Upload a file to a File Search store for RAG (Retrieval Augmented Generation)
* Image:
  * Analyze Image: Take in images and answer questions about them.
  * Generate an Image: Creates an image from a text prompt.
  * Edit Image: Upload one or more images and apply edits based on a prompt
* Media File:
  * Upload Media File: Upload a file to the Google Gemini API for later user.
* Text:
  * Message a Model: Create a completion with a Google Gemini model.
* Video:
  * Analyze Video: Take in videos and answer questions about them.
  * Generate a Video: Creates a video from a text prompt.
  * Download Video: Download a generated video from the Google Gemini API using a URL.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Google Gemini node documentation integration templates](https://n8n.io/integrations/google-gemini) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Google Gemini's documentation](https://ai.google.dev/gemini-api/docs) for more information about the service.

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
