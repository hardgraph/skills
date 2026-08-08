> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/configure-n8n/security/disable-the-public-api.md).

# Disable the public API

The [n8n public REST API](/connect/n8n-api.md) allows you to programmatically perform many of the same tasks as you can in the n8n GUI.

If you don't plan on using this API, n8n recommends disabling it to improve the security of your n8n installation.

To disable the [public REST API](/connect/n8n-api.md), set the `N8N_PUBLIC_API_DISABLED` environment variable to `true`, for example:

```bash
export N8N_PUBLIC_API_DISABLED=true
```

## Disable the API playground <a href="#disable-the-api-playground" id="disable-the-api-playground"></a>

To disable the [API playground](/connect/n8n-api/use-an-api-playground.md), set the `N8N_PUBLIC_API_SWAGGERUI_DISABLED` environment variable to `true`, for example:

```bash
export N8N_PUBLIC_API_SWAGGERUI_DISABLED=true
```

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Deployment environment variables](/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/deployment.md) for more information on these environment variables.

Refer to [Configuration](/deploy/host-n8n/configure-n8n/basic-configuration.md) for more information on setting environment variables.
