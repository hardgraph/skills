> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/acuityscheduling.md).

# Acuity Scheduling credentials

You can use these credentials to authenticate the following nodes:

* [Acuity Scheduling Trigger](/integrations/builtin/trigger-nodes/n8n-nodes-base.acuityschedulingtrigger.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create an [Acuity Scheduling](https://acuityscheduling.com/) account.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* API key
* OAuth2

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Acuity's API documentation](https://developers.acuityscheduling.com/reference/quick-start) for more information about working with the service.

## Using API key <a href="#using-api-key" id="using-api-key"></a>

To configure this credential, you'll need:

* A numeric **User ID**
* An **API Key**

Refer to the [Acuity API Quick Start authentication instructions](https://developers.acuityscheduling.com/reference/quick-start#authentication) to generate an API key and view your User ID.

## Using OAuth2 <a href="#using-oauth2" id="using-oauth2"></a>

{% hint style="info" %}
**Note for n8n Cloud users**

Cloud users don't need to provide connection details. Select **Connect my account** to connect through your browser.
{% endhint %}

If you need to set this up from scratch, complete the [Acuity OAuth2 Account Registration page](https://acuityscheduling.com/oauth2/register). Use the **Client ID** and **Client Secret** provided from that registration.
