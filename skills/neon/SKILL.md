---
name: neon
description: Neon — serverless Postgres with database branching, scale-to-zero, and a connection pooler. Use when provisioning Postgres, modelling data for an autoscaling database, using copy-on-write branches for dev/preview/migrations, sizing the autoscaler, choosing connection options (pooled vs direct, the -pooler endpoint), running the CLI, calling the API, configuring extensions, or integrating Neon with frameworks and edge platforms.
---

# Neon

Neon is fully-managed Postgres that separates compute from storage. Storage is a
copy-on-write page server; compute is a Postgres instance that can start, stop,
and scale on demand. That split unlocks three things bare Postgres does not give
you out of the box: **branches** of a database, **scale-to-zero** idle computes,
and a control-plane **API** for provisioning.

The SQL surface is ordinary Postgres — drivers, queries, extensions, and the
wire protocol are unchanged. What changes is how you obtain a database, where it
connects, and how it behaves when idle.

## Mental model

| Concept              | What it is                                                                                                  |
| -------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Project**          | A group of branches sharing one storage root. The unit the API and dashboard operate on.                    |
| **Branch**           | A copy-on-write clone of a branch. Main is the default; child branches share parent's data history at creation. |
| **Compute / endpoint** | A running Postgres instance attached to a branch. Starts on connect, suspends when idle (scale-to-zero).   |
| **Connection string** | Per-endpoint. The `-pooler` host routes through PgBouncer (pooled, port 5432/6543); the bare host is direct. |

A branch is cheap because it only stores the pages it has *changed* since it
forked. Creating a branch does not copy the database; writing to it does, page
by page. This is why branching for a pull request or a risky migration is the
canonical Neon workflow.

## Resolving versions and the live endpoint

**Resolve the current Postgres version and endpoint host from the console or
API, not memory.** Neon upgrades major versions over time, and the exact host
(`<project>-<branch>.<region>.aws.neon.tech` or the `-pooler` variant) is
per-project.

```bash
# List your endpoints and hosts
neon branches list
neon endpoints list
```

## Connection: pooled vs direct

This is the decision that breaks the most deployments.

- **Pooled (`-pooler` host, port `5432`):** PgBouncer in transaction mode. Use
  this for serverless functions, edge runtimes, and any client that opens many
  short-lived connections. **Transaction-mode pooling breaks features that need
  a session**: prepared statements (some drivers prepare implicitly), `LISTEN`/
  `NOTIFY`, advisory locks held across statements, `SET` outside a transaction,
  and cursors that live across transactions.
- **Direct (bare host, port `5432`):** A real Postgres connection. Use for
  migrations, long-lived servers, and anything relying on session state.

If a driver error mentions prepared statements or "session" while using the
pooled endpoint, switch that workload to the direct endpoint or disable
client-side prepared statements. Do not "fix" it by raising the pool size.

## Branching workflows

```bash
neon branches create --name migrate-test --parent main
# ... run migrations against the new branch's connection string ...
neon branches reset main --branch migrate-test   # promote after verifying
```

Each branch has its own connection string. Point your preview environment at the
branch endpoint, run the migration there, and only promote once it is green.
Resetting main to a child branch fast-forwards main to the child's state; the
original main pages are retained by history.

Scale-to-zero means a cold branch's first query pays a startup latency (seconds,
not milliseconds). Health checks that open a connection every few seconds will
keep it warm — and will keep it billable.

## The autoscaler

When autoscaling is enabled, compute CPU and memory bounds are min/max (Compute
Units). The autoscaler scales within that range; it does not provision
indefinitely. Set the minimum high enough to avoid restarts under steady load,
and the ceiling to the peak you are willing to pay for. Suspended-scale-to-zero
and the autoscaler are complementary but distinct settings — configure both
deliberately.

## Extensions and the connection pooler

`CREATE EXTENSION` works as in vanilla Postgres but some extensions require
session state or background workers that are incompatible with transaction-mode
pooling. Run extension setup on the direct endpoint, and verify the extension's
runtime requirements against the pooled endpoint before relying on it from a
serverless client.

## Current vs deprecated

- Prefer the **API** and **CLI** over scraping the dashboard. The API is the
  source of truth for project, branch, and endpoint state and is stable.
- Branch `main` is the conventional storage root; older docs may reference it as
  the "primary" — same thing.
- The PgBouncer pooled endpoint is the recommended default for application
  traffic; direct is the exception, not the rule.

## References

- [Neon documentation](https://neon.com/docs)
- [Connection pooling](https://neon.com/docs/connect/connection-pooling)
- [Branching](https://neon.com/docs/introduction/branching)
- [Neon API reference](https://neon.com/docs/reference/api)
- [Neon CLI](https://neon.com/docs/reference/neon-cli)
