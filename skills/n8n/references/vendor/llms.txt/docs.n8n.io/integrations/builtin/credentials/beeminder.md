> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/beeminder.md).

# Beeminder credentials

You can use these credentials to authenticate the following node:

* [Beeminder](/integrations/builtin/app-nodes/n8n-nodes-base.beeminder.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create a [Beeminder](https://www.beeminder.com/) account.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* API user token

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Beeminder's API documentation](https://api.beeminder.com/#beeminder-api-reference) for more information about the service.

## Using API user token <a href="#using-api-user-token" id="using-api-user-token"></a>

To configure this credential, you'll need:

* A **User** name: Should match the user who the Auth Token is generated for.
* A personal **Auth Token** for that user. Generate this using either method below:
  * In the GUI: From the [Apps & API](https://help.beeminder.com/article/110-apps-and-api#API-token) option within **Account Settings**
  * In the API: From hitting the [`auth_token` API endpoint](https://api.beeminder.com/#auth)
