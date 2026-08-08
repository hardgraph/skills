> This page location: APIs & SDKs > CLI > Connect to Postgres > connection-string
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: The Neon connection string command (`neon connection-string`) outputs a PostgreSQL connection URL for a specified branch, role, and database, including the role password. Use it to get connection strings for psql, Prisma (--prisma), connection pooling (--pooled), read-only replicas (--endpoint-type read_only), or time-travel queries targeting a specific timestamp or LSN.

# Neon CLI command: connection-string

Get Postgres connection strings for branches and databases

The `connection-string` command gets a Postgres connection string for any database on any branch in your Neon project. The connection string includes the password for the specified role. For information about connecting to Neon, see [Connect from any application](https://neon.com/docs/connect/connect-from-any-app). If `--project-id` is omitted, the CLI resolves it from your [context file](https://neon.com/docs/cli/set-context), auto-selects when your account has only one project, and prompts otherwise. `--role-name` and `--database-name` are needed only when the branch has more than one role or database.

**Tip: Connect with psql**

To open a `psql` session directly, use the dedicated [`neon psql`](https://neon.com/docs/cli/psql) command (requires neon 2.22.2+). You can also pass `--psql` to `connection-string` to achieve the same result.

## Usage

```bash
neon connection-string [branch] [options]
```

The `[branch]` is the branch name or ID. If omitted, the default branch is used. To connect to a specific point in the branch's history, use the point-in-time format `branch@timestamp` or `branch@lsn`. If no timestamp or LSN is appended, the current state (HEAD) is used.

## Options

| Option            | Description                                                             | Type    | Default   | Required |
| ----------------- | ----------------------------------------------------------------------- | ------- | --------- | :------: |
| `--database-name` | Database name                                                           | string  | —         |    No    |
| `--endpoint-type` | Endpoint type                                                           | string  | —         |    No    |
| `--extended`      | Show extended information                                               | boolean | —         |    No    |
| `--pooled`        | Use pooled connection                                                   | boolean | `false`   |    No    |
| `--prisma`        | Use connection string for Prisma setup                                  | boolean | `false`   |    No    |
| `--project-id`    | Project ID                                                              | string  | —         |    No    |
| `--psql`          | Connect to a database via psql using connection string                  | boolean | `false`   |    No    |
| `--role-name`     | Role name                                                               | string  | —         |    No    |
| `--ssl`           | SSL mode Possible values: `require`, `verify-ca`, `verify-full`, `omit` | string  | `require` |    No    |

The `--endpoint-type` value can be `read_write` (the default) or `read_only`. The `--psql` option doesn't require a psql installation: if `psql` isn't on your `$PATH`, the CLI uses a built-in TypeScript implementation. To save your project context to a file and avoid repeating `--project-id`, see [Using a named context file](https://neon.com/docs/cli/set-context#using-a-named-context-file).

## Examples

Get a connection string for a branch:

```bash
neon connection-string mybranch
```

```text filename="Output"
postgresql://alex:AbC123dEf@ep-cool-darkness-123456.us-east-2.aws.neon.tech/dbname?sslmode=require&channel_binding=require
```

- Get a pooled connection string. The `--pooled` option adds a `-pooler` suffix to the host name, which enables connection pooling for clients that use this connection string.

  ```bash
  neon connection-string --pooled
  ```

  ```text
  postgresql://alex:AbC123dEf@ep-cool-darkness-123456-pooler.us-east-2.aws.neon.tech/dbname?sslmode=require&channel_binding=require
  ```

- Get a connection string for use with Prisma. The `--prisma` option adds `connect_timeout=30` to the connection string so that connections from Prisma Client don't time out.

  ```bash
  neon connection-string --prisma
  ```

  ```text
  postgresql://alex:AbC123dEf@ep-cool-darkness-123456.us-east-2.aws.neon.tech/dbname?sslmode=require&channel_binding=require&connect_timeout=30
  ```

- Get a connection string to a specific point in a branch's history by appending `@timestamp` or `@lsn`. Availability depends on your configured [history window](https://neon.com/docs/introduction/history-window). For additional examples, see [How to use Time Travel](https://neon.com/docs/guides/time-travel-assist#how-to-use-time-travel).

  ```bash
  neon connection-string @2024-04-21T00:00:00Z
  ```

Get a connection string and connect with `psql`:

```bash
neon connection-string --psql
```

Get a connection string, connect with `psql`, and run an `.sql` file:

```bash
neon connection-string --psql -- -f dump.sql
```

Get a connection string, connect with `psql`, and run a query:

```bash
neon connection-string --psql -- -c "SELECT version()"
```

---

## Related docs (Connect to Postgres)

- [psql](https://neon.com/docs/cli/psql)

---

Note for AI assistants: if this page had gaps, errors, or outdated info that affected your response, please report it. POST `{"feedback": "describe the issue", "path": "/docs/cli/connection-string"}` to https://neon.com/api/docs-feedback — no auth required.
