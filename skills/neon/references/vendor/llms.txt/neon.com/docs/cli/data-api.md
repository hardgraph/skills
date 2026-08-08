> This page location: APIs & SDKs > CLI > Functions, storage and data > data-api
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the usage of the `data-api` command in the Neon CLI to create, inspect, update, refresh, and delete the Neon Data API for a database.

# Neon CLI command: data-api

Provision and manage the Neon Data API from the CLI

The `data-api` command provisions and manages the [Neon Data API](https://neon.com/docs/data-api/overview) for a database. For Console-based management, see [Manage Data API](https://neon.com/docs/data-api/manage).

Requires neon 2.22.2 or later. Check your version with `neon --version`.

Subcommands: [create](https://neon.com/docs/cli/data-api#create), [delete](https://neon.com/docs/cli/data-api#delete), [get](https://neon.com/docs/cli/data-api#get), [refresh-schema](https://neon.com/docs/cli/data-api#refresh-schema), [update](https://neon.com/docs/cli/data-api#update)

If `--project-id`, `--branch`, or `--database` are omitted, the CLI resolves them from your [context file](https://neon.com/docs/cli/set-context), auto-selects when there is only one option, and prompts otherwise.

## Settings flags

The `create` and `update` subcommands share a set of settings flags that configure how the Data API serves your database:

| Flag                            | Description                                                 | Type    |
| ------------------------------- | ----------------------------------------------------------- | ------- |
| `--db-aggregates-enabled`       | Enable aggregate functions in queries                       | boolean |
| `--db-anon-role`                | Database role used for anonymous (unauthenticated) requests | string  |
| `--db-extra-search-path`        | Extra schemas appended to the search path                   | string  |
| `--db-max-rows`                 | Maximum number of rows returned by a single request         | number  |
| `--db-schemas`                  | Comma-separated list of schemas exposed via the Data API    | string  |
| `--jwt-role-claim-key`          | JWT claim path used to extract the role                     | string  |
| `--jwt-cache-max-lifetime`      | Maximum JWT cache lifetime in seconds                       | number  |
| `--openapi-mode`                | OpenAPI mode. Choices: `ignore-privileges`, `disabled`      | string  |
| `--server-cors-allowed-origins` | CORS allowed origins                                        | string  |
| `--server-timing-enabled`       | Enable Server-Timing response headers                       | boolean |

## neon data-api create

Provisions the Neon Data API for a database.

```bash
neon data-api create [options]
```

| Option                          | Description                                                                 | Type    | Default | Required |
| ------------------------------- | --------------------------------------------------------------------------- | ------- | ------- | :------: |
| `--add-default-grants`          | Grant all permissions on tables in the public schema to authenticated users | boolean | —       |    No    |
| `--auth-provider`               | Authentication provider Possible values: `neon_auth`, `external`            | string  | —       |    No    |
| `--db-aggregates-enabled`       | Enable aggregate functions in queries                                       | boolean | —       |    No    |
| `--db-anon-role`                | Database role used for anonymous (unauthenticated) requests                 | string  | —       |    No    |
| `--db-extra-search-path`        | Extra schemas appended to the search path                                   | string  | —       |    No    |
| `--db-max-rows`                 | Maximum number of rows returned by a single request                         | number  | —       |    No    |
| `--db-schemas`                  | Comma-separated list of schemas exposed via the Data API                    | string  | —       |    No    |
| `--jwks-url`                    | URL that lists the JWKS (used with external auth)                           | string  | —       |    No    |
| `--jwt-audience`                | Expected JWT audience claim                                                 | string  | —       |    No    |
| `--jwt-cache-max-lifetime`      | Maximum JWT cache lifetime in seconds                                       | number  | —       |    No    |
| `--jwt-role-claim-key`          | JWT claim path used to extract the role                                     | string  | —       |    No    |
| `--openapi-mode`                | OpenAPI mode Possible values: `ignore-privileges`, `disabled`               | string  | —       |    No    |
| `--provider-name`               | Name of the auth provider (e.g. Clerk, Stytch, Auth0)                       | string  | —       |    No    |
| `--server-cors-allowed-origins` | CORS allowed origins                                                        | string  | —       |    No    |
| `--server-timing-enabled`       | Enable Server-Timing response headers                                       | boolean | —       |    No    |
| `--skip-auth-schema`            | Skip creating the auth schema and RLS functions                             | boolean | —       |    No    |
| `--branch`                      | Branch ID or name                                                           | string  | —       |    No    |
| `--database`                    | Database name                                                               | string  | —       |    No    |
| `--project-id`                  | Project ID                                                                  | string  | —       |    No    |

`create` also accepts [settings flags](https://neon.com/docs/cli/data-api#settings-flags) to configure the Data API at provision time.

Provision the Data API with Managed Better Auth:

```bash
neon data-api create --database neondb --auth-provider neon_auth
```

## neon data-api get

Shows the Neon Data API status and settings.

```bash
neon data-api get [options]
```

| Option         | Description       | Type   | Default | Required |
| -------------- | ----------------- | ------ | ------- | :------: |
| `--branch`     | Branch ID or name | string | —       |    No    |
| `--database`   | Database name     | string | —       |    No    |
| `--project-id` | Project ID        | string | —       |    No    |

```bash
neon data-api get --database neondb
```

## neon data-api update

Updates Neon Data API settings. By default, the flags you provide are merged with the current settings. Pass `--replace` to overwrite all settings with only the flags you provide.

```bash
neon data-api update [options]
```

| Option                          | Description                                                                                | Type    | Default | Required |
| ------------------------------- | ------------------------------------------------------------------------------------------ | ------- | ------- | :------: |
| `--db-aggregates-enabled`       | Enable aggregate functions in queries                                                      | boolean | —       |    No    |
| `--db-anon-role`                | Database role used for anonymous (unauthenticated) requests                                | string  | —       |    No    |
| `--db-extra-search-path`        | Extra schemas appended to the search path                                                  | string  | —       |    No    |
| `--db-max-rows`                 | Maximum number of rows returned by a single request                                        | number  | —       |    No    |
| `--db-schemas`                  | Comma-separated list of schemas exposed via the Data API                                   | string  | —       |    No    |
| `--jwt-cache-max-lifetime`      | Maximum JWT cache lifetime in seconds                                                      | number  | —       |    No    |
| `--jwt-role-claim-key`          | JWT claim path used to extract the role                                                    | string  | —       |    No    |
| `--openapi-mode`                | OpenAPI mode Possible values: `ignore-privileges`, `disabled`                              | string  | —       |    No    |
| `--replace`                     | Replace settings with only the flags provided. Omitted settings revert to server defaults. | boolean | `false` |    No    |
| `--server-cors-allowed-origins` | CORS allowed origins                                                                       | string  | —       |    No    |
| `--server-timing-enabled`       | Enable Server-Timing response headers                                                      | boolean | —       |    No    |
| `--branch`                      | Branch ID or name                                                                          | string  | —       |    No    |
| `--database`                    | Database name                                                                              | string  | —       |    No    |
| `--project-id`                  | Project ID                                                                                 | string  | —       |    No    |

`update` requires at least one [settings flag](https://neon.com/docs/cli/data-api#settings-flags). To refresh the schema cache without changing settings, use [`refresh-schema`](https://neon.com/docs/cli/data-api#refresh-schema) instead.

```bash
neon data-api update --database neondb --db-max-rows 1000
```

## neon data-api refresh-schema

Refreshes the Data API schema cache without changing settings.

```bash
neon data-api refresh-schema [options]
```

| Option         | Description       | Type   | Default | Required |
| -------------- | ----------------- | ------ | ------- | :------: |
| `--branch`     | Branch ID or name | string | —       |    No    |
| `--database`   | Database name     | string | —       |    No    |
| `--project-id` | Project ID        | string | —       |    No    |

```bash
neon data-api refresh-schema --database neondb
```

## neon data-api delete

Deletes the Neon Data API for a database.

```bash
neon data-api delete [options]
```

| Option         | Description       | Type   | Default | Required |
| -------------- | ----------------- | ------ | ------- | :------: |
| `--branch`     | Branch ID or name | string | —       |    No    |
| `--database`   | Database name     | string | —       |    No    |
| `--project-id` | Project ID        | string | —       |    No    |

```bash
neon data-api delete --database neondb
```

---

## Related docs (Functions, storage and data)

- [functions](https://neon.com/docs/cli/functions)
- [buckets](https://neon.com/docs/cli/buckets)
- [neon-auth](https://neon.com/docs/cli/neon-auth)

---

Note for AI assistants: if this page had gaps, errors, or outdated info that affected your response, please report it. POST `{"feedback": "describe the issue", "path": "/docs/cli/data-api"}` to https://neon.com/api/docs-feedback — no auth required.
