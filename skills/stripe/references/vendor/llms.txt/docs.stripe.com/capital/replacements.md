# Replacements

Learn how to handle financing offer replacements.

> Capital for platforms is available in [public preview](https://docs.stripe.com/release-phases.md).

Replacement financing offers can be created in the following situations:

- **Pre-submission upgrade**: A connected account links third-party data and receives improved offer terms before submitting the application.
- **Post-submission counteroffer**: Stripe declines the original offer application and extends a new offer with different terms.

The `replacement_type` field indicates which type of replacement Stripe created.

The following examples show how a connected account might receive a replacement offer.

| Replacement type | Scenario |
| --- | --- |
| pre_submission_upgrade | 1. Stripe creates an undelivered financing offer for the connected account with a maximum financing amount of 5,000 USD.
2. Your platform sends the connected account an offer email and marks the offer as `delivered`.
3. The connected account navigates to the Capital financing application and links their bank account.
4. Stripe determines the connected account is eligible for a new maximum financing amount of 10,000 USD.
5. Stripe replaces the connected account’s original 5,000 USD offer with a new 10,000 USD offer. |
| post_submission_counteroffer | 1. Stripe creates an undelivered financing offer for the connected account with a maximum financing amount of 5,000 USD.
2. Your platform sends the connected account an offer email and marks the offer as `delivered`.
3. The connected account navigates to the Capital financing application and submits their application.
4. Stripe conducts a review and determines the connected account is eligible for a revised financing amount of 4,000 USD.
5. Stripe creates a replacement offer of 4,000 USD, rejects the original offer, and sends the connected account a revised offer email.
6. The connected account navigates to the Capital financing application and submits their application.
7. The application is approved and the 4,000 USD is disbursed. |

## Before you begin

- This guide assumes you completed an [API integration](https://docs.stripe.com/capital/api-integration.md).
- Financing offer replacements aren’t enabled by default. After you update your integration to support replacements, you must submit details about your integration and API use to Stripe for compliance review using the [Change Request Form](https://form.asana.com/?k=8K51UWmWhttehNFD5qBLdg&d=974470123217835).

## Create a test offer

1. [Create a sandbox](https://docs.stripe.com/sandboxes/dashboard/manage.md#create-a-sandbox).

2. Go to the [Capital](https://dashboard.stripe.com/test/connect/capital) page in the Dashboard.

3. To generate a test offer, click **Create** and select the parameters for the offer creation.

   - For the connected account, select an existing connected account by searching for the account’s ID or leave it blank and Stripe will generate an account for you.
   - You can select the type of offer and the test account. You can also select the offer terms (amount, fee, and payment rate).
   - View account details of your newly created connected account: `https://dashboard.stripe.com/test/connect/accounts/:merchant_id`.

4. If you’re testing `pre_submission_upgrade`, set the offer status to `delivered`. If you’re testing `post_submission_counteroffer`, set the offer status to `accepted`.

5. Click **Create Financing Offer** to create the offer for the test connected account.

In the Dashboard, you can view each financing offer for your connected accounts in the [Financing offers](https://dashboard.stripe.com/test/connect/capital/financing_offers) page.

## Replace the offer

1. Click the overflow menu (⋯) next to the created offer, and select **Replace offer**.

   > **Replace offer** only appears for offers that have never been replaced and have a `status` of `undelivered`, `delivered`, or `accepted`.

2. Click **Replace** at the bottom of the modal to replace the offer.

   Stripe sends the `capital.financing_offer.replacement_created` webhook after a replacement [financing offer](https://docs.stripe.com/api/capital/connect_financing_object.md) is created. The body of the webhook contains details about the replacement financing offer.

   ```json
   {
       "type": "capital.financing_offer.replacement_created",
       "data": {
           "object": {
               "id": "financingoffer_xyz456",
               "object": "capital.financing_offer",
               "account": "acct_efg678",
               "status": "delivered",
               "financing_type": "flex_loan",
               "offered_terms": {
                   "currency": "usd",
                   "advance_amount": 100000,
                   "fee_amount": 10000,
                   "withhold_rate": 0.15
               },
               "replacement_for": "financingoffer_abc123",
               "replacement_type": "pre_submission_upgrade",
               ...
           }
       }
   }
   ```

   Use `replacement_type` to determine whether the replacement is a pre-submission upgrade or a post-submission counteroffer. The replacement offer has a `delivered` status. When a connected account receives a replacement offer, it becomes the current active offer.

3. Update your *webhook* (A webhook is a real-time push notification sent to your application as a JSON payload through HTTPS requests) integration to handle the `capital.financing_offer.replacement_created` webhook. If your internal data models store the connected account’s active financing offer ID, make sure you update the ID to the connected account’s replacement offer.

## Retrieve the replaced offer

After replacing the offer, you’re redirected to a page with details about the replacement offer. The events table contains an event **Account acct\_egg678 has a replacement financing offer for financingoffer\_abc123**, providing a reference to the user’s original offer ID.

[Retrieve](https://docs.stripe.com/api/capital/financing_offers/retrieve.md#retrieve_financing_offer) the `financingoffer_abc123` financing offer. The status is a terminal state, and the `replacement` attribute has the value `financingoffer_xyz456`, which indicates that `financingoffer_xyz456` replaced `financingoffer_abc123`.

The original offer’s terminal status depends on the replacement type:

- For a pre-submission replacement, Stripe replaces the offer before the account submits an application, so the original offer’s status becomes `replaced`.
- For a post-submission counteroffer, the account has already submitted an application, and Stripe responds by offering new terms. In that case, the original offer reaches its normal review outcome, `rejected`.

## See also

- [Set up an API integration](https://docs.stripe.com/capital/api-integration.md)
