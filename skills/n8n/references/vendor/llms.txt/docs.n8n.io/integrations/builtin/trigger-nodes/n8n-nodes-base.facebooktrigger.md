> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/trigger-nodes/n8n-nodes-base.facebooktrigger.md).

# Facebook Trigger

[Facebook](https://www.facebook.com/) is a social networking site to connect and share with family and friends online.

Use the Facebook Trigger node to trigger a workflow when events occur in Facebook.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/facebookapp.md).
{% endhint %}

{% hint style="info" %}
**Examples and templates**

For usage examples and templates to help you get started, refer to n8n's [Facebook Trigger integrations](https://n8n.io/integrations/facebook-trigger/) page.
{% endhint %}

## Objects <a href="#objects" id="objects"></a>

* [**Ad Account**](/integrations/builtin/trigger-nodes/n8n-nodes-base.facebooktrigger/ad-account.md): Get updates for certain ads changes.
* [**Application**](/integrations/builtin/trigger-nodes/n8n-nodes-base.facebooktrigger/application.md): Get updates sent to the application.
* [**Certificate Transparency**](/integrations/builtin/trigger-nodes/n8n-nodes-base.facebooktrigger/certificate-transparency.md): Get updates when new security certificates are generated for your subscribed domains, including new certificates and potential phishing attempts.
* Activity and events in a [**Group**](/integrations/builtin/trigger-nodes/n8n-nodes-base.facebooktrigger/group.md)
* [**Instagram**](/integrations/builtin/trigger-nodes/n8n-nodes-base.facebooktrigger/instagram.md): Get updates when someone comments on the Media objects of your app users; @mentions your app users; or when Stories of your app users expire.
* [**Link**](/integrations/builtin/trigger-nodes/n8n-nodes-base.facebooktrigger/link.md): Get updates about the links for rich previews by an external provider
* [**Page**](/integrations/builtin/trigger-nodes/n8n-nodes-base.facebooktrigger/page.md) updates
* [**Permissions**](/integrations/builtin/trigger-nodes/n8n-nodes-base.facebooktrigger/permissions.md): Updates when granting or revoking permissions
* [**User**](/integrations/builtin/trigger-nodes/n8n-nodes-base.facebooktrigger/user.md) profile updates
* [**WhatsApp Business Account**](/integrations/builtin/trigger-nodes/n8n-nodes-base.facebooktrigger/whatsapp.md)<br>

  <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p><strong>Use WhatsApp Trigger</strong></p><p>n8n recommends using the <a href="/pages/J8yXDhtvvHCnnPcLYPke">WhatsApp Trigger node</a> with the <a href="/pages/Qz7Mc0JBVjrGbx67u8hr">WhatsApp credentials</a> instead of the Facebook Trigger node for these events. The WhatsApp Trigger node has more events to listen to.</p></div>
* [**Workplace Security**](/integrations/builtin/trigger-nodes/n8n-nodes-base.facebooktrigger/workplace-security.md)

For each **Object**, use the **Field Names or IDs** dropdown to select more details on what data to receive. Refer to the linked pages for more details.

## Related resources <a href="#related-resources" id="related-resources"></a>

View [example workflows and related content](https://n8n.io/integrations/facebook-trigger/) on n8n's website.

Refer to Meta's [Graph API documentation](https://developers.facebook.com/docs/graph-api/webhooks/reference) for details about their API.
