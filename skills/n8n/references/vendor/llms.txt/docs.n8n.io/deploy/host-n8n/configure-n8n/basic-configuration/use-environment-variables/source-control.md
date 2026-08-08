> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/source-control.md).

# Source control

{% hint style="info" %}
**File-based configuration**

You can add `_FILE` to individual variables to provide their configuration in a separate file. Refer to [Keeping sensitive data in separate files](/deploy/host-n8n/configure-n8n/basic-configuration.md#keeping-sensitive-data-in-separate-files) for more details.
{% endhint %}

n8n uses Git-based source control to support environments. Refer to [Source control and environments](/administer/use-source-control-and-environments/set-up-source-control.md) for more information on how to link a Git repository to an n8n instance and configure your source control.

| Variable                                 | Type   | Default   | Description                                                                                                                                             |
| ---------------------------------------- | ------ | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `N8N_SOURCECONTROL_DEFAULT_SSH_KEY_TYPE` | String | `ed25519` | Set to `rsa` to make RSA the default SSH key type for [Source control setup](/administer/use-source-control-and-environments/set-up-source-control.md). |
