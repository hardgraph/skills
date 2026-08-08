---
modificationDate: July 17, 2026
title: Programmatic access
description: Learn about types of access tokens and how to use them.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/accounts/programmatic-access/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/accounts/programmatic-access/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > Reference > Expo accounts
Pages in this section:
- [Account types](https://docs.expo.dev/accounts/account-types.md)
- [Two-factor authentication](https://docs.expo.dev/accounts/two-factor.md)
- [Programmatic access](https://docs.expo.dev/accounts/programmatic-access.md) (this page)
- [Single Sign-On (SSO)](https://docs.expo.dev/accounts/sso.md)
- [Audit logs](https://docs.expo.dev/accounts/audit-logs.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Programmatic access

Learn about types of access tokens and how to use them.

When setting up CI or writing a script to help manage your projects, we recommend avoiding using your username and password to authenticate. With these credentials, anyone will be able to log in and use your account.

Instead of providing credentials, you can generate tokens that will allow you to manage each integration point separately. Anyone who has access to these tokens will be able to perform actions against your account. Treat them with the same care as a user password. In case something is leaked, you can revoke these tokens to block access.

## Personal access tokens

You can create Personal access tokens from the [Access tokens](https://expo.dev/settings/access-tokens) on your dashboard. Anyone with this token can perform actions on your behalf. That applies to all content on your Personal Account, as well as any Personal Accounts or Organizations that you have been granted access to.

## Robot users and access tokens

Accounts can create Robot users to take actions on resources owned by the Account. Bot Users can be assigned [a role](/accounts/account-types.md#manage-access) to limit the actions they are authorized to perform. Bot users cannot sign in to any Expo products, cannot own any projects themselves, and can only authenticate via an access token.

## Access tokens usage

You can use any tokens you have created to perform actions with the EAS CLI. To use tokens, you need to define an environment variable, like `EXPO_TOKEN="token"`, before running commands.

Once you set the `EXPO_TOKEN` environment variable, you can run any EAS CLI command authenticated with the token without running the `eas login` command. The `eas login` command is only used for username and password authentication. The `EXPO_TOKEN` auth method takes precedence over the username and password if both are configured.

For example, once you obtain a token, you can run the following EAS CLI command to trigger a build:

```sh
EXPO_TOKEN=my_token eas build
```

> Commands that run with an access token require the project to already be linked to an EAS project. If `extra.eas.projectId` is not set in your [app config](/workflow/configuration.md), commands such as `eas build` fail with an `EAS project not configured` error. To configure it, run `EXPO_TOKEN=my_token eas init --force --non-interactive` first.

If you are using GitHub Actions, [you can configure the `token` property](https://github.com/expo/expo-github-action#configuration-options) to include this environment variable in all the job steps.

Common situations where access tokens are useful:

-   Publish or build from CI without providing your Expo username and password
-   Renew a token to keep it as secure as possible; no need to reset your password and sign out of all sessions
-   Give someone (or a script) one-time access to your project with limited permissions

## Revoke access tokens

In case a token is accidentally leaked, you can revoke it without changing your username and password. When you revoke the access token, you block all access to your account using this token. To do this, go to the [Access Token page](https://expo.dev/settings/access-tokens) on your dashboard and delete the token you want to revoke.
