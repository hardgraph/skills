# PayPal

![PayPal cover](./assets/readme-cover.png)

Practical guidance for selecting and implementing the correct PayPal payment product before
integration decisions become expensive to reverse.

## Install

```bash
npx skills add hardgraph/skills --skill paypal
```

## Use this skill for

- Orders v2 checkout and server-side capture
- PayPal Business versus PayPal Platforms decisions
- Marketplace seller onboarding and multiparty payments
- Subscriptions, payouts, disputes, and webhooks
- Current SDK parameters, webhook event names, and version checks

## What is included

- [`SKILL.md`](./SKILL.md) contains the product-selection and integration procedure.
- [`references/vendor/`](./references/vendor/) contains the mirrored PayPal documentation used
  for exact API and product details.

The skill keeps PayPal REST, PayPal Platforms, and Braintree boundaries explicit while directing
agents to verify details that drift.

## Sources

- [PayPal Developer](https://developer.paypal.com/)
- [REST API reference](https://developer.paypal.com/api/rest/)
- [Orders v2](https://developer.paypal.com/docs/api/orders/v2/)
