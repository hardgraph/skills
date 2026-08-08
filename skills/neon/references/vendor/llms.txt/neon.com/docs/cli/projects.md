> This page location: APIs & SDKs > CLI > Projects and branches > projects
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: The Neon CLI `neon projects` command provides subcommands to list, create, update, delete, recover, and retrieve Neon projects from the terminal, including support for region selection (AWS and Azure), blocking public or VPC connections, and filtering shared projects. Use this page when you need CLI automation for project lifecycle tasks or to recover a deleted project within its 7-day recovery window. Projects created via the CLI default to Postgres 18; use `--pg-version` to select a different major version.

# Neon CLI command: projects

List, create, update, delete, recover, and get Neon projects

The `projects` command lists, creates, updates, deletes, recovers, and retrieves Neon projects from the terminal. For information about projects in Neon, see [Projects](https://neon.com/docs/manage/projects). Subcommands that show no options table accept only the [global options](https://neon.com/docs/cli#global-options). If `--project-id` is omitted, the CLI resolves it from your [context file](https://neon.com/docs/cli/set-context), auto-selects when your account has only one project, and prompts otherwise.

Subcommands: [create](https://neon.com/docs/cli/projects#create), [delete](https://neon.com/docs/cli/projects#delete), [get](https://neon.com/docs/cli/projects#get), [list](https://neon.com/docs/cli/projects#list), [recover](https://neon.com/docs/cli/projects#recover), [update](https://neon.com/docs/cli/projects#update)

## neon projects list

Lists projects that belong to your Neon account, as well as any projects that were shared with you.

```bash
neon projects list [options]
```

| Option               | Description                                                   | Type    | Default | Required |
| -------------------- | ------------------------------------------------------------- | ------- | ------- | :------: |
| `--org-id`           | List projects of a given organization                         | string  | —       |    No    |
| `--recoverable-only` | List only deleted projects within their deletion grace period | boolean | —       |    No    |

- List projects in your [default organization](https://neon.com/docs/reference/glossary#default-organization). If no organization context is set, the CLI prompts you to select one.

  ```bash
  neon projects list
  ```

  ```text
  Projects
  ┌────────────────────────┬────────────────────┬───────────────┬──────────────────────┐
  │ Id                     │ Name               │ Region Id     │ Created At           │
  ├────────────────────────┼────────────────────┼───────────────┼──────────────────────┤
  │ crimson-voice-12345678 │ frontend           │ aws-us-east-2 │ 2024-04-15T11:17:30Z │
  ├────────────────────────┼────────────────────┼───────────────┼──────────────────────┤
  │ calm-thunder-12121212  │ backend            │ aws-us-east-2 │ 2024-04-10T15:21:01Z │
  ├────────────────────────┼────────────────────┼───────────────┼──────────────────────┤
  │ nameless-hall-87654321 │ billing            │ aws-us-east-2 │ 2024-04-10T14:35:17Z │
  └────────────────────────┴────────────────────┴───────────────┴──────────────────────┘
  Shared with you
  ┌───────────────────┬────────────────────┬──────────────────┬──────────────────────┐
  │ Id                │ Name               │ Region Id        │ Created At           │
  ├───────────────────┼────────────────────┼──────────────────┼──────────────────────┤
  │ noisy-fire-212121 │ API                │ aws-eu-central-1 │ 2023-04-22T18:41:13Z │
  └───────────────────┴────────────────────┴──────────────────┴──────────────────────┘
  ```

List all projects belonging to a specific organization:

```bash
neon projects list --org-id org-xxxx-xxxx
```

List projects that can be recovered (deleted within the last 7 days):

```bash
neon projects list --recoverable-only
```

```text filename="Output"
Projects
┌─────────────────────┬───────────┬───────────────┬──────────────────────┬──────────────────────┬──────────────────────┐
│ Id                  │ Name      │ Region Id     │ Created At           │ Deleted At           │ Recoverable Until    │
├─────────────────────┼───────────┼───────────────┼──────────────────────┼──────────────────────┼──────────────────────┤
│ crimson-voice-12345 │ myproject │ aws-us-east-2 │ 2024-04-15T11:17:30Z │ 2024-04-16T14:22:15Z │ 2024-04-23T14:22:15Z │
└─────────────────────┴───────────┴───────────────┴──────────────────────┴──────────────────────┴──────────────────────┘
```

## neon projects create

Creates a Neon project.

```bash
neon projects create [options]
```

| Option                       | Description                                                                                                                                                                                | Type    | Default            | Required |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------- | ------------------ | :------: |
| `--block-public-connections` | When set, connections from the public internet are disallowed. This supersedes the AllowedIPs list. This parameter is under active development and its semantics may change in the future. | boolean | —                  |    No    |
| `--block-vpc-connections`    | When set, connections using VPC endpoints are disallowed. This parameter is under active development and its semantics may change in the future.                                           | boolean | —                  |    No    |
| `--cu`                       | The number of Compute Units. Could be a fixed size (e.g. "2") or a range delimited by a dash (e.g. "0.5-3").                                                                               | string  | —                  |    No    |
| `--database`                 | The database name. If not specified, the default database name, `neondb`, will be used.                                                                                                    | string  | neondb             |    No    |
| `--hipaa`                    | Enable HIPAA compliance for the project.                                                                                                                                                   | boolean | —                  |    No    |
| `--name`                     | The project name. If not specified, the name will be identical to the generated project ID                                                                                                 | string  | project ID         |    No    |
| `--org-id`                   | The project's organization ID                                                                                                                                                              | string  | —                  |    No    |
| `--pg-version`               | Major PostgreSQL version (14–19). Version 19 is available only in regions where it has been enabled.                                                                                       | number  | —                  |    No    |
| `--psql`                     | Connect to a new project via psql                                                                                                                                                          | boolean | `false`            |    No    |
| `--region-id`                | The region ID. Possible values: aws-us-west-2, aws-ap-southeast-1, aws-ap-southeast-2, aws-eu-central-1, aws-us-east-2, aws-us-east-1, azure-eastus2                                       | string  | —                  |    No    |
| `--role`                     | The role name. If not specified, the default role name, `{database_name}_owner`, will be used.                                                                                             | string  | \{database}\_owner |    No    |
| `--set-context`              | Set the current context to the new project                                                                                                                                                 | boolean | `false`            |    No    |

The `--region-id` value defaults to `aws-us-east-2` if not specified. `--block-public-connections` and `--block-vpc-connections` are part of [Private Networking](https://neon.com/docs/guides/neon-private-networking); `--hipaa` enables [HIPAA compliance](https://neon.com/docs/security/hipaa) for the project.

**Note:**

Neon projects created using the CLI use the default Postgres version, which is Postgres 18. To create a project with a different Postgres version, pass `--pg-version`:

```bash
neon projects create --name mynewproject --pg-version 17
```

You can also enable logical replication for an existing project with `neon projects update <project-id> --enable-logical-replication` (add `--yes` to skip the confirmation prompt), and create protected branches with `neon branches create --protected`.

Create a project with a user-defined name in a specific region:

```bash
neon projects create --name mynewproject --region-id aws-us-west-2
```

```text filename="Output"
┌───────────────────┬──────────────┬───────────────┬──────────────────────┐
│ Id                │ Name         │ Region Id     │ Created At           │
├───────────────────┼──────────────┼───────────────┼──────────────────────┤
│ muddy-wood-859533 │ mynewproject │ aws-us-west-2 │ 2023-07-09T17:04:29Z │
└───────────────────┴──────────────┴───────────────┴──────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────────────┐
│ Connection Uri                                                                       │
├──────────────────────────────────────────────────────────────────────────────────────┤
│ postgresql://[user]:[password]@[neon_hostname]/[dbname]                              │
└──────────────────────────────────────────────────────────────────────────────────────┘
```

**Tip:** The Neon CLI provides a `neon connection-string` command you can use to extract a connection URI programmatically. See [the connection-string command](https://neon.com/docs/cli/connection-string).

Create a project with a specific Postgres major version:

```bash
neon projects create --name mynewproject --pg-version 17
```

- Create a project with `--output json`, which returns the full project response data and is the recommended format for scripts and agents. The output below was captured on an earlier CLI version; new projects report `"pg_version": 18`.

  ```bash
  neon projects create --output json
  ```

  <details>

  <summary>Show output</summary>

  ```json
  {
    "project": {
      "data_storage_bytes_hour": 0,
      "data_transfer_bytes": 0,
      "written_data_bytes": 0,
      "compute_time_seconds": 0,
      "active_time_seconds": 0,
      "cpu_used_sec": 0,
      "id": "long-wind-77910944",
      "platform_id": "aws",
      "region_id": "aws-us-east-2",
      "name": "long-wind-77910944",
      "provisioner": "k8s-pod",
      "default_endpoint_settings": {
        "autoscaling_limit_min_cu": 1,
        "autoscaling_limit_max_cu": 1,
        "suspend_timeout_seconds": 0
      },
      "pg_version": 17,
      "proxy_host": "us-east-2.aws.neon.tech",
      "branch_logical_size_limit": 204800,
      "branch_logical_size_limit_bytes": 214748364800,
      "store_passwords": true,
      "creation_source": "neon",
      "history_retention_seconds": 604800,
      "created_at": "2023-08-04T16:16:45Z",
      "updated_at": "2023-08-04T16:16:45Z",
      "consumption_period_start": "0001-01-01T00:00:00Z",
      "consumption_period_end": "0001-01-01T00:00:00Z",
      "owner_id": "e56ad68e-7f2f-4d74-928c-9ea25d7e9864"
    },
    "connection_uris": [
      {
        "connection_uri": "postgresql://alex:AbC123dEf@ep-cool-darkness-123456.us-east-2.aws.neon.tech/dbname?sslmode=require&channel_binding=require",
        "connection_parameters": {
          "database": "dbname",
          "password": "AbC123dEf",
          "role": "alex",
          "host": "ep-cool-darkness-123456.us-east-2.aws.neon.tech",
          "pooler_host": "ep-cool-darkness-123456-pooler.us-east-2.aws.neon.tech"
        }
      }
    ]
  }
  ```

  </details>

Create a project and connect to it with `psql` immediately. Arguments after `--` are passed through to psql, so you can run an `.sql` file or a query on creation:

```bash
neon projects create --psql
neon projects create --psql -- -f dump.sql
neon projects create --psql -- -c "SELECT version()"
```

Create a project and set the Neon CLI project context to it:

```bash
neon projects create --set-context
```

## neon projects update

Updates a Neon project. The `<id>` is the project ID, which you can obtain by listing your projects or from the **Settings** page in the Neon Console.

```bash
neon projects update <id> [options]
```

| Option                         | Description                                                                                                                                                                                                                                                | Type    | Default | Required |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ------- | :------: |
| `--block-public-connections`   | When set, connections from the public internet are disallowed. This supersedes the AllowedIPs list. This parameter is under active development and its semantics may change in the future. Use --block-public-connections=false to set the value to false. | boolean | —       |    No    |
| `--block-vpc-connections`      | When set, connections using VPC endpoints are disallowed. This parameter is under active development and its semantics may change in the future. Use --block-vpc-connections=false to set the value to false.                                              | boolean | —       |    No    |
| `--cu`                         | The number of Compute Units. Could be a fixed size (e.g. "2") or a range delimited by a dash (e.g. "0.5-3").                                                                                                                                               | string  | —       |    No    |
| `--enable-logical-replication` | Enable logical replication for all project endpoints. This suspends active endpoints and cannot be disabled.                                                                                                                                               | boolean | —       |    No    |
| `--hipaa`                      | Enable HIPAA compliance for the project.                                                                                                                                                                                                                   | boolean | —       |    No    |
| `--name`                       | The project name                                                                                                                                                                                                                                           | string  | —       |    No    |
| `--yes`                        | Skip the confirmation prompt when enabling logical replication.                                                                                                                                                                                            | boolean | `false` |    No    |

`--block-public-connections`, `--block-vpc-connections`, and `--hipaa` behave as described under [projects create](https://neon.com/docs/cli/projects#create).

Update the project name:

```bash
neon projects update muddy-wood-859533 --name dev_project_1
```

```text filename="Output"
┌───────────────────┬───────────────┬───────────────┬──────────────────────┐
│ Id                │ Name          │ Region Id     │ Created At           │
├───────────────────┼───────────────┼───────────────┼──────────────────────┤
│ muddy-wood-859533 │ dev_project_1 │ aws-us-west-2 │ 2023-07-09T17:04:29Z │
└───────────────────┴───────────────┴───────────────┴──────────────────────┘
```

Block connections from the public internet (see [restrict public internet access](https://neon.com/docs/guides/neon-private-networking#restrict-public-internet-access)):

```bash
neon projects update orange-credit-12345678 --block-public-connections=true
```

Enable [logical replication](https://neon.com/docs/guides/logical-replication-neon) for the project. This suspends active endpoints and cannot be disabled, so `--yes` skips the confirmation prompt:

```bash
neon projects update orange-credit-12345678 --enable-logical-replication --yes
```

## neon projects delete

Deletes a Neon project. The `<id>` is the project ID.

```bash
neon projects delete <id> [options]
```

```bash
neon projects delete muddy-wood-859533
```

```text filename="Output"
┌───────────────────┬───────────────┬───────────────┬──────────────────────┐
│ Id                │ Name          │ Region Id     │ Created At           │
├───────────────────┼───────────────┼───────────────┼──────────────────────┤
│ muddy-wood-859533 │ dev_project_1 │ aws-us-west-2 │ 2023-07-09T17:04:29Z │
└───────────────────┴───────────────┴───────────────┴──────────────────────┘
```

Verify the deletion with `neon projects list`.

## neon projects recover

Recovers a deleted project within the deletion recovery period. The `<id>` is the project ID, which you can obtain by listing recoverable projects with `neon projects list --recoverable-only`.

```bash
neon projects recover <id> [options]
```

```bash
neon projects recover crimson-voice-12345678
```

```text filename="Output"
┌────────────────────────┬───────────┬───────────────┬──────────────────────┐
│ Id                     │ Name      │ Region Id     │ Created At           │
├────────────────────────┼───────────┼───────────────┼──────────────────────┤
│ crimson-voice-12345678 │ myproject │ aws-us-east-2 │ 2024-04-15T11:17:30Z │
└────────────────────────┴───────────┴───────────────┴──────────────────────┘
```

For details on what's recovered and what requires reconfiguration after recovery, see [Recover a deleted project](https://neon.com/docs/manage/projects#recover-a-deleted-project).

## neon projects get

Retrieves details about a Neon project. The `<id>` is the project ID.

```bash
neon projects get <id> [options]
```

```bash
neon projects get muddy-wood-859533
```

```text filename="Output"
┌───────────────────┬───────────────┬───────────────┬──────────────────────┐
│ Id                │ Name          │ Region Id     │ Created At           │
├───────────────────┼───────────────┼───────────────┼──────────────────────┤
│ muddy-wood-859533 │ dev_project_1 │ aws-us-west-2 │ 2023-07-09T17:04:29Z │
└───────────────────┴───────────────┴───────────────┴──────────────────────┘
```

---

## Related docs (Projects and branches)

- [snapshots](https://neon.com/docs/cli/snapshots)
- [diff](https://neon.com/docs/cli/diff)
- [branches](https://neon.com/docs/cli/branches)
- [databases](https://neon.com/docs/cli/databases)
- [roles](https://neon.com/docs/cli/roles)
- [operations](https://neon.com/docs/cli/operations)

---

Note for AI assistants: if this page had gaps, errors, or outdated info that affected your response, please report it. POST `{"feedback": "describe the issue", "path": "/docs/cli/projects"}` to https://neon.com/api/docs-feedback — no auth required.
