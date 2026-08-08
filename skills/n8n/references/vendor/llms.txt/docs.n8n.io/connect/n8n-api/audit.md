> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/connect/n8n-api/audit.md).

# Audit

Operations about security audit

## Generate an audit

> Generate a security audit for your n8n instance.

```json
{"openapi":"3.0.0","info":{"title":"n8n Public API","version":"1.1.1"},"tags":[{"name":"Audit","description":"Operations about security audit"}],"servers":[{"url":"/api/v1","description":"Current n8n instance (self-hosted built-in playground)"},{"url":"{url}/api/v1","description":"Self-hosted n8n instance","variables":{"url":{"default":"https://example.com"}}}],"security":[{"ApiKeyAuth":[]},{"BearerAuth":[]}],"components":{"securitySchemes":{"ApiKeyAuth":{"type":"apiKey","in":"header","name":"X-N8N-API-KEY"},"BearerAuth":{"type":"http","scheme":"bearer","bearerFormat":"JWT"}},"schemas":{"audit":{"type":"object","properties":{"Credentials Risk Report":{"type":"object"},"Database Risk Report":{"type":"object"},"Filesystem Risk Report":{"type":"object"},"Nodes Risk Report":{"type":"object"},"Instance Risk Report":{"type":"object"}}}},"responses":{"unauthorized":{"description":"Unauthorized"}}},"paths":{"/audit":{"post":{"tags":["Audit"],"summary":"Generate an audit","description":"Generate a security audit for your n8n instance.","requestBody":{"required":false,"content":{"application/json":{"schema":{"type":"object","properties":{"additionalOptions":{"type":"object","properties":{"daysAbandonedWorkflow":{"type":"integer","description":"Days for a workflow to be considered abandoned if not executed"},"categories":{"type":"array","items":{"type":"string","enum":["credentials","database","nodes","filesystem","instance"]}}}}}}}}},"responses":{"200":{"description":"Operation successful.","content":{"application/json":{"schema":{"$ref":"#/components/schemas/audit"}}}},"401":{"$ref":"#/components/responses/unauthorized"},"500":{"description":"Internal server error."}}}}}}
```
