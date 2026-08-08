> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/philipshue.md).

# Philips Hue credentials

You can use these credentials to authenticate the following nodes:

* [Philips Hue](/integrations/builtin/app-nodes/n8n-nodes-base.philipshue.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create a [Philips Hue](https://www.philips-hue.com/en-us) account.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* OAuth2

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Philips Hue's CLIP API documentation](https://developers.meethue.com/develop/hue-api-v2/api-reference/) for more information about the service.

## Using OAuth2 <a href="#using-oauth2" id="using-oauth2"></a>

{% hint style="info" %}
**Note for n8n Cloud users**

Cloud users don't need to provide connection details. Select **Connect my account** to connect through your browser.
{% endhint %}

If you're using the built-in OAuth connection, you don't need to enter an **APP ID**.

If you need to configure OAuth2 from scratch, you'll need a [Philips Hue developer](https://developers.meethue.com/) account

Create a new remote app on the [Add new Hue Remote API app](https://developers.meethue.com/add-new-hue-remote-api-app/) page.

Use these settings for your app:

* Copy the **OAuth Callback URL** from n8n and add it as a **Callback URL**.
* Copy the **AppId**, **ClientId**, and **ClientSecret** and enter these in the corresponding fields in n8n.
