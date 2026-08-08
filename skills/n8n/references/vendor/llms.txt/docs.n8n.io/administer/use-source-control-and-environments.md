> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/administer/use-source-control-and-environments.md).

# Use source control and environments

{% hint style="info" %}
**Feature availability**

* Available on Business and Enterprise plans.
* You must be an n8n instance owner or instance admin to enable and configure source control.
* Instance owners and instance admins can push changes to and pull changes from the connected repository.
* Project admins can push changes to the connected repository. They can't pull changes from the repository.
  {% endhint %}

n8n uses Git-based source control to support environments. Linking your n8n instances to a Git repository lets you create multiple n8n environments, backed by Git branches.

In this section:

* [Understand](/administer/use-source-control-and-environments/understand-source-control.md):
  * [Environments in n8n](/administer/use-source-control-and-environments/work-with-environments.md): The purpose of environments, and how they work in n8n.
  * [Git and n8n](/administer/use-source-control-and-environments/use-git-in-n8n.md): How n8n uses Git.
  * [Branch patterns](/administer/use-source-control-and-environments/choose-branching-patterns.md): The possible relationships between n8n instances and Git branches.
* [Set up source control for environments](/administer/use-source-control-and-environments/set-up-source-control.md): How to connect your n8n instance to Git.
* Using:
  * [Push and pull](/administer/use-source-control-and-environments/push-and-pull-changes.md): Send work to Git, and fetch work from Git to your instance.
  * [Copy work between environments](/administer/use-source-control-and-environments/move-work-between-environments.md): How to copy work between different n8n instances.
* [Tutorial: Create environments with source control](/administer/use-source-control-and-environments/tutorial-create-environments-with-source-control.md): An end-to-end tutorial, setting up environments using n8n's recommended configurations.

Related sections:

* [Variables](/build/code-in-n8n/define-custom-variables.md): reusable values.
* [External secrets](/administer/manage-credentials/use-external-secret-stores.md): manage credentials[^1] with an external secrets vault.

[^1]: In n8n, credentials store authentication information to connect with specific apps and services. After creating credentials with your authentication information (username and password, API key, OAuth secrets, etc.), you can use the associated app node to interact with the service.
