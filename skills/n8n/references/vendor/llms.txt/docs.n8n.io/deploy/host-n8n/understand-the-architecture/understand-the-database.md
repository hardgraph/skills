> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/understand-the-architecture/understand-the-database.md).

# Understand the database

This page describes the purpose of each table in the n8n database.

## Database and query technology <a href="#database-and-query-technology" id="database-and-query-technology"></a>

By default, n8n uses SQLite as the database. If you are using another database the structure will be similar, but the data-types may be different depending on the database.

n8n uses [TypeORM](https://github.com/typeorm/typeorm) for queries and migrations.

To inspect the n8n database, you can use [DBeaver](https://dbeaver.io), which is an open-source universal database tool.

## Tables <a href="#tables" id="tables"></a>

These are the tables n8n creates during setup.

### auth\_identity <a href="#authidentity" id="authidentity"></a>

Stores details of external authentication providers when using [SAML](/administer/manage-users-and-access/verify-user-identity/use-saml.md).

### auth\_provider\_sync\_history <a href="#authprovidersynchistory" id="authprovidersynchistory"></a>

Stores the history of a SAML connection.

### credentials\_entity <a href="#credentialsentity" id="credentialsentity"></a>

Stores the credentials[^1] used to authenticate with integrations.

### event\_destinations <a href="#eventdestinations" id="eventdestinations"></a>

Contains the destination configurations for [Log streaming](/administer/observe-and-log/stream-logs-to-external-systems.md).

### execution\_data <a href="#executiondata" id="executiondata"></a>

Contains the workflow at time of running, and the execution data.

### execution\_entity <a href="#executionentity" id="executionentity"></a>

Stores all saved workflow executions. Workflow settings can affect which executions n8n saves.

### execution\_metadata <a href="#executionmetadata" id="executionmetadata"></a>

Stores [Custom executions data](/build/understand-workflows/understand-executions/customize-executions-data.md).

### installed\_nodes <a href="#installednodes" id="installednodes"></a>

Lists the [community nodes](/integrations/community-nodes/installation-and-management.md) installed in your n8n instance.

### installed\_packages <a href="#installedpackages" id="installedpackages"></a>

Details of npm community nodes packages installed in your n8n instance. [installed\_nodes](#installed_nodes) lists each individual node. `installed_packages` lists npm packages, which may contain more than one node.

### migrations <a href="#migrations" id="migrations"></a>

A log of all database migrations. Read more about [Migrations](https://typeorm.io/docs/advanced-topics/migrations/) in TypeORM's documentation.

### project <a href="#project" id="project"></a>

Lists the [projects](/administer/manage-users-and-access/set-permissions-and-roles-rbac/organize-work-in-projects.md) in your instance.

### project\_relation <a href="#projectrelation" id="projectrelation"></a>

Describes the relationship between a user and a [project](/administer/manage-users-and-access/set-permissions-and-roles-rbac/organize-work-in-projects.md), including the user's [role type](/administer/manage-users-and-access/set-permissions-and-roles-rbac/see-available-roles.md).

### role <a href="#role" id="role"></a>

Not currently used. For use in future work on custom roles.

### settings <a href="#settings" id="settings"></a>

Records custom instance settings. These are settings that you can't control using environment variables. They include:

* Whether the instance owner is set up
* Whether the user chose to skip owner and user management setup
* Whether certain types of authentication, including SAML and LDAP, are on
* License key

### shared\_credentials <a href="#sharedcredentials" id="sharedcredentials"></a>

Maps credentials to users.

### shared\_workflow <a href="#sharedworkflow" id="sharedworkflow"></a>

Maps workflows to users.

### tag\_entity <a href="#tagentity" id="tagentity"></a>

All workflow tags created in the n8n instance. This table lists the tags. [workflows\_tags](#workflows_tags) records which workflows have which tags.

### user <a href="#user" id="user"></a>

Contains user data.

### variables <a href="#variables" id="variables"></a>

Store [variables](/build/code-in-n8n/define-custom-variables.md).

### webhook\_entity <a href="#webhookentity" id="webhookentity"></a>

Records the active webhooks in your n8n instance's workflows. This isn't just webhooks uses in the Webhook node. It includes all active webhooks used by any trigger node.

### workflow\_entity <a href="#workflowentity" id="workflowentity"></a>

Your n8n instance's saved workflows.

### workflow\_history <a href="#workflowhistory" id="workflowhistory"></a>

Store previous versions of workflows.

### workflow\_statistics <a href="#workflowstatistics" id="workflowstatistics"></a>

Counts workflow IDs and their status.

### workflows\_tags <a href="#workflowstags" id="workflowstags"></a>

Maps tags to workflows. [tag\_entity](#tag_entity) contains tag details.

## Entity Relationship Diagram (ERD) <a href="#entity-relationship-diagram-erd" id="entity-relationship-diagram-erd"></a>

!["n8n ERD"](/files/6wwDuOBdnKPvbi443aHQ)

[^1]: In n8n, credentials store authentication information to connect with specific apps and services. After creating credentials with your authentication information (username and password, API key, OAuth secrets, etc.), you can use the associated app node to interact with the service.
