> This page location: APIs & SDKs > CLI > Functions, storage and data > functions
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: The Neon CLI `neon functions` command manages Neon Functions on a branch: `neon functions deploy <slug>` bundles and deploys a function from a local directory or entry file (with --src, --runtime, --env, and --wait), and the list, get, and delete subcommands manage deployed functions. The slug is the permanent function identifier: 1 to 20 lowercase letters and digits.

# Neon CLI command: functions

Deploy, list, inspect, and delete Neon Functions

**Note: Beta**

The **Neon Functions** is in Beta. Share your feedback on [Discord](https://discord.gg/92vNTzKDGp) or via the [Neon Console](https://console.neon.tech/app/projects?modal=feedback).

The `functions` command manages [Neon Functions](https://neon.com/docs/compute/functions/overview) on a branch. This is the command reference; for the full deployment workflow, see [Deploy functions](https://neon.com/docs/compute/functions/deploy). To run functions locally, see [`neon dev`](https://neon.com/docs/cli/dev).

Subcommands: [delete](https://neon.com/docs/cli/functions#delete), [deploy](https://neon.com/docs/cli/functions#deploy), [get](https://neon.com/docs/cli/functions#get), [list](https://neon.com/docs/cli/functions#list)

## neon functions deploy

Deploys a function from a local directory or entry file. The `<slug>` is the permanent function identifier: 1 to 20 lowercase letters and digits (`^[a-z0-9]{1,20}$`).

```bash
neon functions deploy <slug> [options]
```

| Option         | Description                                                                                           | Type    | Default | Required |
| -------------- | ----------------------------------------------------------------------------------------------------- | ------- | ------- | :------: |
| `--env`        | Environment variable as KEY=VALUE (repeatable)                                                        | string  | —       |    No    |
| `--runtime`    | Function runtime Possible values: `nodejs24`                                                          | string  | —       |    No    |
| `--src`        | Function source: a directory containing index.ts, index.mjs, or index.js, or a path to the entry file | string  | —       |    No    |
| `--wait`       | Wait for the deployment to finish building                                                            | boolean | `true`  |    No    |
| `--branch`     | Branch ID or name                                                                                     | string  | —       |    No    |
| `--project-id` | Project ID                                                                                            | string  | —       |    No    |

By default, `deploy` waits until the deployment finishes building (`--wait=true`), which is the predictable path for scripts and CI. Use `--no-wait` to return immediately after triggering the deployment.

Deploy a function from an entry file:

```bash
neon functions deploy hello --src functions/hello.ts
```

```text filename="Output"
INFO: Function deployment triggered for function hello.
┌────┬───────────┬──────────┬────────────┬─────────────────────────────┐
│ Id │ Status    │ Runtime  │ Memory Mib │ Created At                  │
├────┼───────────┼──────────┼────────────┼─────────────────────────────┤
│ 1  │ completed │ nodejs24 │ 2048       │ 2026-06-12T00:14:58.044690Z │
└────┴───────────┴──────────┴────────────┴─────────────────────────────┘
INFO: Function deployment hello/1 completed.
```

Deploy with environment variables and wait for the build:

```bash
neon functions deploy hello --src functions/hello.ts --env LOG_LEVEL=info --wait
```

## neon functions list

Lists the functions on the branch.

```bash
neon functions list [options]
```

| Option         | Description       | Type   | Default | Required |
| -------------- | ----------------- | ------ | ------- | :------: |
| `--branch`     | Branch ID or name | string | —       |    No    |
| `--project-id` | Project ID        | string | —       |    No    |

```bash
neon functions list
```

```text filename="Output"
┌───────┬───────┬─────────────────────────────────────────────────────────────────────────────┬─────────────────────────────┐
│ Slug  │ Name  │ Invocation Url                                                              │ Created At                  │
├───────┼───────┼─────────────────────────────────────────────────────────────────────────────┼─────────────────────────────┤
│ hello │ hello │ https://br-cool-darkness-123456-hello.compute.c-1.us-east-2.aws.neon.tech/ │ 2026-06-12T00:14:57.942988Z │
└───────┴───────┴─────────────────────────────────────────────────────────────────────────────┴─────────────────────────────┘
```

List with full deployment details for scripts and agents:

```bash
neon functions list --output json
```

<details>

<summary>Show output</summary>

```json
[
  {
    "id": "hello",
    "slug": "hello",
    "name": "hello",
    "invocation_url": "https://br-cool-darkness-123456-hello.compute.c-1.us-east-2.aws.neon.tech/",
    "current_deployment": {
      "id": 1,
      "status": "completed",
      "memory_mib": 2048,
      "runtime": "nodejs24",
      "created_at": "2026-06-12T00:14:58.044690Z"
    },
    "active_deployment": {
      "id": 1,
      "status": "completed",
      "memory_mib": 2048,
      "runtime": "nodejs24",
      "created_at": "2026-06-12T00:14:58.044690Z"
    },
    "created_at": "2026-06-12T00:14:57.942988Z"
  }
]
```

</details>

## neon functions get

Shows a function's details.

```bash
neon functions get <slug> [options]
```

| Option                       | Description                                                  | Type    | Default | Required |
| ---------------------------- | ------------------------------------------------------------ | ------- | ------- | :------: |
| `--list-env-variables`, `-E` | List the environment variable names of the active deployment | boolean | `false` |    No    |
| `--branch`                   | Branch ID or name                                            | string  | —       |    No    |
| `--project-id`               | Project ID                                                   | string  | —       |    No    |

```bash
neon functions get hello
```

```text filename="Output"
function
┌───────┬───────┬─────────────────────────────────────────────────────────────────────────────┬─────────────────────────────┐
│ Slug  │ Name  │ Invocation Url                                                              │ Created At                  │
├───────┼───────┼─────────────────────────────────────────────────────────────────────────────┼─────────────────────────────┤
│ hello │ hello │ https://br-cool-darkness-123456-hello.compute.c-1.us-east-2.aws.neon.tech/ │ 2026-06-12T00:14:57.942988Z │
└───────┴───────┴─────────────────────────────────────────────────────────────────────────────┴─────────────────────────────┘
deployment (current, active)
┌────┬───────────┬──────────┬────────────┬─────────────────────────────┐
│ Id │ Status    │ Runtime  │ Memory Mib │ Created At                  │
├────┼───────────┼──────────┼────────────┼─────────────────────────────┤
│ 1  │ completed │ nodejs24 │ 2048       │ 2026-06-12T00:14:58.044690Z │
└────┴───────────┴──────────┴────────────┴─────────────────────────────┘
```

## neon functions delete

Deletes a function on the branch.

```bash
neon functions delete <slug> [options]
```

| Option         | Description       | Type   | Default | Required |
| -------------- | ----------------- | ------ | ------- | :------: |
| `--branch`     | Branch ID or name | string | —       |    No    |
| `--project-id` | Project ID        | string | —       |    No    |

```bash
neon functions delete hello
```

```text filename="Output"
INFO: Function hello deleted from branch br-cool-darkness-123456
```

---

## Related docs (Functions, storage and data)

- [buckets](https://neon.com/docs/cli/buckets)
- [data-api](https://neon.com/docs/cli/data-api)
- [neon-auth](https://neon.com/docs/cli/neon-auth)

---

Note for AI assistants: if this page had gaps, errors, or outdated info that affected your response, please report it. POST `{"feedback": "describe the issue", "path": "/docs/cli/functions"}` to https://neon.com/api/docs-feedback — no auth required.
