# Connect and the Accounts v2 API

Create connected accounts with a unified identity across Stripe.

As a Connect platform, you represent your connected accounts with `Account` objects. There are two versions of `Accounts`:

- **v2**: Highly configurable and extendable support for payments to and by connected accounts, funds transfers, and other functionality
- **v1**: Limited support for connected accounts and payments

v2 `Accounts` use *configurations* (Account configurations represent role-based functionality that you can enable for accounts, such as merchant, customer, or recipient) to enable different sets of functionality. For example, adding the `merchant` configuration to a v2 `Account` enables it to receive payments. If you want to extend an existing connected account’s functionality, you can add more configurations to it.

v1 `Accounts` don’t have configurations. You can only use them to manage payments for connected accounts.

Certain features don’t yet directly support v2 `Accounts`. However, you can still pass the ID of a v2 `Account` to an Accounts v1 API endpoint. When you do so, the response is structured as a v1 `Account`, but any updates still apply to the corresponding properties of the v2 object.

## Use the Accounts v2 API to represent connected accounts

The Accounts v2 API provides:

- **Flexible account configurations**: Enable or change an `Account`’s capabilities and compatibility with Stripe features by updating its configurations. Each capability belongs to a particular configuration.
- **Centralized identity data**: When you add a configuration to an existing `Account` to enable additional functionality, you don’t have to re-collect requirements that they already provided.
- **A single API for connected accounts and customers**: In most cases, you can represent any customer with an `Account` object. That lets you use the Accounts v2 API to manage both connected accounts and customers. You don’t have to use the Customers API at all.

The most common configurations include the following:

- The `merchant` configuration allows an `Account` to accept payments from customers. It includes the `card_payments` and `stripe_balance.payouts` (replacing v1 `payouts`) capabilities.
- The `customer` configuration lets your platform charge an `Account` as a customer. You can use it instead of a `Customer` object when creating a subscription or invoice.
- The `recipient` configuration allows an `Account` to receive funds transfers. It includes the `stripe_balance.stripe_transfers` (replacing v1 `transfers`) capability, which is required to use indirect charges.

The following example creates an `Account` using API v2. Notice that its structure differs from the structure of an `Account` object in API v1.

> #### API v2 response structure
> 
> By default, Accounts v2 API calls return values for certain properties and null for other properties, regardless of their actual values. To retrieve additional property values, request them using [the include parameter](https://docs.stripe.com/api-includable-response-values.md).

```curl
curl -X POST https://api.stripe.com/v2/core/accounts \
  -H "Authorization: Bearer <<YOUR_SECRET_KEY>>" \
  -H "Stripe-Version: 2026-07-29.preview" \
  --json '{
    "contact_email": "jenny.rosen@example.com",
    "display_name": "Jenny Rosen",
    "dashboard": "full",
    "identity": {
        "business_details": {
            "registered_name": "Furever"
        },
        "country": "us",
        "entity_type": "company"
    },
    "configuration": {
        "merchant": {
            "capabilities": {
                "card_payments": {
                    "requested": true
                }
            }
        }
    },
    "defaults": {
        "currency": "usd",
        "responsibilities": {
            "fees_collector": "stripe",
            "losses_collector": "stripe"
        },
        "locales": [
            "en-US"
        ]
    },
    "include": [
        "configuration.merchant",
        "identity",
        "requirements"
    ]
  }'
```

Specifying `requirements` in the `include` array returns any default requirements that apply to the new `Account`.

## Use Accounts as customers

Any API request with a `customer` parameter that accepts a `Customer` ID also has a `customer_account` parameter that accepts an `Account` ID. You can use it to provide the ID of a v2 `Account` that has the `customer` configuration instead of providing the ID of a `Customer`. For example:

```curl
curl https://api.stripe.com/v1/setup_intents \
  -u "<<YOUR_SECRET_KEY>>:" \
  -d customer_account=acct_123 \
  -d "automatic_payment_methods[enabled]=true" \
  -d confirm=true \
  --data-urlencode "return_url=https://example.com/return" \
  -d usage=off_session
```

If you use the Accounts v1 API, you can’t pass an `Account` ID to an endpoint that expects a `Customer` ID. If you want to charge your connected accounts using subscriptions, you have to create and manage separate `Customer` objects for them.

Learn more about [using Accounts as customers](https://docs.stripe.com/accounts-v2/use-accounts-as-customers.md).

## Existing Connect platforms using Accounts v1 and Customers v1

You can [use the Accounts v2 API to manage Accounts created using the Accounts v1 API](https://docs.stripe.com/connect/accounts-v2/migrate-integration.md), including assigning them the `customer` configuration.

Stripe discourages indefinitely maintaining both Accounts API versions simultaneously.

## Accounts API v2 limitations

You must use Accounts v1 in the following cases:

- Using [OAuth to authenticate connected accounts](https://docs.stripe.com/stripe-apps/api-authentication/oauth.md)
- Signing connected accounts to a [recipient service agreement](https://docs.stripe.com/connect/service-agreement-types.md#recipient)
- To request or manage the following capabilities:
  - `treasury`
  - `card_issuing_*`
  - [deprecated capabilities](https://docs.stripe.com/connect/account-capabilities.md?accounts-namespace=v2#deprecated-capabilities) such as `legacy_payments`
  - deprecated payment methods
  - certain payment methods in public or private preview

> #### Cross-border payouts
> 
> Use [Global payouts](https://docs.stripe.com/global-payouts.md) to send cross-border payouts.
