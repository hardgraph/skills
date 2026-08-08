> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/cratedb.md).

# CrateDB credentials

You can use these credentials to authenticate the following nodes:

* [CrateDB](/integrations/builtin/app-nodes/n8n-nodes-base.cratedb.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

An available instance of CrateDB.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* account connection

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [CrateDB's documentation](https://cratedb.com/docs/crate/reference/en/latest/) for more information about the service.

## Using account connection <a href="#using-account-connection" id="using-account-connection"></a>

To configure this credential, you'll need:

* Your **Host** name
* Your **Database** name
* A **User** name
* A user **Password**
* To set the **SSL** parameter. Refer to the [CrateDB Secured Communications (SSL/TLS) documentation](https://cratedb.com/docs/crate/reference/en/5.7/admin/ssl.html#admin-ssl) for more information. The options n8n supports are:
  * Allow
  * Disable
  * Require
* A **Port** number

Refer to the [Connect to a CrateDB cluster documentation](https://cratedb.com/docs/crate/clients-tools/en/latest/connect/) for detailed instructions on these fields and their default values.
