> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Prevent abuse

> Defend Retell agents against International Revenue Sharing Fraud and other abuse — rate limit traffic, restrict destinations, and lock down public keys.

## Abuse scenarios

Retell AI has implemented several mechanisms to prevent bad actors from using our agents to conduct malicious activities. However, there are cases where bad actors may pretend to be a customer and spam your agents.

Malicious activity usually comes in the form of International Revenue Sharing Fraud (IRSF). Bad actors are incentivized to do so because they get kickbacks from carriers when they direct traffic to them. Common abuse scenarios include:

* making excessive outbound calls, usually to non-US numbers, either via your phone call widget or form submission. They usually rotate the destination phone number, and use a real human recording to avoid being detected.
* making outbound SMS (even 2FA SMS) messages, usually to non-US numbers.
* making a large amount of unwanted inbound calls into a number that you made public. This is less common as it's usually not going to bring them kickbacks.
* using robots to spam your chat widget. This is less common as it's usually not going to bring them kickbacks.

## Abuse prevention

Here are a few high level rules of thumb to prevent abuse. We will dive into more details below:

1. Never expose your API key to the public; always use a [public key](/accounts/public-keys) in frontend.
2. If your API key is exposed, always rotate and revoke the key.
3. Always use a reCAPTCHA if possible to prevent bots from abusing your endpoints.
4. Only allow functionalities or regions that you need.
5. Implement rate limiting (number-based, IP-based, etc.) for your endpoints if necessary.
6. Have user identification mechanisms (KYC measures) in place if necessary.
7. Have a prompt in your agent that can potentially detect unrelated calls and hang up quickly.

### Protecting outbound calling / chatting capabilities

There are two ways that you can secure the calling / chatting capabilities that you expose to the public:

* have your own user access management system, and keep the Retell API calls to your backend only.
* use the [Retell widgets](/deploy/chat-widget) to embed the calling / chatting capabilities into your website. It's highly recommended to enable [reCAPTCHA](/accounts/public-keys#google-recaptcha-v3-protection-optional) to prevent bots from abusing your endpoints.

### Protecting inbound calling

When a number is made public, it's possible to have unwanted traffic. You can set up [inbound webhooks](/features/inbound-call-webhook) to detect and block unwanted traffic based on the incoming number.

### Advanced Protection

For additional fraud protection features including rate limiting by IP/phone number and geographic restrictions per phone number, see [Fraud Protection](/reliability/fraud-protection).
