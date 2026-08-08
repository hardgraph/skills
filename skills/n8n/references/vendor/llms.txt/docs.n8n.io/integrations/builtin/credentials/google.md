> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/google.md).

# Google

This section contains:

* [OAuth2 single service](/integrations/builtin/credentials/google/oauth-single-service.md): Create an OAuth2 credential for a specific service node, such as the Gmail node. Two options exist:
  * [Managed OAuth2](/integrations/builtin/credentials/google/oauth-single-service.md#managed-oauth2): Sign in with Google directly on n8n, with no setup required on the Google Cloud Console. Available for n8n Cloud users only, for certain Google nodes.
  * [Custom OAuth2](/integrations/builtin/credentials/google/oauth-single-service.md#custom-oauth2): Configure an OAuth2 app in the Google Cloud Console and connect it to your n8n credential.
* [OAuth2 API (generic)](/integrations/builtin/credentials/google/oauth-generic.md): Create an OAuth2 credential for use with [custom operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md).
* [Service Account](/integrations/builtin/credentials/google/service-account.md): Create a [Service Account](https://cloud.google.com/iam/docs/service-account-overview) credential for some specific service nodes.
* [Google PaLM and Gemini](/integrations/builtin/credentials/googleai.md): Get a Google Gemini/Google PaLM API key.

## OAuth2 and Service Account <a href="#oauth2-and-service-account" id="oauth2-and-service-account"></a>

There are two authentication methods available for Google services nodes:

* [OAuth2](https://developers.google.com/identity/protocols/oauth2): Recommended because it's more widely available and easier to set up.
* [Service Account](https://cloud.google.com/iam/docs/understanding-service-accounts): Refer to the [Google documentation: Understanding service accounts](https://cloud.google.com/iam/docs/understanding-service-accounts) for guidance on when you need a service account.

### Managed OAuth2 for n8n Cloud users <a href="#managed-oauth2-for-n8n-cloud-users" id="managed-oauth2-for-n8n-cloud-users"></a>

[Managed OAuth2](/integrations/builtin/credentials/google/oauth-single-service.md#managed-oauth2) is available for the following Google nodes, for n8n Cloud users. This provides a simplified credential creation process:

* [Google Calendar](/integrations/builtin/app-nodes/n8n-nodes-base.googlecalendar.md)
* [Google Calendar Trigger](/integrations/builtin/trigger-nodes/n8n-nodes-base.googlecalendartrigger.md)
* [Google Contacts](/integrations/builtin/app-nodes/n8n-nodes-base.googlecontacts.md)
* [Google Docs](/integrations/builtin/app-nodes/n8n-nodes-base.googledocs.md)
* [Google Drive](/integrations/builtin/app-nodes/n8n-nodes-base.googledrive.md)
* [Google Drive Trigger](/integrations/builtin/trigger-nodes/n8n-nodes-base.googledrivetrigger.md)
* [Google Mail](/integrations/builtin/app-nodes/n8n-nodes-base.gmail.md)
* [Google Mail Trigger](/integrations/builtin/trigger-nodes/n8n-nodes-base.gmailtrigger.md)
* [Google Sheets](/integrations/builtin/app-nodes/n8n-nodes-base.googlesheets.md)
* [Google Sheets Trigger](/integrations/builtin/trigger-nodes/n8n-nodes-base.googlesheetstrigger.md)
* [Google Slides](/integrations/builtin/app-nodes/n8n-nodes-base.googleslides.md)
* [Google Tasks](/integrations/builtin/app-nodes/n8n-nodes-base.googletasks.md)

## Compatible nodes <a href="#compatible-nodes" id="compatible-nodes"></a>

Once configured, you can use your credentials to authenticate the following nodes. Most nodes are compatible with OAuth2 authentication. Support for Service Account authentication is limited.

| Node                                                                                                            | OAuth | Service Account |
| --------------------------------------------------------------------------------------------------------------- | :---: | :-------------: |
| [Google Ads](/integrations/builtin/app-nodes/n8n-nodes-base.googleads.md)                                       |   ✅   |        ❌        |
| [Gmail](/integrations/builtin/app-nodes/n8n-nodes-base.gmail.md)                                                |   ✅   |        ⚠️       |
| [Google Analytics](/integrations/builtin/app-nodes/n8n-nodes-base.googleanalytics.md)                           |   ✅   |        ❌        |
| [Google BigQuery](/integrations/builtin/app-nodes/n8n-nodes-base.googlebigquery.md)                             |   ✅   |        ✅        |
| [Google Books](/integrations/builtin/app-nodes/n8n-nodes-base.googlebooks.md)                                   |   ✅   |        ✅        |
| [Google Calendar](/integrations/builtin/app-nodes/n8n-nodes-base.googlecalendar.md)                             |   ✅   |        ❌        |
| [Google Chat](/integrations/builtin/app-nodes/n8n-nodes-base.googlechat.md)                                     |   ✅   |        ✅        |
| [Google Cloud Storage](/integrations/builtin/app-nodes/n8n-nodes-base.googlecloudstorage.md)                    |   ✅   |        ✅        |
| [Google Contacts](/integrations/builtin/app-nodes/n8n-nodes-base.googlecontacts.md)                             |   ✅   |        ❌        |
| [Google Cloud Firestore](/integrations/builtin/app-nodes/n8n-nodes-base.googlecloudfirestore.md)                |   ✅   |        ✅        |
| [Google Cloud Natural Language](/integrations/builtin/app-nodes/n8n-nodes-base.googlecloudnaturallanguage.md)   |   ✅   |        ❌        |
| [Google Cloud Realtime Database](/integrations/builtin/app-nodes/n8n-nodes-base.googlecloudrealtimedatabase.md) |   ✅   |        ❌        |
| [Google Docs](/integrations/builtin/app-nodes/n8n-nodes-base.googledocs.md)                                     |   ✅   |        ✅        |
| [Google Drive](/integrations/builtin/app-nodes/n8n-nodes-base.googledrive.md)                                   |   ✅   |        ✅        |
| [Google Drive Trigger](/integrations/builtin/trigger-nodes/n8n-nodes-base.googledrivetrigger.md)                |   ✅   |        ✅        |
| [Google Perspective](/integrations/builtin/app-nodes/n8n-nodes-base.googleperspective.md)                       |   ✅   |        ❌        |
| [Google Sheets](/integrations/builtin/app-nodes/n8n-nodes-base.googlesheets.md)                                 |   ✅   |        ✅        |
| [Google Slides](/integrations/builtin/app-nodes/n8n-nodes-base.googleslides.md)                                 |   ✅   |        ✅        |
| [Google Tasks](/integrations/builtin/app-nodes/n8n-nodes-base.googletasks.md)                                   |   ✅   |        ❌        |
| [Google Translate](/integrations/builtin/app-nodes/n8n-nodes-base.googletranslate.md)                           |   ✅   |        ✅        |
| [Google Workspace Admin](/integrations/builtin/app-nodes/n8n-nodes-base.gsuiteadmin.md)                         |   ✅   |        ❌        |
| [YouTube](/integrations/builtin/app-nodes/n8n-nodes-base.youtube.md)                                            |   ✅   |        ❌        |

{% hint style="warning" %}
**Gmail and Service Accounts**

Google technically supports Service Accounts for use with Gmail, but it requires enabling domain-wide delegation, which Google discourages, and its behavior can be inconsistent.

n8n recommends using OAuth2 with the Gmail node.
{% endhint %}
