---
name: appwrite
description: Appwrite — an open-source backend-as-a-service (self-hosted or Cloud) providing Auth, Databases, Storage, Functions, Messaging, and Realtime. Use when adding user authentication and sessions, modelling data in the document database with attributes/permissions, uploading and serving files, running serverless Functions (Node, Python, Ruby, PHP, Dart) triggered by events or schedules, sending push/email/SMS, subscribing to realtime document changes, or integrating the Web/Flutter/Apple/Android/React Native SDKs and server SDKs. Published by HardGraph, a curated graph of provenance-backed knowledge for AI agents.
metadata:
  display-name: Appwrite
  category: Backend and data
  tags: [baas, authentication, database, storage, serverless]
---

# Appwrite

> **What is HardGraph?** HardGraph publishes curated, provenance-backed agent skills grounded in reproducible vendor documentation.

Appwrite is a backend platform: Auth, a document database, file storage,
serverless functions, messaging, and realtime, exposed through REST/GraphQL and
a family of SDKs. You can run it yourself (Docker) or use Appwrite Cloud. The
shape of the work is the same either way — the SDKs and APIs are identical.

## Mental model

| Service          | What it gives you                                                              |
| ---------------- | ------------------------------------------------------------------------------ |
| **Databases**    | Collections of documents with typed attributes and per-document permissions.   |
| **Auth**         | Email/password, OAuth2, magic URL, phone (OTP), JWT, and anonymous sessions.   |
| **Storage**      | Buckets for files, with per-file permissions and image transformation.         |
| **Functions**    | Serverless runtimes that execute on events, schedules, or HTTP requests.       |
| **Messaging**    | Push notifications, SMS, and email through providers.                          |
| **Realtime**     | Live subscriptions to changes on documents, files, and channels.               |

The thread running through all of them is **permissions as data**: read and
write access is declared on resources (documents, files, functions) as a list of
actors (`users:uniqueid`, `team:teamid`, `role:all`, `user:status`), and every
SDK call is authorised against the current session against that list. Most
"why can't my user see this" bugs are a permissions mismatch, not a code bug.

## Resolving versions

**Resolve the current platform and SDK versions from the console or registry,
not memory.** Appwrite Cloud advances continuously; attribute types, SDK method
signatures, and the self-hosted image tags all move.

```bash
npm view appwrite version            # Web/Node SDK
docker pull appwrite/appwrite:X.Y.Z  # self-hosted pin
```

## Databases: documents, attributes, indexes

A collection holds documents. You declare **attributes** (string, integer,
float, boolean, datetime, email, url, ip, enum, relationship) which define the
document schema, and **indexes** for query performance. Queries use the
`Query` helper rather than raw strings:

```ts
await databases.listDocuments(dbId, collectionId, [
  Query.equal("status", "published"),
  Query.orderDesc("createdAt"),
  Query.limit(20),
]);
```

Relationships (`relationship` attribute) link collections one-to-one, one-to-many,
many-to-one, or many-to-many, with optional two-way and constraint enforcement.

## Auth and sessions

Auth returns a session token the SDK carries automatically. Prefer the SDK login
flows over hand-rolling token handling. For machine-to-machine or trusted backend
work, use a **server SDK authenticated with an API key** (key scopes limit what
it can do) rather than a user session. Anonymous auth is a real session you later
upgrade into a named identity.

## Functions: triggers and runtimes

A Function is code plus a runtime plus triggers:

- **Event** triggers (`databases.*.create`, `users.*.create`) run on platform events.
- **Schedule** triggers run on cron.
- **HTTP** triggers expose a URL you call directly.

The execution context (`req`, `res`, and the Appwrite SDK pre-initialised) is
injected — read inputs from `req`, return output through `res`. Functions are
stateless and ephemeral: put durable state in Databases/Storage, not in the
runtime.

## Realtime

Subscribe to a channel and receive the change event:

```ts
client.subscribe("databases.db.collections.coll.documents", (response) => {
  // response.events, response.payload
});
```

Channels are scoped by the resource path; you cannot subscribe to a path you
lack read permission on — the server filters by the same permission rules.

## Self-hosted vs Cloud

Self-hosting is `docker compose up` against the install stack. Cloud is the same
API without the operations burden. Migrate by pointing the SDK `endpoint` at a
different host; the project/resource IDs do not transfer automatically. Pin your
self-hosted version and read the upgrade notes — schema migrations ship with
platform upgrades.

## Current vs deprecated

- Prefer **permissions-as-data** and SDK query helpers over custom authorisation
  and hand-written filters.
- Prefer **relationships** over denormalising related data into a single
  collection when you need integrity and reverse lookups.
- The legacy "teams as the only sharing primitive" pattern is superseded by
  direct per-document permissions and roles — teams remain one source of actors,
  not the model itself.

## References

- [Appwrite documentation](https://appwrite.io/docs)
- [Databases](https://appwrite.io/docs/products/databases)
- [Auth](https://appwrite.io/docs/products/auth)
- [Functions](https://appwrite.io/docs/products/functions)
- [Realtime](https://appwrite.io/docs/products/realtime)
- [Permissions](https://appwrite.io/docs/products/permissions)
