# Stripe

![Stripe cover](./assets/readme-cover.png)

Practical guidance for building Stripe integrations without relying on stale API details or
deprecated payment flows.

## Install

```bash
npx skills add hardgraph/skills --skill stripe
```

## Use this skill for

- Checkout and Payment Element architecture
- Payment Intents, subscriptions, invoices, and webhooks
- Connect integrations and Accounts v2
- Payment-method selection and dynamic payment methods
- Current SDK versions, API versions, and migration decisions

## What is included

- [`SKILL.md`](./SKILL.md) contains the agent procedure and integration guardrails.
- [`references/vendor/llms.txt/`](./references/vendor/llms.txt/) contains the mirrored Stripe
  documentation used for exact API and product details.

The skill directs agents to verify details that drift instead of guessing version numbers, event
names, request fields, or product behavior.

## Source

Reference material is reproduced from [Stripe documentation](https://docs.stripe.com/).
