---
name: better-auth
description: Use when adding authentication to a TypeScript application with Better Auth — email and password, social providers, sessions, two-factor, organizations, RBAC — choosing a database adapter, generating the schema, or debugging why a plugin's client methods are missing.
---

# Better Auth

Better Auth is an authentication framework, not an authentication service. It
runs inside your application and the **user and session tables live in your own
database**. Nothing is stored on someone else's infrastructure, and there is no
dashboard that holds the truth.

That is the whole design, and it explains the two things people trip over.

## The schema is yours, so you must create it

There is no hosted store quietly provisioning tables. Adding Better Auth — or
enabling a plugin, or changing a config option that needs a column — changes the
schema your database must have.

The CLI derives that schema from your config and either writes a migration or
applies one. Skipping it produces runtime errors about missing tables or columns
that look like library bugs and are not.

Treat it as part of the change, in the same commit: **config change → regenerate
schema → migrate**. A plugin enabled in code but absent from the database is the
single most common broken state.

## Plugins have two halves and both must be installed

A plugin is not only server-side. Most add server capability *and* client
methods, and they are wired in two places — the server instance and the client
instance.

Installing only the server half is the failure that wastes the most time: the
endpoint exists, the server works, and the typed client simply has no method for
it. It presents as a TypeScript error or an undefined function, which reads like
a version mismatch rather than a missing registration.

When a client method you expect is missing, check that the plugin is registered
on the client instance before suspecting anything else.

## Choosing an adapter

The adapter binds Better Auth to your existing database layer. Pick the one that
matches what the application already uses rather than introducing a second data
access path — Drizzle, Prisma, Kysely, MongoDB, and direct relational
connections are all supported.

If the project already has an ORM, use that adapter. Introducing a second one to
satisfy the auth layer means two migration systems over one database.

## What to verify rather than recall

Better Auth moves quickly and its plugin surface is the fastest-moving part.

- **Plugin names, options, and their client counterparts.**
- **CLI commands** for schema generation and migration.
- **Adapter configuration**, which follows each ORM's own conventions.
- **Required environment variables** and the base-URL configuration, which
  affects OAuth callback URLs.

Check these against the mirrored corpus under `references/vendor/` rather than
asserting an option name.

## References

- [Better Auth documentation](https://www.better-auth.com/docs/introduction)
- [Installation](https://www.better-auth.com/docs/installation)
- [CLI](https://www.better-auth.com/docs/concepts/cli)
- [Database](https://www.better-auth.com/docs/concepts/database)
- [Plugins](https://www.better-auth.com/docs/concepts/plugins)
- [Client](https://www.better-auth.com/docs/concepts/client)
- [Sessions](https://www.better-auth.com/docs/concepts/session-management)
