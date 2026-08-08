> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/configure-n8n/basic-configuration/configuration-examples/configure-the-base-url.md).

# Configure the Base URL

{% hint style="warning" %}
**Requires manual UI build**

This use case involves configuring the `VUE_APP_URL_BASE_API` environmental variable which requires a manual build of the `n8n-editor-ui` package. You can't use it with the default n8n Docker image where the default setting for this variable is `/`, meaning that it uses the root-domain.
{% endhint %}

You can configure the Base URL that the front end uses to connect to the back end's REST API. This is relevant when you want to host n8n's front end and back end separately.

```bash
export VUE_APP_URL_BASE_API=https://n8n.example.com/
```

Refer to [Environment variables reference](/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/deployment.md) for more information on this variable.
