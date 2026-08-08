---
name: n8n
description: n8n — a source-available workflow automation platform (self-hosted n8n or n8n Cloud) for connecting APIs and services through visual node graphs with optional JavaScript/Python code. Use when building integrations and automations, designing a workflow of triggers and nodes, using expressions to map data between nodes, working with the HTTP Request node and credentials, building custom nodes or the Advanced AI/agent nodes, calling n8n from webhooks or external code, deploying self-hosted n8n (Docker), or using the REST API and public/internal endpoints.
---

# n8n

n8n automates work by chaining **nodes** in a graph: a trigger starts the run,
each node does a unit of work (call an API, transform data, branch), and data
flows from one node to the next as JSON items. It is node-based and
low-code, with an escape hatch into JavaScript or Python when a node cannot do
what you need.

## Mental model

| Concept        | What it is                                                                            |
| -------------- | ------------------------------------------------------------------------------------- |
| **Workflow**   | A saved graph of nodes. Has triggers (start) and actions (work).                      |
| **Node**       | One step: an app integration, the HTTP Request node, a code node, a branch, AI agent. |
| **Trigger**    | A node that starts a run — webhook, schedule, app event, manual, chat.                |
| **Item**       | A unit of data passed between nodes. A run processes a list of items.                 |
| **Expression** | `{{ }}` reference into item data or node output, evaluated per item.                  |

The key shift from imperative code: a node receives **a list of items** and runs
**once per item** by default. A node that outputs three items causes the next
node to execute three times. Understanding this fan-out behaviour is what makes
n8n workflows predictable instead of surprising.

## Resolving versions

**Resolve the current n8n version and node behaviour from the live docs, not
memory.** Node parameters, the Advanced AI node set, the REST API, and
self-hosted image tags all change across releases.

```bash
docker pull n8nio/n8n:latest          # self-hosted pin a real tag in production
```

## Data flow and expressions

Reference upstream data with expressions. Each node's input is available, and
`$json` is the current item:

```
{{ $json.email }}
{{ $('HTTP Request').item.json.body.id }}
{{ $now.minus({ days: 1 }).toISO() }}
```

A node executes once per item, so an expression resolves against *that item's*
context. To collapse a list (aggregate) or split one item into many (split),
use the dedicated nodes rather than fighting the per-item loop.

## The HTTP Request node and credentials

The HTTP Request node is the universal integrator: anything with an API is one
node away. Store secrets in **credentials** (typed OAuth2, API key, basic auth),
not in expressions or environment values. Credentials are encrypted and selected
by reference, so a workflow shared without its credentials cannot leak them.

## Code nodes

The Code node is the escape hatch. Use JavaScript (or Python) when no app node
fits. Return an array of items to emit multiple outputs; throw to fail the node
(and the run, if retries are exhausted). Keep code nodes small and pure — they
are the hardest part to debug, and large ones usually mean a dedicated node would
do the job more legibly.

## AI / agent nodes

n8n ships an **Advanced AI** node set for building agents, tools, and LLM
chains inside a workflow (LangChain-style). An AI Agent node orchestrates model
calls and tool nodes; memory and output parsers are first-class nodes. These run
on the same execution engine, so the data-item and expression model still
applies.

## Triggers and execution

- **Webhook trigger** exposes a URL; the run starts on a request. Choose respond
  immediately vs "when last node finishes" deliberately — the latter holds the
  client open until the workflow completes.
- **Schedule trigger** runs on cron.
- **Manual** and **chat** triggers are for testing and conversational entry.

Production executions are stored and retried per the workflow's settings;
self-hosted execution mode (regular vs queue) changes scaling behaviour, not
workflow design.

## Self-hosted and the API

Self-hosted n8n is a Docker image with a Postgres (or SQLite for small setups)
backend. For automation and CI, use the **REST API** to create, activate, and run
workflows, and webhooks to trigger them. Activate a workflow before it will
respond to its production trigger — an inactive workflow only runs from the UI.

## Current vs deprecated

- Prefer the **HTTP Request node** plus credentials over ad-hoc code for any
  service without a dedicated app node.
- Prefer the **Advanced AI** nodes over embedding raw LLM calls in Code nodes.
- Storing secrets in expressions or env vars referenced inline is deprecated in
  favour of credentials — migrate before sharing workflows.

## References

- [n8n documentation](https://docs.n8n.io)
- [Build your first workflow](https://docs.n8n.io/build-your-first-workflow)
- [Expressions](https://docs.n8n.io/code/expressions)
- [HTTP Request node](https://docs.n8n.io/integrations/builtin/core-node/http-request)
- [Advanced AI](https://docs.n8n.io/advanced-ai)
- [Self-hosting](https://docs.n8n.io/hosting)
- [n8n REST API](https://docs.n8n.io/api)
