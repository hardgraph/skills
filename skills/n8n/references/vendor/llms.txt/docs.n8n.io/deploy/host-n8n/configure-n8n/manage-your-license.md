> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/configure-n8n/manage-your-license.md).

# Manage your license

To enable certain licensed features, you must first activate your license. You can do this either through the UI or by setting environment variables.

## Add a license key using the UI <a href="#add-a-license-key-using-the-ui" id="add-a-license-key-using-the-ui"></a>

In your n8n instance:

1. Log in as **Admin** or **Owner**.
2. Select **Settings** > **Usage and plan**.
3. Select **Enter activation key**.
4. Paste in your license key.
5. Select **Activate**.

## Add a license key using an environment variables <a href="#add-a-license-key-using-an-environment-variables" id="add-a-license-key-using-an-environment-variables"></a>

In your n8n configuration, set `N8N_LICENSE_ACTIVATION_KEY` to your license key. If the instance already has an activated license, this variable will have no effect.

Refer to [Environment variables](/deploy/host-n8n/configure-n8n/basic-configuration.md) to learn more about configuring n8n.

## Allowlist the license server IP addresses <a href="#allowlist-the-license-server-ip-addresses" id="allowlist-the-license-server-ip-addresses"></a>

n8n uses Cloudflare to host the license server. As the specific IP addresses can change, you need to allowlist the [full range of Cloudflare IP addresses](https://www.cloudflare.com/ips/) to ensure n8n can always reach the license server.
