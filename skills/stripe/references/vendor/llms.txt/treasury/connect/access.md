# Get started with API access to Treasury for platforms

Test in a sandbox environment to experiment before going live.

> #### Legacy integration
> 
> The v1 version of Treasury for platforms is a legacy integration that doesn’t support many of the features introduced in [Treasury for platforms v2](https://docs.stripe.com/treasury/connect/v2.md). Don’t build a new v1 integration.

> Stripe Treasury for platforms is available in the United States only.

You can use Treasury for platforms and Issuing in a *sandbox* (A sandbox is an isolated test environment that allows you to test Stripe functionality in your account without affecting your live integration. Use sandboxes to safely experiment with new features and changes) environment to see what functionality you want to enable in your live integration.

## Get test access to Treasury for platforms and Issuing

Enable your Stripe account to request `issuing` and `treasury` (Treasury for platforms) capabilities on connected accounts.

To issue cards to your own company or employees, see the [Issuing documentation](https://docs.stripe.com/issuing.md).

You can only offer Treasury for platforms features [to connected accounts](https://docs.stripe.com/treasury/connect/account-management/accounts-structure.md).

1. Create a [sandbox environment](https://docs.stripe.com/sandboxes/dashboard/manage.md#create-a-sandbox) and switch to it.
2. Enable Issuing and Treasury for platforms on the sandbox in the Dashboard by clicking the activate button below.

[Activate Issuing and Treasury for platforms in a Sandbox](https://dashboard.stripe.com/setup/treasury/activate)

> You must be an [account administrator](https://docs.stripe.com/get-started/account/teams/roles.md) to complete the Treasury for platforms onboarding steps for a platform.

## Start with your sandbox

There are a few ways to start testing the Issuing and Treasury for platforms.

### Test with the Issuing and Treasury for platforms sample application

Use the [Issuing and Treasury for platforms sample application](https://docs.stripe.com/treasury/connect/examples/sample-app.md) to onboard your first test connected account, create a financial account and card, and make test transactions using your newly activated sandbox environment.

### Test from the Dashboard

You must use the API or sample app to create financial accounts and cards linked to financial accounts. After you create a financial account, you can use the Dashboard to view activity, copy routing and account numbers, and move funds from your platform financial account balance into the financial account. After you create a card, you can use the Dashboard to make test authorizations. See [Use the Dashboard for Issuing with connect](https://docs.stripe.com/issuing/connect.md#using-dashboard-issuing).

### Test Treasury for platforms only (without Issuing)

To test Treasury for platforms without Issuing, request the `treasury` capability on a connected account and don’t request `card_issuing`. When you activate Treasury for platforms and Issuing through the link above, it gives your platform the ability to request both capabilities independently.

### Confirm test Issuing and Treasury for platforms are enabled on your sandbox

To confirm you’ve enabled Treasury for platforms and Issuing on your sandbox, click **Connect** > **Stripe Treasury** in the Dashboard to access the [Treasury page](https://dashboard.stripe.com/test/connect/financial-accounts). If you can’t access Treasury, then you haven’t enabled access.

## Configure your account to go live

Enabling Treasury for platforms and Issuing through the link above lets you try out basic functionality in a testing environment. After you’re approved for a [supported business use case](https://support.stripe.com/questions/supported-business-use-cases-for-stripe-issuing), you can configure your live account for live Treasury for platforms and Issuing. Your sandbox environment retains the previously created test financial accounts and cards.

[Speak to sales](https://go.stripe.global/treasury-inquiry) to get approved for a supported business use case, and configure your live account for live Treasury for platforms and Issuing.

> Speak to sales before building a full API integration, because some functionality could change.
