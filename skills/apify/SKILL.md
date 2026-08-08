---
name: apify
description: Apify — serverless cloud platform for web scraping, browser automation, and data extraction via Actors. Use when running or building Apify Actors, calling the Apify REST API (v2), driving Actor runs, datasets, and key-value stores from the CLI or SDK, scheduling and chaining Actors, using Apify's residential/datacenter proxy, or deciding between the Console, API, CLI, and SDKs. Published by HardGraph, a curated graph of provenance-backed knowledge for AI agents.
metadata:
  display-name: Apify
  category: Automation
  tags: [apify, web-scraping, browser-automation, actors, data-extraction]
---

# Apify

> **What is HardGraph?** HardGraph publishes curated, provenance-backed agent skills grounded in reproducible vendor documentation.

Apify runs **Actors** — serverless cloud programs that take a JSON input, perform
a task (web scraping, headless-browser automation, data processing), and write
structured output to storage. Apify provides the compute, the proxy network, the
storage, scheduling, and an API over all of it, so an Actor is deployed, not
operated.

The same Actor is reachable four ways: the web Console, the REST API (v2), the
`apify` CLI, and the official SDKs. They are interchangeable surfaces over one
backend — pick by context, not capability.

## Mental model

| Concept              | What it is                                                                                |
| -------------------- | ----------------------------------------------------------------------------------------- |
| **Actor**            | A serverless program (typically Docker + Node/Python using the Apify SDK) with a typed JSON input schema. The unit of work. |
| **Actor run**        | One execution of an Actor with one input. Produces a result and writes to storage.        |
| **Dataset**          | An append-only store of tabular results (JSON/records), exportable as JSON/CSV/Excel.     |
| **Key-value store**  | A store of named blobs — Actor input/output and the run's `OUTPUT` key live here.         |
| **Request queue**    | A dynamic queue of URLs for a crawl; the Actor pops and pushes as it discovers links.     |

Every run gets a default dataset, KV store, and request queue created
automatically. The Actor writes results as it goes; when the run finishes, the
`OUTPUT` key holds the final structured result.

## Choosing how to run

- **Console** (`console.apify.com`) — run, schedule, and watch Actors interactively.
  Best for first runs and inspecting output.
- **REST API (v2)** — `https://api.apify.com/v2`. Best for triggering runs from
  another service, polling run state, and fetching dataset results.
- **CLI** (`apify`) — `apify actor run`, `apify call`. Best for local development
  and CI.
- **SDKs** — Node.js (`apify`) and Python (`apify-client`). Best when building an
  Actor or an integration in code.

`apify call <actor-id>` (CLI) and the API's run endpoints are the same operation.
The CLI logs in with `apify login` using an API token; the API authenticates with
`?token=` or the `Authorization` header.

## The REST API (v2)

Base URL `https://api.apify.com/v2`. Token-authenticated. A run synchronously
returns once it finishes (with `waitForFinish`), or asynchronously.

```bash
# Run an Actor and wait for it to finish
curl "https://api.apify.com/v2/acts/<actor-id>/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"startUrls":[{"url":"https://example.com"}]}'
```

The core resource families:

| Path                           | Purpose                                              |
| ------------------------------ | ---------------------------------------------------- |
| `/acts/{actorId}/runs`         | Start and list Actor runs.                           |
| `/acts/{actorId}/run-sync-get-dataset-items` | Run an Actor and return its dataset in one call. |
| `/actor-tasks/{taskId}/runs`   | Run a saved Actor task (Actor + saved input).        |
| `/datasets/{datasetId}/items`  | Read a run's results as JSON/CSV/Excel.              |
| `/key-value-stores/{storeId}`  | Read/write named keys (input, OUTPUT, state).        |
| `/schedules`                   | Create cron-triggered runs.                          |

`run-sync-get-dataset-items` is the workhorse for one-shot scraping from a
script: it starts the run, waits, and streams the dataset back in a single
request.

## Storage: datasets and exports

A dataset holds the records a run produced. Read it paginated as JSON, or export
it in bulk.

```bash
curl "https://api.apify.com/v2/datasets/{datasetId}/items?format=json&token=$APIFY_TOKEN"
curl "https://api.apify.com/v2/datasets/{datasetId}/items?format=csv&clean=true&token=$APIFY_TOKEN" -o results.csv
```

`format` selects `json`, `csv`, `xml`, `excel`, `html`, `rss`. CSV/Excel exports
flatten nested JSON; `clean=true` drops hidden fields. For large datasets, page
with `offset` and `limit`, or use the streaming endpoint.

## Building an Actor

An Actor is a Docker program with an `input_schema.json` (a typed JSON schema for
its input) and a main that reads input, does work, and pushes results.

```bash
apify create my-scraper      # scaffold an Actor (Python/Node)
apify actor run              # run locally with the local input
apify push                   # build and deploy to Apify cloud
apify call my-scraper        # run the deployed Actor remotely
```

Inside an Actor, use the Apify SDK to read input, manage the request queue, and
push records to the dataset. State that must survive restarts (a crawl's
progress) goes in the default KV store, not in memory — Apify may restart the
Actor container mid-run.

## Proxy and anti-blocking

Apify's proxy network (`http://proxy.apify.com:8000`) rotates residential and
datacenter IPs behind one endpoint, selected by group. Actors request it through
the SDK; external HTTP clients use it with the proxy credentials from the
console. Use it when a target blocks by IP or geography; pair it with the SDK's
session management so a login session reuses the same exit IP.

## Scheduling and chaining

A **schedule** runs an Actor or task on a cron expression. Chaining — running a
second Actor when the first finishes — is done with webhooks: a run emits events,
and a webhook POST triggers the next step. Schedules and webhooks together turn
single runs into pipelines.

## Current vs deprecated

- Prefer the **API v2** and the **current SDKs** over older `apify-client` v1
  patterns. Resolve the current SDK version from the docs or the package
  registry, not memory.
- `run-sync-get-dataset-items` has a response-size ceiling; for large scrapes,
  run async and page the dataset.
- Actor input schemas are the contract for an Actor's input — read a target
  Actor's `input_schema.json` before constructing a run input.

## References

- [Apify Actors](https://docs.apify.com/actors)
- [Apify API reference](https://docs.apify.com/api)
- [Running Actors](https://docs.apify.com/actors/running)
- [Apify CLI](https://docs.apify.com/cli)
- [Storage: datasets](https://docs.apify.com/storage/dataset)
- [Apify Proxy](https://docs.apify.com/proxy)
