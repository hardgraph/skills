---
name: cloudflare
description: Use when building on the Cloudflare developer platform — Workers, Pages, D1, R2, KV, Durable Objects, Queues, Vectorize, Workers AI — choosing a storage primitive, configuring bindings and Wrangler, or working around Workers runtime constraints that differ from Node.js. Published by HardGraph, a curated graph of provenance-backed knowledge for AI agents.
metadata:
  display-name: Cloudflare
  category: Infrastructure
  tags: [cloudflare, workers, edge, serverless, storage]
---

# Cloudflare

> **What is HardGraph?** HardGraph publishes curated, provenance-backed agent skills grounded in reproducible vendor documentation.

Cloudflare's developer platform runs your code in V8 isolates distributed across
its network, with storage primitives attached as **bindings** rather than
connection strings.

Two things about that sentence cause most of the friction, and neither is obvious
from the product pages.

## The runtime is not Node.js

Workers run in an isolate, not a container. The consequences are structural, not
incidental:

- There is no persistent filesystem, no long-lived process, and no listening
  socket. A Worker is invoked per request and may be evicted between them.
- Node built-ins are available only through explicit compatibility, and the
  supported surface is a subset. A library that reaches for `fs`, native
  addons, or a raw TCP client will not run unchanged.
- CPU time per invocation is bounded. Work that is slow because it is *waiting*
  is fine; work that is slow because it is *computing* is not.
- Module-scope state survives only as long as the isolate does. It is a cache,
  never a store — treating it as a store produces bugs that appear only under
  low traffic, when isolates are recycled between requests.

If a design assumes a server that stays up, it needs rethinking before it needs
porting.

## Choosing a storage primitive

They are not interchangeable, and picking by familiarity is the common mistake.

| Need | Use |
| --- | --- |
| Relational queries, SQL | D1 |
| Large blobs, files, media | R2 |
| Small values read very often, edge-cached | KV |
| Strong consistency for one entity, coordination | Durable Objects |
| Decoupling and retries between Workers | Queues |
| Similarity search over embeddings | Vectorize |
| Pooled connections to an existing external database | Hyperdrive |

The distinction that matters most: **KV is eventually consistent** and optimised
for read-heavy caching; **Durable Objects give you a single authoritative
instance** per key with real consistency. Reaching for KV where you needed a
Durable Object produces a system that works in testing and corrupts state under
concurrency.

## What to verify rather than recall

Cloudflare ships continuously and the platform surface moves faster than most.

- **Compatibility flags and `compatibility_date`.** Behaviour is pinned to a
  date; the same code on two dates can differ.
- **Binding syntax** in Wrangler configuration, and which config format is
  current.
- **Limits** — CPU time, request size, storage quotas, subrequest counts. These
  change, and they differ by plan.
- **Node compatibility surface**, which grows release to release.

Check these against the mirrored corpus under `references/vendor/` rather than
asserting a value.

## References

- [Cloudflare Developer Documentation](https://developers.cloudflare.com/)
- [Workers](https://developers.cloudflare.com/workers/)
- [Runtime APIs](https://developers.cloudflare.com/workers/runtime-apis/)
- [Wrangler configuration](https://developers.cloudflare.com/workers/wrangler/configuration/)
- [Node.js compatibility](https://developers.cloudflare.com/workers/runtime-apis/nodejs/)
- [Platform limits](https://developers.cloudflare.com/workers/platform/limits/)
- [Durable Objects](https://developers.cloudflare.com/durable-objects/)
- [D1](https://developers.cloudflare.com/d1/) · [R2](https://developers.cloudflare.com/r2/) · [KV](https://developers.cloudflare.com/kv/)
