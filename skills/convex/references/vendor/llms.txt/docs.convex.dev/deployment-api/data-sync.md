> For AI agents: see [llms.txt](/llms.txt) for the complete documentation index. Markdown versions are available by adding .md to a page URL or requesting Accept: text/markdown.

# Data sync

```
POST 
/data/sync
```

Beta

This endpoint is in beta. Its behavior and response shape may change.

Paginated streamable export of some or all of a deployment's data.

Call this endpoint repeatedly, passing the opaque `pagination.nextCursor` from each response back in the next request as `cursor`. Omit `cursor` on the first call.

To do a one time data sync, keep fetching pages until reaching an `upToDate` page. For a continuous streaming export, continue fetching pages periodically. It's recommended to sleep between `upToDate` pages to reduce overhead.

The caller must have the `deployment:data:view` permission.

## Request[​](#request "Direct link to request")

## Responses[​](#responses "Direct link to Responses")

* 200
