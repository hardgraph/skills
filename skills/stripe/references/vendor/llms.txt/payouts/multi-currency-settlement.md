# Multi-currency settlement

Accept, settle, and pay out funds in multiple currencies.

If you’re a Connect platform looking to offer multi-currency settlement capabilities to your connected accounts, see our [Connect docs](https://docs.stripe.com/connect/multi-currency-settlement.md).

Stripe automatically converts all incoming funds into your default currency. With multi-currency settlement, you can configure your account to accrue balances and get paid out in up to 18 supported currencies to pay suppliers, process refunds, and avoid FX fees. Where supported, you can use [instant currency conversion](https://docs.stripe.com/instant-currency-conversion.md) to convert between currencies.

## Availability

Multi-currency settlement is available in the following locations:

- AE
- AU
- CH
- EU
- GB
- HK
- LI
- NO
- SG
- US

## Enable multi-currency settlement

Add other settlement currencies from your [Balances](https://dashboard.stripe.com/balance/overview) page. You can also configure bank accounts and currencies in your [Payout Settings](https://dashboard.stripe.com/settings/payouts). To accrue a balance in a given currency, you must first accept payments in that currency, known as presentment. 

### Attach bank accounts to receive payouts in local currencies

You must provide a separate supported bank account for each settlement currency you configure to receive payouts in that currency. You must match the currency of the bank account to the settlement currency. After you provide the corresponding bank accounts, Stripe directs payouts from your multi-currency balances to their respective bank accounts in the matching currency.

### Configure payout settings

After you meet a currency’s [minimum payout amount](https://docs.stripe.com/payouts/multi-currency-settlement.md#multicurrency-settlement-fees), payouts follow your configured [payout schedule](https://docs.stripe.com/connect/manage-payout-schedule.md), whether manual or automatic. You can’t pay out until your balance for the currency meets the minimum payout amount.

## Settlement pricing and minimum payout amounts 

Stripe charges the applicable fee when funds settle into a non-primary settlement currency, not when you initiate a payout. The following table shows the pricing, supported bank-account countries, and minimum payout amounts for each settlement currency.
[See table on original page](https://docs.stripe.com/payouts/multi-currency-settlement)
## See also

- [Instant currency conversion](https://docs.stripe.com/instant-currency-conversion.md)
- [Supported currencies](https://docs.stripe.com/currencies.md)
- [Localize prices](https://docs.stripe.com/payments/currencies/localize-prices.md)
