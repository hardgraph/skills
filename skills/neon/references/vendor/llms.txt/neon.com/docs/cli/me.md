> This page location: APIs & SDKs > CLI > Setup and context > me
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: The `neon me` CLI command prints the authenticated user's login, email, name, and projects limit in the default table output. Use it to confirm which account is active after authentication. JSON output (`-o json`) exposes additional account and quota fields, including plan type, branches limit, max autoscaling limit, billing_account, and auth_accounts.

# Neon CLI command: me

View current user info, login details, and project limits

The `me` command shows information about the authenticated Neon CLI user: login, email, name, and projects limit.

## Usage

```bash
neon me [options]
```

## Options

Takes only the [global options](https://neon.com/docs/cli#global-options).

## Examples

```bash
neon me
```

```text filename="Output"
┌────────────────┬──────────────────────────┬─────────────┬────────────────┐
│ Login          │ Email                    │ Name        │ Projects Limit │
├────────────────┼──────────────────────────┼─────────────┼────────────────┤
│ sally          │ sally@example.com        │ Sally Smith │       1        │
└────────────────┴──────────────────────────┴─────────────┴────────────────┘
```

Show details with `--output json`, which includes data omitted from the `table` output:

```bash
neon me -o json
```

<details>

<summary>Show output</summary>

```json
{
  "active_seconds_limit": 360000,
  "billing_account": {
    "payment_source": {
      "type": ""
    },
    "subscription_type": "free",
    "quota_reset_at_last": "2023-07-01T00:00:00Z",
    "email": "sally@example.com",
    "address_city": "",
    "address_country": "",
    "address_line1": "",
    "address_line2": "",
    "address_postal_code": "",
    "address_state": ""
  },
  "auth_accounts": [
    {
      "email": "sally@example.com",
      "image": "https://lh3.googleusercontent.com/a/AItbvml5rjEQkmt-h_abcdef-MwVtfpek7Aa_xk3cIS_=s96-c",
      "login": "sally",
      "name": "Sally Smith",
      "provider": "google"
    },
    {
      "email": "sally@example.com",
      "image": "",
      "login": "sally",
      "name": "sally@example.com",
      "provider": "hasura"
    }
  ],
  "email": "sally@example.com",
  "id": "8a9f604e-d04e-1234-baf7-e78909a5d123",
  "image": "https://lh3.googleusercontent.com/a/AItbvml5rjEQkmt-h_abcdef-MwVtfpek7Aa_xk3cIS_=s96-c",
  "login": "sally",
  "name": "Sally Smith",
  "projects_limit": 10,
  "branches_limit": 10,
  "max_autoscaling_limit": 0.25,
  "plan": "free"
}
```

</details>

In JSON output, `max_autoscaling_limit` is a numeric value in compute units. A value of `0` means no autoscaling limit is configured.

---

## Related docs (Setup and context)

- [auth](https://neon.com/docs/cli/auth)
- [init](https://neon.com/docs/cli/init)
- [bootstrap](https://neon.com/docs/cli/bootstrap)
- [link](https://neon.com/docs/cli/link)
- [checkout](https://neon.com/docs/cli/checkout)
- [env](https://neon.com/docs/cli/env)
- [set-context](https://neon.com/docs/cli/set-context)
- [profile](https://neon.com/docs/cli/profile)
- [api-keys](https://neon.com/docs/cli/api-keys)
- [completion](https://neon.com/docs/cli/completion)

---

Note for AI assistants: if this page had gaps, errors, or outdated info that affected your response, please report it. POST `{"feedback": "describe the issue", "path": "/docs/cli/me"}` to https://neon.com/api/docs-feedback — no auth required.
