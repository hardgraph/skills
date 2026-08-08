> For AI agents: see [llms.txt](/llms.txt) for the complete documentation index. Markdown versions are available by adding .md to a page URL or requesting Accept: text/markdown.

# List active data syncs

```
GET 
/data/list_active_syncs
```

Beta

This endpoint is in beta. Its behavior and response shape may change.

Returns the progress of active data sync (/v1/data/sync).

A data sync is considered active for 3 days after the most recent API call. from `/data/sync` within the past 3 days.

## Request[​](#request "Direct link to request")

## Responses[​](#responses "Direct link to Responses")

* 200
