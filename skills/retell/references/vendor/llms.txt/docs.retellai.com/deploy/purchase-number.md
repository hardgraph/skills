> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Purchase phone number

> Purchase Retell-managed US or Canada phone numbers directly in the dashboard — no telephony provider account required, ready for inbound and outbound calling.

Purchase a phone number from Retell directly in the dashboard or through the API. Retell manages these numbers, so you don't have to set up any telephony infrastructure.

<Note>Currently we only support purchase of US and Canada numbers and support making calls to [15 countries](/deploy/international-call). If you are looking to use numbers from other countries, or to make calls to more countries, or to use your own telephony provider, check out [Custom Telephony guide](/deploy/custom-telephony).</Note>

### From the dashboard

You can purchase a number and bind agents to it from the dashboard. You can optionally specify the
area codes you want to purchase from.

<img height="200" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/deploy/purchase-number/purchase-new.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=37af1861e983af1260f2ecc0fec5337d" data-path="images/deploy/purchase-number/purchase-new.png" />

After purchasing, you can change the number's nickname to make it easier to find and identify.

<img height="200" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/deploy/purchase-number/rename.jpeg?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=2d38919aecddc341584dff635e13d6a1" data-path="images/deploy/purchase-number/rename.jpeg" />

The number is ready to accept [inbound calls](/deploy/inbound-call) once you've assigned an inbound agent. Try calling it.

### From the API

Check out [Create Phone Number API Reference](/api-references/create-phone-number)
for all the parameters you can use programmatically.

* Phone numbers are yours once purchased, and can be used indefinitely.
  Find numbers you own [here](/api-references/list-phone-numbers).
* You can assign different inbound and outbound agents to the number.
* If you don't want a user to be able to call this number (maybe you are doing [outbound calls](/deploy/outbound-call) and don't
  want callbacks), you can leave `inbound_agent_id` unset.

<CodeGroup>
  ```typescript Node theme={"dark"}
  const phoneNumberResponse = await retell.phoneNumber.create({
    inbound_agent_id: "oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD", // replace with the agent id you want to assign
    outbound_agent_id: "oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD", // replace with the agent id you want to assign
  });

  console.log(phoneNumberResponse);
  ```

  ```python Python theme={"dark"}
  # Purchase a phone number
  phone_number = client.phone_number.create(
    inbound_agent_id="oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD", # replace with the agent id you want to assign
    outbound_agent_id="oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD", # replace with the agent id you want to assign
  )
  print(phone_number)
  ```
</CodeGroup>

### Pricing

We support both **Twilio** and **Telnyx** numbers:

* **Twilio**
  * US numbers: **\$2/month**
  * US toll-free numbers: **\$5/month**
  * Canadian numbers: **\$2/month**
* **Telnyx**
  * US numbers only: **\$2/month**

<Note>Toll-free numbers cost \$0.06 per minute for inbound calls. Outbound calls are charged at the same rate as regular U.S. numbers.</Note>

The monthly number fee is billed to your payment method at the end of each billing cycle (prorated if purchased mid-month on credit-based accounts) and recurs until the number is released. See the [billing overview](/accounts/billing) for how number charges appear on your invoice.

## FAQ

<AccordionGroup>
  <Accordion title="Can I use a phone number I already own?">
    Yes. Connect your own telephony provider through [custom telephony](/deploy/custom-telephony), or [import an existing number](/api-references/import-phone-number) into Retell.
  </Accordion>

  <Accordion title="Can I buy a number outside the US and Canada?">
    Not directly. Retell-managed numbers are US and Canada only. To use numbers from other countries, connect your own provider via [custom telephony](/deploy/custom-telephony). Retell-managed numbers can still place calls to [15 countries](/deploy/international-call).
  </Accordion>

  <Accordion title="How do I release a number I no longer need?">
    Delete it from the phone number page in the dashboard, or call the [Delete Phone Number API](/api-references/delete-phone-number). The monthly fee stops once the number is released.
  </Accordion>
</AccordionGroup>
