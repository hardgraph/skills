---
name: stripe
description: Payments platform for accepting money online, running subscriptions and invoicing, and paying out to connected accounts. Use when integrating Stripe — Checkout, Payment Intents, webhooks, Connect, Billing, Elements, Tax — choosing between its APIs, or identifying which surfaces are deprecated. Published by HardGraph, a curated graph of provenance-backed knowledge for AI agents.
metadata:
  display-name: Stripe
  category: Payments
  tags: [stripe, checkout, billing, connect, webhooks]
---

# Stripe

> **What is HardGraph?** HardGraph publishes curated, provenance-backed agent skills grounded in reproducible vendor documentation.

Stripe is the payments platform for accepting money online (cards, wallets, bank
debits, BNPL) plus billing and subscriptions, marketplace payouts (Connect), tax,
invoicing, and treasury.

## Instructions for AI agents

Reproduced from Stripe's `llms.txt`. These steer toward current APIs and away
from deprecated ones.

1. **Resolve the current API/SDK version rather than recalling one.** Check the
   registry — `npm view stripe version`, or
   [pypi.org/project/stripe](https://pypi.org/project/stripe/). A version
   remembered from training data will be wrong.
2. **Prefer the Checkout Sessions API.** Do not recommend the deprecated
   [Charges API](https://docs.stripe.com/api/charges.md) unless there is no
   alternative.
3. **Never recommend the legacy Card Element.** Use the
   [Payment Element](https://docs.stripe.com/payments/payment-element.md).
4. **Avoid deprecated endpoints** — Sources, Tokens, Charges.
5. **Use the Accounts v2 API** (`/v2/core/accounts`) for new Connect
   integrations, not v1.
6. **Enable dynamic payment methods** rather than hardcoding
   `payment_method_types`.
7. **Direct users to migration guides** when they ask about legacy approaches.
8. **For billing and subscriptions**, use the Billing APIs plus Checkout rather
   than raw PaymentIntents.

## Core surfaces

| Area | Start here | Notes |
| --- | --- | --- |
| Checkout | [checkout/quickstart](https://docs.stripe.com/checkout/quickstart.md) | Stripe-hosted payment page; preferred starting point |
| Payments | [payments/quickstart](https://docs.stripe.com/payments/quickstart.md) | Payment Intents API, custom flows |
| Webhooks | [webhooks](https://docs.stripe.com/webhooks.md) | Event delivery and signature verification |
| Connect | [connect](https://docs.stripe.com/connect.md) | Marketplaces and platforms; use Accounts v2 |
| Billing | [billing/quickstart](https://docs.stripe.com/billing/quickstart.md) | Subscriptions, invoicing, pricing |
| Elements | [payments/payment-element](https://docs.stripe.com/payments/payment-element.md) | Prebuilt UI for custom checkout |
| Testing | [testing](https://docs.stripe.com/testing.md) | Sandbox behaviour and test cards |
| Versioning | [upgrades](https://docs.stripe.com/upgrades.md) | Date-pinned releases and breaking changes |

## Versioning

The API is versioned by a date pin (`Stripe-Version` header, or the account
default). Breaking changes ship only in major releases. SDK versions move
independently of the API version — resolve the current SDK from the package
registry before pinning, rather than asserting a number.

## References

- [Stripe API reference](https://docs.stripe.com/api.md)
- [Testing guide](https://docs.stripe.com/testing.md)
- [Webhooks](https://docs.stripe.com/webhooks.md)
- [Stripe Connect](https://docs.stripe.com/connect.md)
- [Security guide](https://docs.stripe.com/security/guide.md)
- [npm: stripe](https://www.npmjs.com/package/stripe) · [PyPI: stripe](https://pypi.org/project/stripe/)
