> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/privacy-and-security/what-you-can-do.md).

# What you can do

It's also your responsibility as a customer to ensure you are securing your code and data. This document lists some steps you can take.

## All users <a href="#all-users" id="all-users"></a>

* Report security issues and [terms of service](https://n8n.io/legal/#terms) violations to <security@n8n.io>.
* If more than one person uses your n8n instance, set up [User management](/administer/manage-users-and-access.md) and follow the [Best practices](/administer/manage-users-and-access/follow-best-practices.md).
* Use OAuth to connect integrations whenever possible.

## Self-hosted users <a href="#self-hosted-users" id="self-hosted-users"></a>

If you self-host n8n, there are additional steps you can take:

* Set up a reverse proxy to handle TLS, ensuring data is encrypted in transit.
* Ensure data is encrypted at rest by using encrypted partitions, or encryption at the hardware level, and ensuring n8n and its database is written to that location.
* Run a [Security audit](/deploy/host-n8n/configure-n8n/security/run-security-audits.md).
* Be aware of the [Risks](/integrations/community-nodes/risks.md) when installing community nodes, or choose to disable them.
* Make sure users can't import external modules in the Code node. Refer to [Environment variables | Nodes](/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/nodes.md) for more information.
* Choose to exclude certain nodes. For example, you can disable nodes like Execute Command or SSH. Refer to [Environment variables | Nodes](/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/nodes.md) for more information.
* For maximum privacy, you can [Isolate n8n](/deploy/host-n8n/configure-n8n/basic-configuration/configuration-examples/isolate-n8n.md).

### GDPR for self-hosted users <a href="#gdpr-for-self-hosted-users" id="gdpr-for-self-hosted-users"></a>

If you self-host n8n, you are responsible for deleting user data. If you need to delete data on behalf of one of your users, you can delete the respective execution. n8n recommends configuring n8n to prune execution data automatically every few days to avoid effortful GDPR request handling as much as possible. Configure this using the `EXECUTIONS_DATA_MAX_AGE` environment variable. Refer to [Environment variables](/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables.md) for more information.
