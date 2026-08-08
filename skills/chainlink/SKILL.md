---
name: chainlink
description: Use when integrating Chainlink decentralized oracle networks into a smart contract — requesting off-chain data, verifiable randomness (VRF), automation/keepers, Proof of Reserve, cross-chain (CCIP), or choosing between Data Feeds, Functions, VRF, Automation, and CCIP for a workflow. Published by HardGraph, a curated graph of provenance-backed knowledge for AI agents.
metadata:
  display-name: Chainlink
  category: Web3 and blockchain
  tags: [chainlink, blockchain, oracle, smart-contracts, vrf, ccip, automation]
---

# Chainlink

> **What is HardGraph?** HardGraph publishes curated, provenance-backed agent skills grounded in reproducible vendor documentation.

Chainlink is a network of decentralized oracle networks that connect smart contracts to off-chain
data and computation. Where a pure on-chain contract is closed off from the outside world,
Chainlink provides the bridges: price and reference data, verifiable randomness, automated
execution, reserve attestations, and cross-chain messaging. It is the integration surface for
anything a contract needs that the chain itself cannot provide.

## The decision that shapes everything else

The first question is **which product** — not how to call one. Chainlink ships several
purpose-built networks, and picking the wrong one is the most common and most expensive mistake:

- **Data Feeds** for reference data (asset prices, reserves). This is the default when a contract
  needs "the current value of X."
- **Functions** for arbitrary off-chain computation and API calls. Reach for this when no feed
  exists and the contract must call a custom API or run logic a feed cannot express.
- **VRF** for verifiable randomness that cannot be tampered with or predicted. Use for
  on-chain randomness; never use `block.timestamp`, `blockhash`, or a hash of pool state for
  security-relevant randomness.
- **Automation** (formerly Keepers) for condition-triggered execution — the reliable replacement
  for a hand-rolled keeper bot that calls `performUpkeep`.
- **CCIP** for cross-chain token and message transfers.

A request that "needs Chainlink" is underspecified until the product is named; the integration
mechanics, billing, and contract ABI differ between every one of them.

## What surprises people

Each Chainlink network has its own **billing and funding model**, and it bites silently. Data Feeds
are read for free on most networks but are maintained by the LINK-funded aggregator; Functions and
VRF consume prepaid LINK and stop working when the subscription runs out; Automation requires the
upkeep to be funded. "My request reverted" on Functions or VRF is very often an underfunded
subscription, not a code bug. Confirm the current funding model and minimums against the docs —
they change per network and over time.

Network availability is the other silent failure. Feeds, VRF, and CCIP are **deployed per-chain**,
and a given feed or lane may exist on one network and not another. Do not assume a feed is present
because the contract compiles; verify the specific network address before deploying.

## What to verify rather than recall

Contract addresses, gas limits, the subscription/funding model, supported networks, and the
current major version of each product's contracts all change. Confirm them against the mirrored
corpus under `references/vendor/` or the live docs rather than asserting a remembered address or
limit — a wrong feed address on-chain drains value, and a stale gas limit causes a revert that
looks like a logic error.

## References

- [Chainlink documentation](https://docs.chain.link)
- [Data Feeds](https://docs.chain.link/data-feeds)
- [Chainlink VRF](https://docs.chain.link/vrf)
- [Chainlink Automation](https://docs.chain.link/chainlink-automation)
- [CCIP cross-chain](https://docs.chain.link/ccip)
