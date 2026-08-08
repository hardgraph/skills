> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Public Keys

> How Retell public keys authenticate the Chat Widget from your frontend, what they can and cannot access, and how to safely embed them in client-side code.

## Overview

Public keys are specifically designed for authenticating the Retell Chat Widget when embedded on your website. Unlike API keys, which should never be exposed in client-side code, public keys are safe to include in frontend applications for this specific purpose.

Public keys are used exclusively for:

* Embedding the [Retell Chat Widget](/deploy/chat-widget) on your website

<Frame>
  <img height="300" src="https://mintcdn.com/retellai/5qvaMB42_hTUyXzb/images/public_key.png?fit=max&auto=format&n=5qvaMB42_hTUyXzb&q=85&s=b5c68c9f869cecd66c05947e8ab4524a" alt="Public key management page" data-path="images/public_key.png" />
</Frame>

## Allowed Domains

For security reasons, public keys are restricted to specific domains. This prevents unauthorized use of your public key on other websites.

To configure allowed domains:

1. Navigate to the **Public Keys** section in your Retell dashboard
2. Click on the public key you want to configure
3. Add the domains where your public key can be used (e.g., `example.com`, `app.example.com`)
4. Save your changes

<Note>
  **Testing on localhost**

  To test your integration locally, add `localhost` to your allowed domains list. This enables development and testing on your local machine before deploying to production.
</Note>

## Google reCAPTCHA v3 Protection (Optional)

You can optionally enable Google reCAPTCHA v3 protection for your public key to prevent abuse when using the Retell Chat Widget. When enabled, the chat widget will require reCAPTCHA verification before initiating conversations.

To enable reCAPTCHA:

1. Navigate to the **Public Keys** section in your Retell dashboard
2. Click on the public key you want to configure
3. Toggle on **Abuse Prevention (Google reCAPTCHA)**
4. Add your reCAPTCHA Secret Key (obtain from [Google's reCAPTCHA page](https://www.google.com/recaptcha))
5. Adjust the Score Threshold (default: 0.5)
   * Lower scores are more likely to be bots
   * Higher thresholds may block more real users
6. Save your changes

<Note>
  When reCAPTCHA is enabled for a public key, you must also implement reCAPTCHA on your frontend. See [Google's reCAPTCHA documentation](https://developers.google.com/recaptcha/docs/v3) for implementation details.
</Note>

## Security Considerations

While public keys are specifically designed for use with the Retell Chat Widget in client-side code, you should still follow these best practices:

* Only add domains you control to the allowed domains list
* Regularly review your allowed domains to ensure they're up-to-date
* Use the most restrictive domain settings possible for your use case
* For server-to-server communication, use [API keys](/accounts/api-keys-overview) instead

## Managing Public Keys

You can create, view, and manage your public keys from the Retell dashboard:

1. Navigate to the **Public Keys** section
2. Create a new public key or select an existing one to configure
3. Set up allowed domains as needed
4. Copy the public key to use with the Retell Chat Widget on your website
