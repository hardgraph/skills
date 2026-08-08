# Replacement cards

Learn how to replace cards that are expired, damaged, lost, or stolen.

You can replace cards that are expired, damaged, lost, or stolen. The process differs slightly for each kind of card replacement.

- **Card expired**: The card has reached its expiration date and is no longer valid.
- **Card damaged**: The cardholder requests a new card for a reason other than lost or stolen (for example, a physical card’s chip no longer reads properly).
- **Card lost/stolen**: The card is reported lost or stolen and a new card number, expiry, security code are issued.

Depending on the scenario, the replacement card might have a different card number, expiry, or security code from the original:

| Scenario | New card number | New security code | New expiry |
| --- | --- | --- | --- |
| **Card expired** | No | Yes | Yes |
| **Card damaged** | No | Yes | Yes |
| **Card lost/stolen** | Yes | Yes | Yes |

## Replacements for expired or damaged cards

Physical cards can get damaged, and both physical cards and virtual cards expire, but you can create replacement cards that have the same card number. The cardholder can continue to use the original card before the replacement card is activated, as long as the card isn’t too damaged or already expired. Activating the replacement card cancels the original card if it isn’t already canceled.

> Stripe doesn’t automatically replace expiring cards. You’re responsible for identifying cards that are approaching their expiration date, canceling them, and creating replacement cards.

You can use the [Dashboard](https://dashboard.stripe.com/issuing/cards) or the [Create a card](https://docs.stripe.com/api/issuing/cards.md) endpoint to replace an expired or damaged card.

## Index

This article has multiple variants. Fetch one of the following URLs to view specific content for your use case:

- [Dashboard](https://docs.stripe.com/issuing/cards/replacements.md?testing-method=without-code)
- [API](https://docs.stripe.com/issuing/cards/replacements.md?testing-method=with-code)

## Replacements for lost or stolen cards

Lost or stolen cards get new card numbers for security reasons. We need to cancel the original cards before we can create the replacement card.

## Index

This article has multiple variants. Fetch one of the following URLs to view specific content for your use case:

- [Dashboard](https://docs.stripe.com/issuing/cards/replacements.md?testing-method=without-code)
- [API](https://docs.stripe.com/issuing/cards/replacements.md?testing-method=with-code)

## All replacements

All replacement cards have renewed expiration dates and new security codes. Authorizations made on the original cards are migrated to the replacements, but might still clear on the original cards. Like the originals, replacement cards must be activated before use.

## Card-on-file updating

For many of our card programs, Stripe automatically updates the card details on file with acquiring merchants, even when a card is completely reissued. This feature offers several benefits, including saving your cardholders the hassle of manually re-entering card details when their cards expire.

### Card expired or damaged

Updating the payment details for a card that has been replaced due to expiration or damage ensures that recurring payments and stored payment details continue to function. This enables cardholders to continue making payments when they replace a card.

### Card lost or stolen

Stripe doesn’t update businesses with the new card number, expiry, and security code of a replacement card if the old card is marked as being lost or stolen.
