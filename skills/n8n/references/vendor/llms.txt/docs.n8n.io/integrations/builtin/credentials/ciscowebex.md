> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/ciscowebex.md).

# Webex by Cisco credentials

You can use these credentials to authenticate the following nodes:

* [Webex by Cisco](/integrations/builtin/app-nodes/n8n-nodes-base.ciscowebex.md)
* [Webex by Cisco Trigger](/integrations/builtin/trigger-nodes/n8n-nodes-base.ciscowebextrigger.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create a [Webex by Cisco](https://www.webex.com/) account (this should automatically get you [developer account access](https://developer.webex.com)).

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* OAuth2

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Webex's API documentation](https://developer.webex.com/docs/getting-started) for more information about the service.

## Using OAuth2 <a href="#using-oauth2" id="using-oauth2"></a>

{% hint style="info" %}
**Note for n8n Cloud users**

You'll only need to enter the Credentials Name and select the **Connect my account** button in the OAuth credential to connect your Webex by Cisco account to n8n.
{% endhint %}

Should you need to configure OAuth2 from scratch, you'll need to create an integration to use this credential. Refer to the instructions in the [Webex Registering your Integration documentation](https://developer.webex.com/docs/integrations#registering-your-integration) to begin.

n8n recommends using the following **Scopes** for your integration:

* `spark:rooms_read`
* `spark:messages_write`
* `spark:messages_read`
* `spark:memberships_read`
* `spark:memberships_write`
* `meeting:recordings_write`
* `meeting:recordings_read`
* `meeting:preferences_read`
* `meeting:schedules_write`
* `meeting:schedules_read`
