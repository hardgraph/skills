> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/googleai.md).

# Google Gemini(PaLM) credentials

You can use these credentials to authenticate the following nodes:

* [Embeddings Google Gemini](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.embeddingsgooglegemini.md)
* [Google Gemini](/integrations/builtin/app-nodes/n8n-nodes-langchain.googlegemini.md)
* [Google Gemini Chat Model](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmchatgooglegemini.md)
* [Embeddings Google PaLM](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.embeddingsgooglepalm.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Create a [Google Cloud](https://cloud.google.com/) account.
* Create a [Google Cloud Platform project](https://developers.google.com/workspace/marketplace/create-gcp-project).

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* Gemini(PaLM) API key

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Google's Gemini API documentation](https://ai.google.dev/gemini-api/docs) for more information about the service.

View n8n's [Advanced AI](/build/integrate-ai.md) documentation.

## Using Gemini(PaLM) API key <a href="#using-geminipalm-api-key" id="using-geminipalm-api-key"></a>

To configure this credential, you'll need:

* The API **Host** URL: Both PaLM and Gemini use the default `https://generativelanguage.googleapis.com`.
* An **API Key**: Create a key in [Google AI Studio](https://aistudio.google.com/apikey).

{% hint style="warning" %}
**Custom hosts not supported**

The related nodes don't yet support custom hosts or proxies for the API host and must use `https://generativelanguage.googleapis.com`.
{% endhint %}

To create an API key:

1. Go to the API Key page in Google AI Studio: <https://aistudio.google.com/apikey>.
2. Select **Create API Key**.
3. You can choose whether to **Create API key in new project** or search for an existing Google Cloud project to **Create API key in existing project**.
4. Copy the generated API key and add it to your n8n credential.
