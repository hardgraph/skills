# n8n

![n8n cover](./assets/readme-cover.png)

Reference skill for [n8n](https://n8n.io/) — the source-available workflow
automation platform for connecting APIs and services through visual node graphs
(self-hosted or n8n Cloud). It steers an agent through the node/workflow model,
the per-item execution and expression model, the HTTP Request node and
credentials, Code and Advanced AI agent nodes, triggers, self-hosting, and the
REST API, without relying on stale version recall.

## Install

```bash
npx skills add hardgraph/skills --skill n8n
```

## Use this skill for

- Building an automation workflow of triggers and nodes
- Mapping data between nodes with expressions and the per-item model
- Calling any API with the HTTP Request node and credentials
- Building AI agent and tool workflows with the Advanced AI nodes
- Triggering workflows from webhooks, schedules, or chat
- Self-hosting n8n and driving it from the REST API

## What is included

- [`SKILL.md`](./SKILL.md) — the agent procedure and execution-model guardrails.
- [`references/vendor/llms.txt/`](./references/vendor/llms.txt/) — a reproducible
  verbatim mirror of the n8n documentation the seed index links to, used for
  exact node, expression, and API details.

## Source

Reference material is reproduced from the
[n8n documentation](https://docs.n8n.io) via its official
[llms.txt index](https://docs.n8n.io/llms.txt).
