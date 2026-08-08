> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/code-in-n8n/use-built-in-shortcuts/jmespath.md).

# JMESPath

This is an n8n-provided method for working with the [JMESPath](/build/work-with-data/handle-special-data-types/query-json-data.md) library.

{% hint style="info" %}
**Python support**

You can use Python in the Code node. It isn't available in expressions.
{% endhint %}

{% tabs %}
{% tab title="JavaScript" %}

| Method        | Description                                       | Available in Code node? |
| ------------- | ------------------------------------------------- | :---------------------: |
| `$jmespath()` | Perform a search on a JSON object using JMESPath. |            ✅            |
| {% endtab %}  |                                                   |                         |

{% tab title="Python" %}

| Method        | Description                                       |
| ------------- | ------------------------------------------------- |
| `_jmespath()` | Perform a search on a JSON object using JMESPath. |
| {% endtab %}  |                                                   |
| {% endtabs %} |                                                   |
