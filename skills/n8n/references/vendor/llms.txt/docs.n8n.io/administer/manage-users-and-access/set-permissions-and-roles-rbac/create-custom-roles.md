> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/administer/manage-users-and-access/set-permissions-and-roles-rbac/create-custom-roles.md).

# Custom roles

{% hint style="info" %}
**Feature availability**

Custom roles are available on Self-hosted Enterprise and Cloud Enterprise plans. Refer to n8n's [pricing page](https://n8n.io/pricing/) for plan details.
{% endhint %}

Custom roles let you define granular permissions beyond the built-in roles. Instead of giving users full Admin access, you can create a role with only the capabilities they need.

n8n has two types of custom roles:

* **Custom project roles**: Define permissions within a specific project, including access to workflows, credentials, folders, and other project resources. Assign them to project members to control what they can do inside that project.

  Refer to [Create custom project roles](/administer/manage-users-and-access/set-permissions-and-roles-rbac/create-custom-roles/create-custom-project-roles.md).
* **Custom instance roles**: Define permissions that apply across the entire n8n instance, such as managing users, tags, API keys, or custom roles themselves. Assign them to users who need specific instance-level capabilities without full Admin access.

  Refer to [Create custom instance roles](/administer/manage-users-and-access/set-permissions-and-roles-rbac/create-custom-roles/create-custom-instance-roles.md).
