---
name: revenuecat
description: Cross-platform subscription and in-app-purchase platform. Use when implementing paid subscriptions or one-time purchases — configuring RevenueCat products, offerings, and entitlements, integrating the Purchases SDK on iOS/Android/web/Flutter/React Native/Unity, checking entitlement status via CustomerInfo, receiving server webhooks to grant access, restoring purchases, handling StoreKit 2 and Google Play Billing, sandbox testing, or managing anonymous vs identified app user IDs — across the App Store, Play Store, Amazon, and Stripe.
metadata:
  display-name: RevenueCat
  category: Payments and subscriptions
  tags: [in-app-purchase, subscriptions, entitlements, ios, android, mobile, stripe]
---

# RevenueCat

RevenueCat is a layer over the platform billing systems — Apple App Store / StoreKit, Google Play
Billing, Amazon Appstore, and Stripe — that gives you one SDK and one server API for subscriptions
and in-app purchases across every platform. The reason it exists is that each store has its own
product model, receipt format, validation rules, and lifecycle edge cases, and stitching them into a
single "does this user have access?" answer is the bulk of the work. RevenueCat owns that
normalisation; you describe what you sell once and ask for entitlements in one way.

## Mental model

Four nouns carry the whole system:

- **Products** are the store-level SKUs you sell (a monthly plan, a yearly plan, a coin pack). They
  are configured per-store in App Store Connect / Play Console and referenced in RevenueCat.
- **Offerings** are the *presentational* grouping: which products you show on a paywall right now,
  and in what order. Swapping an offering changes the paywall without a store review.
- **Entitlements** are the *unlock*: the named capability ("pro", "premium") a purchase grants. Many
  products can grant the same entitlement; your app checks entitlements, never products.
- **CustomerInfo** is the per-user answer object: which entitlements are active, when they expire,
  the latest purchase. This is what your feature gates read.

The flow: you fetch the current **Offering** to render a paywall → the user buys a **Product** → the
SDK reports the purchase to RevenueCat → RevenueCat validates it against the store and updates the
user's **Entitlements** → `CustomerInfo` reflects the new access → your app unlocks the feature.

```swift
let offerings = try await Purchases.shared.offerings()
let package = offerings.current?.availablePackages.first
try await Purchases.shared.purchase(package: package)
let info = try await Purchases.shared.customerInfo()
if info.entitlements["pro"]?.isActive == true { /* unlock */ }
```

## Identity: anonymous vs identified users

RevenueCat assigns every install an **anonymous app user ID** immediately, so a purchase works before
any login. When the user signs in, you call `logIn(yourAppUserId)` to attach your account; RevenueCat
merges the anonymous history into the identified user. Getting this ordering wrong — calling `logIn`
before restoring, or generating your own ID for anonymous state — is the most common cause of
"purchase lost" support tickets. Let RevenueCat own the anonymous ID; call `logIn` exactly once at the
moment you have a real account.

Never hardcode or guess app user IDs. If you need server-side trust, use **offered signed requests
/ delegated authentication** so RevenueCat validates the user against your backend rather than trusting
a client-supplied ID.

## Entitlement checks and the source of truth

Gate features on `CustomerInfo.entitlements["<id>"].isActive`, and treat **RevenueCat as the source of
truth**, not the device's local receipt. A device receipt can be forged or stale; RevenueCat has
already validated against the store. For server-authoritative access (a backend that must not trust the
app), do not ship the entitlement check in the client at all — receive the **webhook** server-side and
grant access from your database.

## Webhooks and the server API

RevenueCat posts **webhooks** on every subscription event: initial purchase, renewal, cancellation,
refund, grace-period entry, expiration, billing issues. The webhook is signed; verify the signature
before trusting it. Use it to update your backend's record of what the user can access. The REST API
(`/v1/subscribers/{app_user_id}`) lets the server query entitlements and the management URL for
Customer Center / web cancellation.

Two classic traps:
- Webhooks can arrive **out of order** or **more than once**. Key on the event id and reconcile
  against the expiration date, do not toggle access from event ordering alone.
- A "billing issue" event means renewal *failed* but the user is often in a grace period and still has
  access; do not revoke until "expiration" (or "non_renewing_purchase" + past date).

## Store platforms

| Store                | What RevenueCat wraps                          |
| -------------------- | ---------------------------------------------- |
| Apple App Store      | StoreKit 2 (and legacy StoreKit receipts)      |
| Google Play          | Play Billing Library (linked purchases, PRS)   |
| Amazon Appstore      | Amazon IAP                                      |
| Stripe (web)         | Stripe-backed subscriptions for web/credit-card |

Receipt validation, sandbox vs production, family-sharing, and offer codes are all handled by
RevenueCat's backend; the SDK hands it the platform purchase and gets back normalised entitlements.

## Restore purchases

"Restore" re-associates a device with the user's prior purchases — essential on reinstall or a new
device. Call `restorePurchases()`, then read `CustomerInfo`. Restore only finds purchases tied to the
store account signed in on the device; a user on the wrong store account will appear to have lost
their subscription until they sign in to the correct one.

## Testing

- **Apple**: StoreKit Configuration files in Xcode let you test purchases fully locally, and
  StoreKit Testing in App Store Connect drives sandbox without a real device. RevenueCat supports
  both; configure sandbox API keys separately.
- **Google**: Play Console license-test accounts and the Play Billing test tracks.
- **RevenueCat**: a separate sandbox/preview project key keeps test purchases out of production
  analytics and revenue.

Test the lifecycle, not just the happy path: renewal, refund, cancellation, and billing failure are
where the entitlement logic actually gets exercised.

## Current vs mutable

Store billing and RevenueCat's SDK surface change regularly. Treat these as mutable and resolve from
the live documentation rather than memory:

- **Purchases SDK API per platform** — method names, async/await shapes, and observer-mode vs full
  SDK responsibilities change across major versions (e.g. StoreKit 2 migration).
- **Offering / entitlement configuration fields** — the dashboard and the object model evolve.
- **Webhook event types and payload fields** — event names and the `store`/`expiration_at` fields are
  extended between releases.
- **REST API versions** — `/v1` vs `/v2` endpoints and auth header shape (`Authorization: Bearer …`
  vs `X-Platform`) differ.
- **Supported platforms and SDKs** — the React Native / Flutter / Unity / web adapter versions move
  independently of the native SDKs.

For an exact SDK method, a webhook payload field, or an API path, read the live RevenueCat
documentation rather than recalling a version.

## References

- [RevenueCat documentation home](https://www.revenuecat.com/docs/getting-started)
- [Entitlements](https://www.revenuecat.com/docs/dashboard/entitlements)
- [Offerings](https://www.revenuecat.com/docs/dashboard/offerings)
- [Making purchases (iOS)](https://www.revenuecat.com/docs/making-purchases)
- [CustomerInfo and entitlements](https://www.revenuecat.com/docs/customer-info)
- [Restoring purchases](https://www.revenuecat.com/docs/restoring-purchases)
- [Webhooks](https://www.revenuecat.com/docs/integrations/webhooks)
- [REST API](https://www.revenuecat.com/docs/api-v1)
- [User identity](https://www.revenuecat.com/docs/user-ids)
- [Testing](https://www.revenuecat.com/docs/test-purchases)
