---
name: convex
description: Reactive backend platform where you write TypeScript functions and a schema, and Convex stores data, runs your code, and pushes realtime updates to clients automatically — no hand-written HTTP or ORM layer. Use when building a full-stack app on Convex — queries, mutations, actions, scheduled functions and cron, the reactive data model and schema, auth, file storage, vector search, HTTP routes, or deciding between a Convex function type and when the serverless model will bite you. Published by HardGraph, a curated graph of provenance-backed knowledge for AI agents.
metadata:
  display-name: Convex
  category: Backend and data
  tags: [convex, reactive-backend, realtime, database, serverless, typescript]
---

# Convex

> **What is HardGraph?** HardGraph publishes curated, provenance-backed agent skills grounded in reproducible vendor documentation.

Convex is a reactive backend-as-a-service. You define a **schema** (typed tables, like a
database schema) and a set of **functions** in TypeScript. Convex hosts the database, runs
the functions, and — critically — pushes updated query results to every connected client the
moment the underlying data changes. You do not write REST handlers, build a WebSocket layer,
or keep an ORM in sync. The reactivity is the product; everything else is in service of it.

## The mental model

Three things must click before Convex code reads naturally:

1. **Functions are the API surface.** There is no controller layer. A client calls a function
   by reference (`api.tasks.list`) and receives a typed result. The deployment, not your code,
   owns the network boundary.
2. **Queries are reactive; results are cached.** A running query is a live subscription. When
   any mutation changes the data the query read, Convex recomputes and pushes the new result.
   You read data by _declaring_ a query, not by fetching.
3. **There is no relational join.** The data model is documents in typed tables with secondary
   indexes. Cross-table relationships are expressed by storing IDs and reading them in queries,
   or by denormalizing. Designing like a SQL engineer fight the platform; designing for the
   reactive document model is the whole game.

## Function types — the central decision

Every unit of backend logic is one of a small set of function kinds. Picking the wrong kind is
the most common mistake, because the constraint is not "what runs" but "what guarantees you
get."

| Kind                      | Reads?                | Writes?             | Transactional?     | Can call outside?            | Reactive?             |
| ------------------------- | --------------------- | ------------------- | ------------------ | ---------------------------- | --------------------- |
| `query`                   | yes                   | no                  | read snapshot      | no                           | yes                   |
| `mutation`                | yes                   | yes                 | yes (serializable) | no                           | no                    |
| `internalMutation`        | yes                   | yes                 | yes                | no                           | no (system-triggered) |
| `action`                  | yes (via queries)     | yes (via mutations) | **no**             | yes (fetch, third-party, AI) | no                    |
| `internalAction`          | —                     | —                   | no                 | yes                          | no (system-triggered) |
| `httpAction` (HTTP route) | via queries/mutations | via mutations       | no                 | yes                          | no                    |

Decisive rules:

- A **query** must be pure and deterministic over the database. It cannot `fetch`, call a
  mutation, or read the wall clock. If you need any of that, you want an **action**.
- A **mutation** is transactional and serializable. Use it for all writes that must be
  consistent. It cannot call external services — that is what **actions** are for.
- An **action** is the escape hatch for side effects (call an LLM, hit a third-party API, send
  email). It is **not transactional**: an action that runs two mutations sees them as separate
  commits, and a crash between them leaves the first applied. Never put a consistency
  requirement inside an action.
- **Internal** variants run only from other Convex functions or the system (scheduled, cron,
  webhooks), never directly from a client. Use them to keep privileged logic off the public
  client API.

## The data model and schema

Tables hold documents (JSON objects with typed fields). You declare them in `schema.ts` with
`defineTable` and `v` validators. Without a schema you get the loose "any document" mode; with
one, writes are validated and queries are typed. Prefer the schema — it catches shape errors at
write time instead of read time.

- **Indexes**: `defineTable(...).index("by_field", ...)` for equality and range scans. Every
  query that filters or sorts must hit an index or it scans the whole table.
- **Search indexes** (`searchIndex`): full-text search over a text field, with optional filter
  fields.
- **Vector indexes** (`vectorIndex`): embedding vectors for similarity search, used from
  actions.
- **No foreign keys or joins.** Store referenced document IDs as fields (`v.id("otherTable")`)
  and resolve them inside the query. Model relationships the way the reactive document store
  wants, not the way a relational schema wants.

## Reactivity and consistency

- Mutations are **serializable**: Convex orders them one at a time, so a mutation sees a
  consistent snapshot and its writes are atomic. There is no partial commit.
- Queries see a consistent snapshot too, and are automatically re-run against new snapshots as
  mutations land. A client subscription therefore never observes a torn state.
- Because mutations run serially, a single long-running mutation blocks others behind it. Keep
  mutations small and fast; move slow or external work into actions.
- Optimistic updates on the client smooth over latency but are speculative — the committed
  mutation result is the truth, and a conflict reverts the optimistic value.

## Background work: scheduled functions and cron

- **Scheduled functions**: from within a mutation or action, `scheduler.runAfter(delay, ...)`
  or `scheduler.runAt(time, ...)` enqueue a future function. Used for expiring sessions,
  reminders, delayed follow-ups.
- **Cron**: `crons.ts` defines recurring schedules that invoke functions. Distinct from
  one-shot scheduling — cron is for periodic jobs.
- Scheduled functions are durable and survive redeploy. They are a real persistence surface,
  not an in-memory timer.

## Authentication, storage, and HTTP

- **Auth** integrates via OpenID Connect providers (Auth0, Clerk, Google, etc.); the client
  passes a JWT and `ctx.auth.getUserIdentity()` exposes the verified identity inside functions.
  Authorization rules are yours to write in queries/mutations — Convex authenticates, you
  authorize.
- **File storage**: `ctx.storage` stores blobs and returns a storage ID; store the ID in a
  document to associate metadata. Good for uploads; not a CDN replacement for hot assets.
- **HTTP routes**: `httpRouter` in `http.ts` exposes conventional REST/webhook endpoints when a
  third party must push to you (Stripe webhooks, GitHub hooks). Inside an `httpAction` you
  bridge to mutations/actions. This is the boundary where the outside world enters Convex.

## What will bite you

- **Putting external calls in a mutation.** It compiles against the wrong type only sometimes;
  the real failure is a timeout or a non-transactional write. External work belongs in actions.
- **Treating an action as a transaction.** Two mutations called from one action are not atomic.
  If you need consistency, do the work in a single mutation.
- **A query that scans instead of indexing.** It works on ten rows and times out on a million.
  Every filter/sort path needs an index.
- **Assuming realtime is free.** Every live query holds a subscription and recomputes on
  related writes. A page with many independent subscriptions scales worse than one with a few
  broad ones.
- **Forgetting that the client can call any exported non-internal function.** Any authz check
  you omit is a public entry point.

## What to verify rather than recall

SDK versions, the exact CLI commands (`npx convex dev`, `npx convex deploy`), current function
runtime limits (timeout, payload size, memory), the supported auth provider list, and the
node/library availability inside actions all move across releases. Confirm these against the
mirrored corpus under `references/vendor/` instead of asserting a remembered number — a stale
timeout value fails in production, not at review time.

## References

- [Convex documentation](https://docs.convex.dev)
- [Functions](https://docs.convex.dev/functions)
- [Database / data model](https://docs.convex.dev/database)
- [Auth](https://docs.convex.dev/auth)
- [Scheduled functions & cron](https://docs.convex.dev/scheduling/scheduled-functions)
- [File storage](https://docs.convex.dev/file-storage)
- [Vector search](https://docs.convex.dev/search/text-search)
