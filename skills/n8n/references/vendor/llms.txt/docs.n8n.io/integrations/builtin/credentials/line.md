> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/line.md).

# Line credentials

{% hint style="warning" %}
**Deprecated: End of service**

LINE Notify is discontinuing service as of April 1st 2025 and this node will no longer work after that date. View LINE Notify's [end of service announement](https://notify-bot.line.me/closing-announce) for more information.
{% endhint %}

You can use these credentials to authenticate the following nodes:

* [Line](/integrations/builtin/app-nodes/n8n-nodes-base.line.md)

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* Notify OAuth2

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Line Notify's API documentation](https://notify-bot.line.me/doc/en/) for more information about the service.

## Using Notify OAuth2 <a href="#using-notify-oauth2" id="using-notify-oauth2"></a>

To configure this credential, you'll need a [Line](https://line.me/en/) account and:

* A **Client ID**
* A **Client Secret**

To generate both, connect Line with [Line Notify](https://notify-bot.line.me/en/). Then:

1. Open the Line Notify page to [add a new service](https://notify-bot.line.me/my/services/new).
2. Enter a **Service name**. This name displays when someone tries to connect to the service.
3. Enter a **Service description**.
4. Enter a **Service URL**
5. Enter your **Company/Enterprise**.
6. Select your **Country/region**.
7. Enter your name or team name as the **Representative**.
8. Enter a valid **Email address**. Line will verify this email address before the service is fully registered. Use an email address you have ready access to.
9. Copy the **OAuth Redirect URL** from your n8n credential and enter it as the **Callback URL** in Line Notify.
10. Select **Agree and continue** to agree to the terms of service.
11. Verify the information you entered is correct and select **Add**.
12. Check your email and open the Line Notify Registration URL to verify your email address.
13. Once verification is complete, open [**My services**](https://notify-bot.line.me/my/services/).
14. Select the service you just added.
15. Copy the **Client ID** and enter it in your n8n credential.
16. Select the option to **Display** the **Client Secret**. Copy the **Client Secret** and enter it in your n8n credential.
17. In n8n, select **Connect my account** and follow the on-screen prompts to finish the credential.

Refer to the Authentication section of [Line Notify's API documentation](https://notify-bot.line.me/doc/en/) for more information.
