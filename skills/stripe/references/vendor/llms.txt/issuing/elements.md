# Using Issuing Elements

Learn how to display card details in your web application in a PCI-compliant way.

[Stripe.js](https://docs.stripe.com/js.md) includes a browser-side JavaScript library you can use to display the sensitive data of your Issuing cards on the web in compliance with PCI requirements. The sensitive data renders inside Stripe-hosted iframes and never touches your servers. Stripe.js also collects extra data to protect your users. Learn more about how Stripe collects data for [advanced fraud detection](https://docs.stripe.com/disputes/prevention/advanced-fraud-detection.md).

## Ephemeral key authentication

Stripe.js uses ephemeral keys to securely retrieve Card information from the Stripe API without publicly exposing your secret keys. You need to do some of the ephemeral key exchange on the server-side to set this up.

The ephemeral key creation process begins in the browser, by creating a **nonce** using Stripe.js. A nonce is a single-use token that creates an **ephemeral key**. This nonce is sent to your server, where you exchange it for an ephemeral key by calling the Stripe API (using your secret key).

Create a server-side ephemeral key, then pass it back to the browser for Stripe.js to use.

## Create a secure endpoint [Server-side]

The first step to integrating with Issuing Elements is to create a secure, server-side endpoint to generate ephemeral keys for the card you want to show. Your Issuing Elements web integration calls this endpoint. When creating ephemeral keys, specify an API version of `2020-03-02` or later and include the ephemeral key nonce, which you create in your web integration.

Here’s how you might implement an ephemeral key creation endpoint in web applications framework across various languages:

#### Node.js

```javascript
// This example sets up an endpoint using the Express framework.

const express = require('express');
const bodyParser = require('body-parser');
const app = express();

app.use(bodyParser.urlencoded({ extended: true }));

// Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
const stripe = require('stripe')('<<YOUR_SECRET_KEY>>');

app.post('/ephemeral-keys', async (request, response) => {
  const { card_id, nonce } = request.body;

  /* Important: Authenticate your user here! */

  const ephemeralKey = await stripe.ephemeralKeys.create({
    nonce: nonce,
    issuing_card: card_id,
  }, {
    apiVersion: '2026-07-29.dahlia',
  });

  response.json({
    ephemeralKeySecret: ephemeralKey.secret,
  });
});
```

### Create a secure endpoint with Connect

To create a secure endpoint as a Connect user, add `Stripe-Account` in the header.

#### Node.js

```javascript

const express = require('express');
const bodyParser = require('body-parser');
const app = express();

app.use(bodyParser.urlencoded({ extended: true }));

const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);

app.post('/ephemeral-keys', async (request, response) => {
  const { card_id, nonce } = request.body;

  /* Important: Authenticate your user here! */

  const ephemeralKey = await stripe.ephemeralKeys.create({
    nonce: nonce,
    issuing_card: card_id,
  }, {
    apiVersion: '2026-01-28.clover',
    stripeAccount: connectedAccountToken,
  });

  response.json({
    ephemeralKeySecret: ephemeralKey.secret,
  });
});

```

> Your endpoint is responsible for authenticating that the requesting user has permission to see the requested card’s details. Make sure your endpoint only issues ephemeral keys to users of the requested card.

## Web API integration [Client-side]

First, include Stripe.js on your page. For more information on how to set up Stripe.js, refer to [including Stripe.js.](https://docs.stripe.com/js/including)

Create a `Stripe` instance and an ephemeral key nonce for the card you want to retrieve using [stripe.createEphemeralKeyNonce](https://docs.stripe.com/js/issuing/create_ephemeral_key_nonce). Use the nonce to retrieve the ephemeral key by calling the [server-side endpoint](https://docs.stripe.com/issuing/elements.md#create-secure-endpoint) that you created:

```javascript

const stripe = Stripe('<<YOUR_PUBLISHABLE_KEY>>');


// Initialize Elements which you'll need later
const elements = stripe.elements();

// Use Stripe.js to create a nonce
const cardId = '{{CARD_ID}}';
const nonceResult = await stripe.createEphemeralKeyNonce({
  issuingCard: cardId,
});
const nonce = nonceResult.nonce;

// Call your ephemeral key creation endpoint to fetch the ephemeral key.
// Note that the ephemeral key expires after 15 minutes.
const ephemeralKeyResult = await fetch('/ephemeral-keys', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': /* Important: this endpoint must be authenticated. */
  },
  body: JSON.stringify({
    card_id: cardId,
    nonce: nonce,
  })
});

const ephemeralKeyResponse = await ephemeralKeyResult.json();
const ephemeralKeySecret = ephemeralKeyResponse.ephemeralKeySecret;

// Retrieve card data — must be called before creating Issuing Elements
await stripe.retrieveIssuingCard(cardId, {
  ephemeralKeySecret: ephemeralKeySecret,
  nonce: nonce,
});
```

### Web API integration with Connect

If you use Connect, you must initialize Stripe with beta flags for Issuing Elements.

> #### Feature flag required
> 
> If you want to use Issuing Elements with connected accounts, you need to contact [Stripe Support](https://support.stripe.com) to request access. Otherwise, creating ephemeral keys on behalf of connected accounts fails with authorization errors.

```javascript
// Initialize Stripe with beta flags for Issuing Elements
const stripe = Stripe('<<YOUR_PUBLISHABLE_KEY>>', {
    stripeAccount: '{{CONNECTED_ACCOUNT_ID}}',
    betas: [
        'issuing_add_to_wallet_button_element_1',
        'issuing_elements_2',
    ]
});

// Initialize Elements which you'll need later
const elements = stripe.elements();

// Use Stripe.js to create a nonce
const cardId = '{{CARD_ID}}';
const nonceResult = await stripe.createEphemeralKeyNonce({
  issuingCard: cardId,
});
const nonce = nonceResult.nonce;

// Call your ephemeral key creation endpoint to fetch the ephemeral key.
// Note that the ephemeral key expires after 15 minutes.
const ephemeralKeyResult = await fetch('/ephemeral-keys', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': /* Important: this endpoint must be authenticated. */
  },
  body: JSON.stringify({
    card_id: cardId,
    nonce: nonce,
  })
});

const ephemeralKeyResponse = await ephemeralKeyResult.json();
const ephemeralKeySecret = ephemeralKeyResponse.ephemeralKeySecret;

// When using the issuing_elements_2 beta with Connect, you must call
// retrieveIssuingCard before creating elements. Without this step, element
// creation fails with "Issuing card has not been retrieved." The returned
// cardResult isn't used directly. Elements need the card registered internally
// with Stripe.js to mount.
const cardResult = await stripe.retrieveIssuingCard(cardId, {
  ephemeralKeySecret: ephemeralKeySecret,
  nonce: nonce,
});
```

### Refresh ephemeral key every 15 minutes

Ephemeral keys for Issuing Elements expire after 15 minutes. Call your backend endpoint every 15 minutes during a user session to refresh the ephemeral key and prevent stale authentication.

After regenerating, use the Element’s `update({...})` method to send in a new ephemeral key and nonce.

## Display an Element [Client-side]

Now that you have an ephemeral key and have called `stripe.retrieveIssuingCard()`, you can display an Issuing Element.

All Elements are created with the following pattern:

```javascript
const element = elements.create(elementName, options);
element.mount("#my-parent-container");
```

### Available Issuing Elements

| Element | Name | Availability |
| --- | --- | --- |
| Number (PAN) | `issuingCardNumberDisplay` | Virtual cards only |
| CVC | `issuingCardCvcDisplay` | Virtual cards only |
| Expiry date | `issuingCardExpiryDisplay` | Any card |
| PIN | `issuingCardPinDisplay` | Any card |
| Copy button | `issuingCardCopyButton` | Any card |

> In a sandbox, all card data, including the number and CVC, is returned for any card type, regardless of the restrictions above. In live mode, `issuingCardNumberDisplay` and `issuingCardCvcDisplay` return data for virtual cards only by default. To display the number and CVC for physical cards in live mode, you must request access to the `allow_retrieve_physical_card_details` feature by [contacting Stripe support](https://support.stripe.com/contact).

### Display physical card details

By default, PAN and CVC elements only work with virtual cards. To display these details for physical cards, you need to:

1. **Request access**: Contact [Stripe support](https://support.stripe.com/contact) to request the `allow_retrieve_physical_card_details` feature for your account.
2. **Enable the beta flag**: Initialize Stripe.js with the `issuing_elements_2` beta flag:

```javascript
const stripe = Stripe('<<YOUR_PUBLISHABLE_KEY>>', {
  betas: ['issuing_elements_2'],
});
```

When both requirements are met, `issuingCardNumberDisplay` and `issuingCardCvcDisplay` work for physical cards as they do for virtual cards.

### Usage 

Each element type has different options and functions. Select which element type you want to learn more about:

#### Card Display Element

This section applies to creating elements that display a card’s details: `issuingCardNumberDisplay`, `issuingCardCvcDisplay`, `issuingCardExpiryDisplay`, or `issuingCardPinDisplay`.

### Options

| Name | Type | Usage | Required |
| --- | --- | --- | --- |
| `issuingCard` | `string` | The ID of your issued card (for example, `ic_abc123`) | Yes |
| `nonce` | `string` | Your ephemeral key nonce | Yes |
| `ephemeralKeySecret` | `string` | The `secret` component of your ephemeral key | Yes |
| `style` | [Style object](https://docs.stripe.com/js/appendix/style) | Keep in mind that some variants, pseudo-classes, and properties are for input Elements and won’t apply to these Elements. An example of an input-only pseudo-class is `::placeholder`. | No |

### Example

```javascript
const number = elements.create('issuingCardNumberDisplay', {
  issuingCard: cardId,
  nonce: nonce,
  ephemeralKeySecret: ephemeralKeySecret,
  style: {
    base: {
      color: '#fff',
      fontSize: '16px'
    },
  },
});

number.mount('#card-number');
```

#### Card Copy Button Element

This section applies to displaying the `issuingCardCopyButton`.

This element takes a `toCopy` option and renders a transparent “copy to clipboard” button that takes up the space of its parent `<div>`. This allows it to intercept all click events with a click handler. The click handler takes the corresponding card data specified at initialization and copies it to the clipboard.

With this, you can display “copy to clipboard” buttons next to the card number, expiry, and cvc, which prevents your cardholders from manually copying card data. We restrict the copy functionality to Stripe’s PCI-compliant `<iframe>`.

### Options

| Name | Type | Usage | Required |
| --- | --- | --- | --- |
| `toCopy` | `'expiry'` or `'cvc'` or `'number'` or `'pin'` | The attribute of the card that’s copied to the clipboard. | Yes |
| `style` | [Style object](https://docs.stripe.com/js/appendix/style) | Keep in mind that some variants, pseudo-classes, and properties are for input Elements and won’t apply to these Elements. An example of an input-only pseudo-class is `::placeholder`. | No |

### Example

```javascript
const cardNumber = elements.create('issuingCardNumberDisplay', {
  issuingCard: cardId,
  nonce: nonce,
  ephemeralKeySecret: ephemeralKeySecret,
});

cardNumber.mount('#card-number');

const cardNumberCopy = elements.create('issuingCardCopyButton', {
  toCopy: 'number',
  style: {
    base: {
      fontSize: '12px',
      lineHeight: '24px',
    },
  },
});

cardNumberCopy.mount('#card-number-copy');
```

If you’re having trouble with your button responding to clicks, be sure to line up the iframe to your button correctly. You can customize your image and containing `<div>` in your stylesheets however you want.

```css
#card-number-copy {
  height: 24px;
  width: 24px;
  position: relative;
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  background-image: url('data:image/svg+xml;base64,...');
}
```

As a last step, provide an “after click feedback” option to your users. To do so, use the `issuingCardCopyButton` Element’s [on click event](https://docs.stripe.com/js/element/events/on_click). This could be temporarily showing a new icon as shown below.

```css
#card-number-copy-success {
  display: none;
  height: 24px;
  width: 24px;
  background-image: url('data:image/svg+xml;base64,...');
  background-size: 100%;
}
```

```javascript
// Example of hiding, replacing, and re-showing icons upon click
const timeout = (ms) => {
  return new Promise((resolve) => setTimeout(resolve, ms));
};
const hideAndShowSuccess = (iconElementId, successIconElementId) => {
  const el = document.getElementById(iconElementId);
  el.style.display = 'none';
  const elSuccess = document.getElementById(successIconElementId);
  elSuccess.style.display = 'block';
  timeout(2000).then(() => {
    elSuccess.style.display = 'none';
    el.style.display = 'block';
  });
};

cardNumberCopy.on('click', () => {
  hideAndShowSuccess('card-number-copy', 'card-number-copy-success');
});
```

## Security requirements

If you choose to use `issuingCardPinDisplay`, you must implement measures to ensure that only authorized users can access it. Specifically, you need to apply two-factor authentication (2FA) before granting access to any page that uses `issuingCardPinDisplay`. If Stripe determines that your security measures are inadequate, we might suspend your access to this Element.

> Unlike our mobile SDKs, Issuing Elements *doesn’t* provide an integration with [verifications](https://docs.stripe.com/api/issuing/verifications.md). You must implement two-factor authentication in order to display card PINs through Issuing Elements.

## Additional details

The returned card object has PCI fields (such as the number) fully removed from the `result.issuingCard` payload.

In addition to `.mount()` in the previous examples, the Elements also support the following methods:

- `.destroy()`
- `.unmount()`
- `.update(options)`

## Issuing Elements and native applications

Issuing Elements doesn’t directly support native application platforms such as iOS, Android, or React Native.

To display sensitive card details with Issuing Elements in your native app, use a web view. Build a web integration on your servers following this guide, and then point a web view’s URL to that integration. To learn about implementing web views for native apps, see these external resources:

- iOS and iPadOS: [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview)
- Android: [WebView](https://developer.android.com/reference/android/webkit/WebView)
- React Native: [react-native-webview](https://github.com/react-native-webview/react-native-webview)
- Flutter: [webview-flutter](https://pub.dev/packages/webview_flutter)
