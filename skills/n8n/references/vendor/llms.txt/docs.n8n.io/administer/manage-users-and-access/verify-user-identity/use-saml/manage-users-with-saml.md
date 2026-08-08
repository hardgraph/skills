> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/administer/manage-users-and-access/verify-user-identity/use-saml/manage-users-with-saml.md).

# Manage users with SAML

{% hint style="info" %}
**Feature availability**

* Available on Business and Enterprise plans.
* You need to be an instance owner or admin to enable and configure SAML.
  {% endhint %}

There are some user management tasks that are affected by SAML.

## Exempt users from SAML <a href="#exempt-users-from-saml" id="exempt-users-from-saml"></a>

You can allow users to log in without using SAML. To do this:

1. Go to **Settings** > **Users**.
2. Select the menu icon by the user you want to exempt from SAML.
3. Select **Allow Manual Login**.

## Deleting users <a href="#deleting-users" id="deleting-users"></a>

If you remove a user from your IdP, they remain logged in to n8n. You need to manually remove them from n8n as well. Refer to [Manage users](/administer/manage-users-and-access/add-and-remove-users.md) for guidance on deleting users.
