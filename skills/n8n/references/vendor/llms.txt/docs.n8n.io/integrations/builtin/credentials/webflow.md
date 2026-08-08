> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/webflow.md).

# Webflow credentials

You can use these credentials to authenticate the following nodes:

* [Webflow](/integrations/builtin/app-nodes/n8n-nodes-base.webflow.md)
* [Webflow Trigger](/integrations/builtin/trigger-nodes/n8n-nodes-base.webflowtrigger.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Create a [Webflow](https://webflow.com/) account.
* [Create a site](https://developers.webflow.com/data/reference/structure-1#sites): Required for API access token authentication only.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* API access token
* OAuth2

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Webflow's API documentation](https://developers.webflow.com/data/reference/rest-introduction) for more information about the service.

## Using API access token <a href="#using-api-access-token" id="using-api-access-token"></a>

To configure this credential, you'll need:

* A Site **Access Token**: Access tokens are site-specific. Go to your site's **Site Settings > Apps & integrations > API access** and select **Generate API token**. Refer to [Get a Site Token](https://developers.webflow.com/data/v1.0.0/docs/get-a-site-token) for more information.

## Using OAuth2 <a href="#using-oauth2" id="using-oauth2"></a>

{% hint style="info" %}
**Note for n8n Cloud users**

Cloud users don't need to provide connection details. Select **Connect my account** to connect through your browser.
{% endhint %}

If you need to configure OAuth2 from scratch, [register an application](https://developers.webflow.com/data/docs/register-an-app) in your workspace.

Use these settings for your application:

* Copy the **OAuth callback URL** from n8n and add it as a **Redirect URI** in your application.
* Once you've created your application, copy the **Client ID** and **Client Secret** and enter them in your n8n credential.
* If you are using the Webflow Data API V1 (deprecated), enable the **Legacy** toggle. Otherwise, leave this inactive.

Refer to [OAuth](https://developers.webflow.com/data/reference/oauth-app) for more information on Webflow's OAuth web flow.
