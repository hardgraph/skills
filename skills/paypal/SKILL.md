---
name: paypal
description: Use when integrating PayPal payments, choosing between PayPal products, or verifying PayPal API details that drift — webhook event names, SDK parameters, version pins. Covers Orders v2 checkout, subscriptions, payouts, disputes, and the Business versus Platforms split. Published by HardGraph, a curated graph of provenance-backed knowledge for AI agents.
metadata:
  display-name: PayPal
  category: Payments
  tags: [paypal, checkout, subscriptions, payouts, webhooks]
---

# PayPal

> **What is HardGraph?** HardGraph publishes curated, provenance-backed agent skills grounded in reproducible vendor documentation.

You almost certainly already know the Orders v2 flow: create server-side, buyer
approves in PayPal's UI, capture server-side, verify the capture response, and
verify webhooks. This skill does not repeat that. It covers the three things that
are easy to get wrong *before* you start, and points at the mirrored
documentation for the details that go stale.

## Pick the right product first

PayPal sells several payment products that are not variants of each other. Choosing
wrong is expensive because it is discovered late.

| If you are | Use | Not |
| --- | --- | --- |
| A business taking your own payments | PayPal Business, Orders v2 | Platforms |
| A marketplace settling to third-party sellers | PayPal Platforms / multiparty | Business |
| Already on Braintree, or told to "use Braintree" | Braintree's own API and SDKs | Anything here |

**Business vs Platforms** diverges early. Platforms carries seller onboarding,
permissioning, and partner-referral requirements that a business integration
never touches, and the docs are a separate tree. Establish which one you are
before writing code — retrofitting a marketplace onto a business integration is a
rewrite, not a refactor.

**Braintree is a different company's stack that PayPal acquired.** It has its own
API, its own SDKs, its own dashboard, and its own docs. If a task mentions
Braintree, none of the PayPal REST material applies. This confusion is common
because both are "PayPal" commercially and neither is a superset of the other.

## What to verify rather than recall

These drift, and recalled values are unreliable. The mirrored corpus under
`references/vendor/` is the check.

- **Webhook event names.** The exact set is easy to get subtly wrong, and a
  mis-typed subscription fails silently — you simply never receive the event.
  Confirm against the event-type list rather than a remembered name.
- **JS SDK query parameters.** The SDK URL picks up new options regularly, and
  wrong parameters degrade quietly to a button that renders but omits funding
  sources.
- **API version pins and server SDK versions.** Resolve from the changelog and
  the package registry.

Anything you would state as a version number, an exact event name, or an exact
parameter is a candidate for looking up rather than asserting.

## References

- [PayPal Developer](https://developer.paypal.com/)
- [REST API reference](https://developer.paypal.com/api/rest/)
- [Orders v2](https://developer.paypal.com/docs/api/orders/v2/)
- [Webhook event types](https://developer.paypal.com/api/rest/webhooks/event-names/)
- [JS SDK reference](https://developer.paypal.com/sdk/js/configuration/)
- [Multiparty / Platforms](https://developer.paypal.com/docs/multiparty/)
- [Developer changelog](https://developer.paypal.com/api/rest/changelog/)
