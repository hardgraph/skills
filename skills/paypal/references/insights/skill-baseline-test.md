---
title: What a model already knows about PayPal, measured
---

# What a model already knows about PayPal, measured

Recorded 2026-08-08. This exists so the next person editing `SKILL.md` does not
re-add material that testing showed is redundant or harmful.

## Method

Two agents, same prompt — a realistic "integrate PayPal checkout on a deadline"
request. One with no skill (baseline), one with an earlier draft of `SKILL.md`
that contained an eight-rule "Instructions for AI agents" section plus a prose
walkthrough of auth, the order lifecycle, and webhooks. Neither could search the
web.

## Result

The baseline already produced, unprompted:

- Orders v2, with NVP/SOAP, Express Checkout, and Payments v1 explicitly rejected
  as legacy
- OAuth client-credentials with token caching and the secret kept server-side
- create → approve → capture, with capture as the point money moves
- verifying `status`, amount, and currency against its own database before
  marking an order paid, explicitly to defeat a tampered client
- webhook signature verification, idempotent handlers, sandbox-first testing

Every one of those was a rule in the draft skill. The skill was teaching what the
model already did.

The baseline also produced specifics the skill did **not** contain: the
`breakdown` fields must sum exactly to `amount.value` or the API returns 400;
`INSTRUMENT_DECLINED` should drive `actions.restart()`; `ORDER_ALREADY_CAPTURED`
should be treated as success and reconciled; `PayPal-Request-Id` for idempotency;
`invoice_id` as a duplicate-charge guard; refunds go against the capture ID, not
the order ID; `debug_id` is the only identifier PayPal support acts on.

## The part that matters

Running the same prompt *with* the draft skill produced a **worse** answer. It
gained four things traceable to the skill — the Business/Platforms scope check,
the Braintree warning, "the `onApprove` callback is buyer-controlled", and the
raw-body verification trap — and lost roughly eight of the specifics listed
above.

The eight numbered rules behaved as a checklist. The agent satisfied them and
stopped, instead of reasoning from what it knew.

## Consequences for this skill

1. **Do not restate the integration flow.** It is redundant at best and crowds
   out better recall at worst.
2. **Do not write prohibition-style rule lists** in a reference skill. They shape
   output toward the list. Prohibitions are for discipline failures — cases where
   an agent knows the rule and skips it under pressure. That was not the failure
   here.
3. **Keep only what the baseline lacked**: the product-selection question
   (Business vs Platforms vs Braintree), and the pointer toward details that
   drift.

## A drift signal worth keeping

The two runs subscribed to **different webhook event sets** — six events in one,
four in the other — with no prompt difference to explain it. Exact event names are
something the model is measurably inconsistent about, and a mistyped subscription
fails silently: the event simply never arrives. This is the strongest argument for
the mirrored corpus, and the reason `SKILL.md` sends readers to the event-type
list rather than printing one.
