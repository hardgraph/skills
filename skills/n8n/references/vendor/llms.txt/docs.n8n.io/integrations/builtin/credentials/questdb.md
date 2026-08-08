> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/questdb.md).

# QuestDB credentials

You can use these credentials to authenticate the following nodes:

* [QuestDB](/integrations/builtin/app-nodes/n8n-nodes-base.questdb.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create a user account on an instance of [QuestDB](https://questdb.io/).

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* Database connection

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [QuestDB's documentation](https://questdb.io/docs) for more information about the service.

## Using database connection <a href="#using-database-connection" id="using-database-connection"></a>

To configure this credential, you'll need:

* The **Host**: Enter the host name or IP address for the server.
* The **Database**: Enter the database name, for example `qdb`.
* A **User**: Enter the username for the user account as configured in `pg.user` or `pg.readonly.user` property in `server.conf`. Default value is `admin`.
* A **Password**: Enter the password for the user account as configured in `pg.password` or `pg.readonly.password` property in `server.conf`. Default value is `quest`.
* **SSL**: Select whether the connection should use SSL, which sets the `sslmode` parameter. Options include:
  * **Allow**
  * **Disable**
  * **Require**
* The **Port**: Enter the port number to use for the connection. Default is `8812`.

Refer to [List of supported connection properties](https://questdb.io/docs/reference/api/postgres/#list-of-supported-connection-properties) for more information.
