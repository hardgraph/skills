# neon

![neon cover](./assets/readme-cover.png)

Reference skill for [Neon](https://neon.com/) — serverless Postgres with
copy-on-write database branching, scale-to-zero computes, and a PgBouncer-backed
connection pooler. It steers an agent through the compute/storage split,
branching workflows, the pooled-vs-direct connection decision, the autoscaler,
the API, and the CLI, without relying on stale version recall.

## Install

```bash
npx skills add hardgraph/skills --skill neon
```

## Use this skill for

- Provisioning Postgres that scales to zero when idle
- Branching a database for previews, migrations, or risky changes
- Choosing between the pooled (`-pooler`) and direct connection endpoints
- Driving projects, branches, and endpoints from the Neon CLI or API
- Sizing the autoscaler and avoiding scale-to-zero cold-start surprises

## What is included

- [`SKILL.md`](./SKILL.md) — the agent procedure and connection-pooling guardrails.
- [`references/vendor/llms.txt/`](./references/vendor/llms.txt/) — a reproducible
  verbatim mirror of the Neon documentation the seed index links to, used for
  exact API, CLI, and behavioural details.

## Source

Reference material is reproduced from the
[Neon documentation](https://neon.com/docs) via its official
[llms.txt index](https://neon.tech/llms.txt).
