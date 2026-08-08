> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/credentials.md).

# Credentials

{% hint style="info" %}
**File-based configuration**

You can add `_FILE` to individual variables to provide their configuration in a separate file. Refer to [Keeping sensitive data in separate files](/deploy/host-n8n/configure-n8n/basic-configuration.md#keeping-sensitive-data-in-separate-files) for more details.
{% endhint %}

Enable credential overwrites using the following environment variables. Refer to [Credential overwrites](/administer/manage-credentials/credential-overwrites.md) for details.

| Variable                                                                 | Type    | Default          | Description                                                                                                                                                               |
| ------------------------------------------------------------------------ | ------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><code>CREDENTIALS\_OVERWRITE\_DATA</code><br>/<code>\_FILE</code></p> | \*      | -                | Overwrites for credentials.                                                                                                                                               |
| `CREDENTIALS_OVERWRITE_ENDPOINT`                                         | String  | -                | The API endpoint to fetch credentials.                                                                                                                                    |
| `CREDENTIALS_OVERWRITE_PERSISTENCE`                                      | Boolean | `false`          | Enable database persistence for credential overwrites. Required for multi-instance or queue mode to propagate overwrites to workers through a publish/subscribe approach. |
| `CREDENTIALS_DEFAULT_NAME`                                               | String  | `My credentials` | The default name for credentials.                                                                                                                                         |
