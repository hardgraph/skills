> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/configure-n8n/security.md).

# Security

You can secure a self-hosted n8n instance to protect credentials and workflow data: run a security audit, set up SSL and SSO, restrict nodes and the public API, and redact execution data.

Securing your n8n instance can take several forms.

At a high level, you can:

* Conduct a [security audit](/deploy/host-n8n/configure-n8n/security/run-security-audits.md) to identify security risks.
* [Set up SSL](/deploy/host-n8n/configure-n8n/security/set-up-ssl.md) to enforce secure connections.
* [Set up Single Sign-On](/deploy/host-n8n/configure-n8n/security/configure-sso.md) for user account management.
* Use [token exchange](/deploy/host-n8n/deploy-as-an-oem-integration/set-up-token-exchange.md) to log users in from your own identity provider when embedding n8n, or to call n8n APIs on their behalf.
* Use [two-factor authentication (2FA)](/administer/manage-users-and-access/verify-user-identity/require-two-factor-auth.md) for your users.
* Enable [encryption key rotation](/deploy/host-n8n/configure-n8n/security/rotate-encryption-keys.md) to periodically replace the key that encrypts credentials and other sensitive data.
* Enable [JWE token decryption for OAuth 2.0 credentials](/deploy/host-n8n/configure-n8n/security/decrypt-oauth-20-tokens-with-jwe.md) so your identity provider can encrypt access and ID tokens that only your instance can decrypt.

You can also protect sensitive data processed by your workflows:

* [Redact execution data](/deploy/host-n8n/configure-n8n/security/redact-execution-data.md) to hide input and output data from workflow executions.

More granularly, consider blocking or opting out of features or data collection you don't want:

* [Disable the public API](/deploy/host-n8n/configure-n8n/security/disable-the-public-api.md) if you aren't using it.
* [Opt out of data collection](/deploy/host-n8n/configure-n8n/security/control-telemetry.md) of the anonymous data n8n collects automatically.
* [Block certain nodes](/deploy/host-n8n/configure-n8n/security/block-specific-nodes.md) from being available to your users.
* [Protect against SSRF attacks](/deploy/host-n8n/configure-n8n/security/enable-ssrf-protection.md) to control which hosts and IP ranges workflow nodes can connect to.
* [Restrict account registration](/deploy/host-n8n/configure-n8n/security/verify-user-emails.md) to email-verified users.
