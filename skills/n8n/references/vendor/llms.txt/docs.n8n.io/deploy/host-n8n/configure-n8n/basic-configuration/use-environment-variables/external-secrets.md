> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/external-secrets.md).

# External secrets

{% hint style="info" %}
**File-based configuration**

You can add `_FILE` to individual variables to provide their configuration in a separate file. Refer to [Keeping sensitive data in separate files](/deploy/host-n8n/configure-n8n/basic-configuration.md#keeping-sensitive-data-in-separate-files) for more details.
{% endhint %}

You can use an external secrets store to manage credentials for n8n. Refer to [External secrets](/administer/manage-credentials/use-external-secret-stores.md) for details.

| Variable                               | Type   | Default           | Description                                         |
| -------------------------------------- | ------ | ----------------- | --------------------------------------------------- |
| `N8N_EXTERNAL_SECRETS_UPDATE_INTERVAL` | Number | `300` (5 minutes) | How often (in seconds) to check for secret updates. |
