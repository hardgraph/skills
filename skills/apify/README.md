# apify

![apify cover](./assets/readme-cover.png)

Reference skill for [Apify](https://apify.com/) — the serverless cloud platform
for web scraping, browser automation, and data extraction via Actors. It steers
an agent through Actors and their lifecycle, the REST API v2, the CLI and SDKs,
storage (datasets, key-value stores, request queues), dataset exports, the proxy
network, and scheduling/chaining, without relying on stale version recall.

## Install

```bash
npx skills add hardgraph/skills --skill apify
```

## Use this skill for

- Running an Apify Actor to scrape a site and reading its dataset results
- Triggering Actor runs over the REST API and fetching dataset items
- Building and deploying an Actor with an input schema from the CLI
- Choosing between the Console, API, CLI, and SDK for a task
- Exporting a run's dataset as JSON, CSV, or Excel
- Scheduling runs and chaining Actors with webhooks
- Using Apify's residential/datacenter proxy for anti-blocking

## What is included

- [`SKILL.md`](./SKILL.md) — the agent procedure and the surface-selection decision criteria.
- [`references/vendor/llms.txt/`](./references/vendor/llms.txt/) — a reproducible
  verbatim mirror of the Apify documentation the seed index links to, used for
  exact API endpoints, CLI commands, and SDK usage.

## Source

Reference material is reproduced from the
[Apify documentation](https://docs.apify.com) via its official
[llms.txt index](https://docs.apify.com/llms.txt).
