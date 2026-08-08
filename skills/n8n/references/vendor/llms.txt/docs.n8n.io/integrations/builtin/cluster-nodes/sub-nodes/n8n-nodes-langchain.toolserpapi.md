> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.toolserpapi.md).

# SerpApi (Google Search)

{% hint style="warning" %}
**Deprecated**

This node is deprecated, and will be removed in a future version. Use the verified **SerpApi Official** community node instead.
{% endhint %}

The SerpAPI node allows an agent[^1] in your workflow to call Google's Search API.

On this page, you'll find the node parameters for the SerpAPI node, and links to more resources.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/serp.md).
{% endhint %}

{% hint style="info" %}
**Parameter resolution in sub-nodes**

Sub-nodes behave differently to other nodes when processing multiple items using an expression.

Most nodes, including root nodes, take any number of items as input, process these items, and output the results. You can use expressions to refer to input items, and the node resolves the expression for each item in turn. For example, given an input of five `name` values, the expression `{{ $json.name }}` resolves to each name in turn.

In sub-nodes, the expression always resolves to the first item. For example, given an input of five `name` values, the expression `{{ $json.name }}` always resolves to the first name.
{% endhint %}

## Node options <a href="#node-options" id="node-options"></a>

* **Country**: Enter the country code you'd like to use. Refer to [Google GL Parameter: Supported Google Countries](https://serpapi.com/google-countries) for supported countries and country codes.
* **Device**: Select the device to use to get the search results.
* **Explicit Array**: Choose whether to force SerpApi to fetch the Google results even if a cached version is already present (turned on) or not (turned off).
* **Google Domain**: Enter the Google Domain to use. Refer to [Supported Google Domains](https://serpapi.com/google-domains) for supported domains.
* **Language**: Enter the language code you'd like to use. Refer to [Google HL Parameter: Supported Google Languages](https://serpapi.com/google-languages) for supported languages and language codes.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse SerpApi (Google Search) node documentation integration templates](https://n8n.io/integrations/serpapi) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Serp's documentation](https://serpapi.com/search-api) for more information about the service. You can also view [LangChain's documentation on their Serp integration](https://js.langchain.com/docs/integrations/tools/serpapi/).

View n8n's [Advanced AI](/build/integrate-ai.md) documentation.

[^1]: AI agents are artificial intelligence systems capable of responding to requests, making decisions, and performing real-world tasks for users. They use large language models (LLMs) to interpret user input and make decisions about how to best process requests using the information and resources they have available.
