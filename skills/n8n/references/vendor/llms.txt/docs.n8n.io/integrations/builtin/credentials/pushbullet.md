> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/pushbullet.md).

# Pushbullet credentials

You can use these credentials to authenticate the following nodes:

* [Pushbullet](/integrations/builtin/app-nodes/n8n-nodes-base.pushbullet.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create a [Pushbullet](https://www.pushbullet.com/) account.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* OAuth2

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Pushbullet's API documentation](https://docs.pushbullet.com/) for more information about the service.

## Using OAuth2 <a href="#using-oauth2" id="using-oauth2"></a>

To configure this credential, you'll need:

* A **Client ID**: Generated when you create a Pushbullet app, also known as an OAuth client.
* A **Client Secret**: Generated when you create a Pushbullet app, also known as an OAuth client.

To generate the **Client ID** and **Client Secret**, go to the [create client](https://www.pushbullet.com/create-client) page. Copy the **OAuth Redirect URL** from n8n and add this as your **redirect\_uri** for the app/client. Use the **client\_id** and **client\_secret** from the OAuth Client in your n8n credential.

Refer to Pushbullet's [OAuth2 Guide](https://docs.pushbullet.com/#oauth2) for more information.

{% hint style="info" %}
**Pushbullet OAuth test link**

Pushbullet offers a test link during the client creation process described above. This link isn't compatible with n8n. To verify the authentication works, use the **Connect my account** button in n8n.
{% endhint %}
