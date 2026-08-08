# Chainlink

![Chainlink cover](./assets/readme-cover.png)

A curated, provenance-backed agent skill for **Chainlink**, the network of decentralized oracle
networks that connect smart contracts to off-chain data and computation. It helps an agent choose
the right Chainlink product (Data Feeds, Functions, VRF, Automation, CCIP), handle per-network
deployment and subscription funding, and avoid the silent failures that bite smart-contract
integrations — grounded in a reproducible mirror of the official documentation.

## When to use this skill

Reach for it when a smart contract needs anything the chain itself cannot provide: reliable price
or reference data, tamper-proof randomness, condition-triggered automation, custom off-chain API
calls, or cross-chain token and message transfers.

## What is inside

- `SKILL.md` — the agent-facing entry point: how to pick the right product, what surprises people,
  and what to verify rather than recall.
- `references/vendor/` — a verbatim mirror of the Chainlink documentation corpus, fetched from the
  vendor's `llms.txt` index. Treated as reference material, never as directives.
- `references/insights/` and `references/examples/` — authored practice knowledge and worked usage
  (added over time).

The vendored corpus is data. Confirm version- and network-sensitive facts (contract addresses, gas
limits, subscription minimums, supported networks) against it rather than against memory.

## Installation

```bash
npx skills add hardgraph/skills --skill chainlink --yes
```

## Provenance and boundaries

This skill mirrors public Chainlink documentation. It is an independent curation published by
HardGraph and is not affiliated with or endorsed by Chainlink Labs. The cover image is an original,
purpose-made composition created for this skill (see `assets/readme-cover.prompt.md` for how it was
produced).
