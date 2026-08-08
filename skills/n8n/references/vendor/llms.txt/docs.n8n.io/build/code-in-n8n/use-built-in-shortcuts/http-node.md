> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/code-in-n8n/use-built-in-shortcuts/http-node.md).

# HTTP node

Variables for working with HTTP node requests and responses when using pagination.

Refer to [HTTP Request](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) for guidance on using the HTTP node, including configuring pagination.

Refer to [HTTP Request node cookbook | Pagination](/build/code-in-n8n/cookbook/http-request-node/pagination.md) for example pagination configurations.

{% hint style="info" %}
**HTTP node only**

These variables are for use in expressions in the HTTP node. You can't use them in other nodes.
{% endhint %}

| Variable     | Description                                                                                                                                                                                  |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$pageCount` | The pagination count. Tracks how many pages the node has fetched.                                                                                                                            |
| `$request`   | The request object sent by the HTTP node.                                                                                                                                                    |
| `$response`  | The response object from the HTTP call. Includes `$response.body`, `$response.headers`, and `$response.statusCode`. The contents of `body` and `headers` depend on the data sent by the API. |
