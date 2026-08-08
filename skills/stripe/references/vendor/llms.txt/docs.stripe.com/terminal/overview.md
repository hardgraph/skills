# How Terminal works

Learn the core concepts behind Stripe Terminal.

With [Stripe Terminal](https://docs.stripe.com/terminal.md), you can accept in-person payments and manage them alongside your online payments in Stripe. You can use Terminal with [no-code options](https://docs.stripe.com/no-code/in-person.md), a custom point-of-sale (POS) integration, or supported third-party POS and gateway integrations.

Terminal includes supported readers and Tap to Pay devices, Dashboard tools for ordering and managing readers, country-specific payment method support, and default encryption for in-person payments. Learn about Terminal [availability](https://docs.stripe.com/terminal/payments/collect-card-payment/supported-card-brands.md) and [readers](https://docs.stripe.com/terminal/payments/setup-reader.md).

## Terminal setup 

A Terminal setup usually includes:

- A Stripe account
- A physical location where you accept payments, either a fixed location, or a corporate address for on the go payments
- A supported reader or Tap to Pay device
- A way to start the payment, such as a no-code flow, a custom POS integration, Tap to Pay, or a supported partner integration
- Dashboard tools for ordering, registering, configuring, and monitoring readers

The exact setup depends on how you want to accept payments. You can use no-code options, build a custom integration with Terminal SDKs or Stripe APIs, accept contactless payments with Tap to Pay, or use a supported third-party POS or gateway.



## Core concepts 

Use Terminal to take the complexity out of in-person payments:

- **Unified payments**: Manage online and in-person payments in a single Stripe account.
- **Readers and devices**: Choose from supported [readers](https://docs.stripe.com/terminal/payments/setup-reader.md) and Tap to Pay devices based on your checkout environment.
- **Locations**: Register readers to [locations](https://docs.stripe.com/terminal/fleet/locations-and-zones.md) so Stripe can apply the right regional and payment method settings.
- **Payment methods**: Accept supported cards, wallets, and selected local payment methods depending on your country, reader, and integration type.
- **Reader management**: Order, register, configure, and monitor readers from the Dashboard.
- **Platforms**: Use Terminal with [Connect](https://docs.stripe.com/connect.md) to support connected accounts.
- **Security**: Terminal encrypts in-person payment data by default. Some businesses can also use the Stripe [P2PE solution](https://support.stripe.com/questions/stripe-terminal-encryption-e2ee-vs-p2pe).

## Ways to use Terminal 

- **Use no-code options**: Accept payments without building a custom POS integration.
- **Build a custom POS integration**: Use Terminal SDKs or Stripe APIs to integrate in-person payments into your own POS, mobile app, or web app.
- **Use Tap to Pay**: Accept contactless payments on compatible iPhone or Android devices.
- **Use a third-party POS or gateway**: Connect Stripe with supported partner POS, hardware, or gateway integrations.
- **Use Terminal with Connect**: Enable in-person payments for your [Connect](https://docs.stripe.com/connect.md) platform or marketplace, with readers for each connected account.

## Security and encryption 

Businesses using Terminal for in-person payments can choose between two levels of encryption. All payments using Terminal are securely encrypted using end-to-end encryption (E2EE) by default. Businesses in certain industries, such as healthcare and education, can also use the PCI-audited point-to-point encryption (P2PE) solution from Stripe.

Stripe P2PE is an optional, paid feature. See [pricing](https://stripe.com/pricing) for more information. If you’re interested in P2PE, [contact your sales representative](https://stripe.com/contact/sales) to discuss whether P2PE is a good fit for your business.

P2PE standards, which are developed by the PCI Council, add an additional decryption step through hardware security modules (HSM) before Stripe sends payment data to card networks. Our P2PE solution simplifies PCI compliance and reduces PCI audit scope and audit costs. It’s validated by a third party, ensuring compliance with PCI P2PE standards, and doesn’t require any additional integration to get started.

If you’ve enabled P2PE, consult the [P2PE Instruction Manual (PIM)](https://docs.stripecdn.com/Stripe_PCI-P2PE-PIM_v2.3.pdf) for detailed guidance. This manual provides instructions on how to properly implement our validated P2PE solution.

## See also

- [Terminal overview](https://docs.stripe.com/terminal.md)
- [Availability](https://docs.stripe.com/terminal/payments/collect-card-payment/supported-card-brands.md)
- [Readers](https://docs.stripe.com/terminal/payments/setup-reader.md)
- [No-code options](https://docs.stripe.com/no-code/in-person.md)
- [Design an integration](https://docs.stripe.com/terminal/designing-integration.md)
