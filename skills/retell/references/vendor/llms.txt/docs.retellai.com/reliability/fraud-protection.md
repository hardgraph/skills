> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Fraud Protection

> Protect your Retell deployment with rate limiting by IP and destination number, geographic restrictions, and public key fraud protection for frontend traffic.

## Overview

Retell provides fraud protection features to help you prevent abuse of your voice AI agents. These features complement the general [abuse prevention measures](/reliability/prevent-abuse) and give you fine-grained control over how your agents are accessed.

## Rate Limiting

When using [public keys](/accounts/public-keys) to authenticate calls from your frontend, you can enable fraud protection to automatically rate limit requests based on IP address and destination phone number.

### Enabling Fraud Protection

You can enable fraud protection when creating or updating a public key:

1. Navigate to **Public Keys** in your Retell dashboard
2. Click on the public key you want to configure
3. Toggle on **Fraud Protection**
4. Save your changes

<Frame caption="Enabling fraud protection on a public key">
  <img src="https://mintcdn.com/retellai/pRKmmGF-LulYX2bs/images/fraud-protection/fraud-protection-toggle.png?fit=max&auto=format&n=pRKmmGF-LulYX2bs&q=85&s=df0f110890156299c47077ff1207c335" alt="Public key settings with the fraud protection toggle enabled" width="609" height="496" data-path="images/fraud-protection/fraud-protection-toggle.png" />
</Frame>

### How It Works

When fraud protection is enabled on a public key:

* Requests are rate limited based on the combination of the caller's IP address and the destination phone number
* This prevents bad actors from using the same IP to spam calls to premium rate numbers
* The rate limiting applies to outbound phone calls and SMS initiated via public key authentication

<Tip>
  For maximum protection, combine fraud protection with [Google reCAPTCHA](/accounts/public-keys#google-recaptcha-v3-protection-optional) to prevent bot abuse.
</Tip>

## Geographic Restrictions

You can restrict which countries are allowed to make inbound calls to your Retell phone numbers, and which countries your phone numbers can make outbound calls to. This helps prevent International Revenue Sharing Fraud (IRSF) and limits your exposure to unwanted traffic.

### Allowed Inbound Countries

Restrict which countries can call your Retell phone numbers:

1. Navigate to **Phone Numbers** in your Retell dashboard
2. Click on the phone number you want to configure
3. Under **Allowed Inbound Countries**, add the countries that should be allowed to call this number

Changes are saved automatically.

<Frame caption="Configuring allowed inbound countries">
  <img src="https://mintcdn.com/retellai/pRKmmGF-LulYX2bs/images/fraud-protection/allowed-inbound-countries.png?fit=max&auto=format&n=pRKmmGF-LulYX2bs&q=85&s=2e231be6a69c5e03247ff328def88510" alt="Phone number settings showing the allowed inbound countries list" width="1886" height="541" data-path="images/fraud-protection/allowed-inbound-countries.png" />
</Frame>

When configured, calls from countries not on the list will be automatically rejected.

### Allowed Outbound Countries

Restrict which countries your phone numbers can call:

1. Navigate to **Phone Numbers** in your Retell dashboard
2. Click on the phone number you want to configure
3. Under **Allowed Outbound Countries**, add the countries this number should be allowed to call

Changes are saved automatically.

<Frame caption="Configuring allowed outbound countries">
  <img src="https://mintcdn.com/retellai/pRKmmGF-LulYX2bs/images/fraud-protection/allowed-outbound-countries.png?fit=max&auto=format&n=pRKmmGF-LulYX2bs&q=85&s=aa7d730f94b5b4d8693d4d11261970ef" alt="Phone number settings showing the allowed outbound countries list" width="1879" height="514" data-path="images/fraud-protection/allowed-outbound-countries.png" />
</Frame>

When configured, outbound calls to countries not on the list will be blocked.

### Configuring via API

You can also configure geographic restrictions via the [Update Phone Number API](/api-references/update-phone-number):

```bash theme={"dark"}
curl -X PATCH "https://api.retellai.com/update-phone-number/+14155551234" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "allowed_inbound_country_list": ["US", "CA", "GB"],
    "allowed_outbound_country_list": ["US", "CA"]
  }'
```

Use [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country codes (e.g., "US" for United States, "CA" for Canada, "GB" for United Kingdom).

To remove restrictions, set the list to `null`:

```bash theme={"dark"}
curl -X PATCH "https://api.retellai.com/update-phone-number/+14155551234" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "allowed_inbound_country_list": null,
    "allowed_outbound_country_list": null
  }'
```

### Sanctioned Countries

The following countries are always blocked regardless of your configuration:

| Country     | Code |
| ----------- | ---- |
| Cuba        | CU   |
| Iran        | IR   |
| North Korea | KP   |
| Syria       | SY   |
| Russia      | RU   |
| Belarus     | BY   |
| Venezuela   | VE   |

Calls to or from these countries will be automatically rejected.

## Best Practices

1. **Enable fraud protection on all public keys** - This adds an extra layer of protection against abuse at minimal cost
2. **Combine with reCAPTCHA** - Use both fraud protection and reCAPTCHA for web-initiated calls to prevent bot abuse
3. **Start with restrictive country lists** - Begin with only the countries you need and expand as necessary
4. **Monitor for blocked calls** - Use [webhooks](/features/webhook-overview) to track when calls are blocked due to geographic restrictions
5. **Review regularly** - Periodically review your country restrictions to ensure they match your current business needs
