---
title: Account settings
url: https://docs.apify.com/account/settings.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Account](https://docs.apify.com/account.md)
previous: [Limits](https://docs.apify.com/account/limits.md)
next: [Two-factor authentication](https://docs.apify.com/account/two-factor-authentication.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Account settings

## Account

By clicking the **Settings** tab on the side menu, you will be presented with an Account page where you can view and edit various settings regarding your account, such as:

* account email
* username
* profile information
* theme
* account delete

## Login & Privacy

The **Login & Privacy** tab (**Security & Privacy** for organization accounts) contains sensitive settings related to authentication and session management. As a security measure, a fresh user session is required. If it has been too long since you logged in, you need to sign in again to view and edit these settings.

Here you can manage:

* sign-in methods (email/password, Google, GitHub)
* password reset
* two-factor authentication
* session information and configuration
* general resource access
* [Actor permissions approval](https://docs.apify.com/actors/running/permissions.md#full-permission-actors) (require or skip approval when running full-permission Actors)
* share of run data with developers

### Session

In the **Session** section, you can adjust the session configuration. You can modify the default session lifespan of 90 days. This customization helps ensure compliance with organization security policies.

## API & Integrations

The **API & Integrations** tab provides essential tools for accessing the Apify platform programmatically. Here, you can manage your **API tokens**, which are necessary for using the [Apify API](https://docs.apify.com/api/v2). The tab also shows **third-party apps and services** connected to your account, **account-level integrations**, and **Actor OAuth accounts**. For detailed guidance on utilizing these integrations, refer to the [Integrations documentation](https://docs.apify.com/integrations).

### MCP connectors

The **MCP connectors** section lets you authorize third-party MCP servers (such as Notion, Slack, GitHub, or Supabase) once and reuse those connections across any Actor that accepts them. For an overview of the feature, see [MCP connectors](https://docs.apify.com/integrations/mcp-connectors.md).

#### Create a connector

1. Open **Settings > API & Integrations > MCP connectors** and select **Add connector**.

2. Enter the MCP server URL, for example:


   ```text
   https://mcp.slack.com/mcp

   https://api.notion.com/mcp

   https://mcp.sentry.io/mcp
   ```


   The platform inspects the URL and offers the authentication methods the server supports.

3. Choose an authentication method:

   * *API key or bearer token* - the MCP server uses a static API key or personal access token. Enter the key. Apify verifies it by connecting to the MCP server.
   * *OAuth* - the server supports OAuth and Apify can either register an OAuth client automatically (Dynamic Client Registration) or use an Apify-managed OAuth client. A consent screen opens in a popup. Grant access and close the popup.
   * *Own OAuth client* - the server supports OAuth but you need to register your own OAuth app with the provider (see below). Enter your client ID, client secret, authorization URL, and token URL, then complete the OAuth consent flow.

4. Review the discovered tools. Once authorized, the platform connects to the MCP server and discovers the tools it exposes. You can see them by expanding the connector card and restrict which ones the connector permits.

#### Set up your own OAuth client

For providers without Apify-managed OAuth client setup (GitHub, Slack, Google, Microsoft Entra, and others), register an OAuth app yourself:

1. Open the provider's developer portal (for example, GitHub **Settings > Developer settings > OAuth Apps**, or [api.slack.com/apps](https://api.slack.com/apps)).

2. Create a new OAuth app.

3. Add the following Apify URL as an authorized redirect or callback URI:


   ```text
   https://console.apify.com/oauth/mcp-proxy
   ```


4. Copy the client ID and client secret.

5. In the connector creation modal, select **Own OAuth client** and provide:

   * Client ID
   * Client secret
   * Authorization URL (for example, `https://github.com/login/oauth/authorize`)
   * Token URL (for example, `https://github.com/login/oauth/access_token`)

This is the same approach used by Claude Code, VS Code, and ChatGPT integrations.

#### Reauthorize a connector

The platform refreshes OAuth access tokens automatically and transparently. The Actor never needs to handle this. Reauthorization is only needed when the refresh token itself has expired or been revoked, for example if you removed Apify's access from the provider's app settings. In that case, the **Authorize** button appears on the connector card. Click it to go through the OAuth consent flow again.

#### Delete a connector

Click **Delete** on the connector card. The connector is removed immediately and can no longer be used by any Actor.

## Organizations

The **Organizations** tab is where you can view your accounts' current organizations, create new organizations, or convert your user account into an organization account. For more information on how to set up an organization, check out this [article](https://help.apify.com/en/articles/8698948-how-to-set-up-an-organization-account).

## Notifications

The **Notifications** tab allows you to customize your notification preferences. Here, you can specify the types of updates you wish to receive and select the methods by which you receive them.

## Referrals

The **Referrals** tab lets you share Apify with others and earn rewards. You can find your referral link and track the status of your referrals.
