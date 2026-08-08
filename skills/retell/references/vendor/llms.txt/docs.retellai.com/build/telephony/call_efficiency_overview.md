> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Spam likely overview

> Why phone numbers get marked "Spam Likely" by carriers and how Retell verified phone numbers and branded call services raise pickup rates on outbound calls.

<div className="flex flex-row gap-16">
  <Frame caption="Spam Likely">
    <img style={{ height: '300px', width: 'auto' }} src="https://mintcdn.com/retellai/_FxJdxv7mQoPqyGs/images/vp.png?fit=max&auto=format&n=_FxJdxv7mQoPqyGs&q=85&s=deb425180734b50c28afc089f080c0ae" alt="Spam Likely" width="800" height="1386" data-path="images/vp.png" />
  </Frame>

  <Frame caption="Branded Call">
    <img style={{ height: '300px', width: 'auto' }} src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/b_1.PNG?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=0eab2bc57f4ac8831f0ab29267cfc421" alt="Apply for branded call" width="1290" height="2796" data-path="images/b_1.PNG" />
  </Frame>
</div>

Phone carriers may mark certain phone numbers as "Spam Likely". This lowers your outbound call pickup rate, and can sometimes lead telephony providers to ban your number or account.

## Check whether your number is marked as spam likely

Check your outbound call log for calls that failed to connect with SIP error code 608. That code means the call was rejected for being marked as spam likely. Not every spam-likely rejection returns error code 608, though.

A few external services can help you check whether your number is marked as spam likely. Results aren't exact, since each carrier has its own rules and database, but you can use them as a reference:

* [Nomorobo](https://www.nomorobo.com/): use the app or API to check your number against its database.
* [IPQualityScore](https://www.ipqualityscore.com/phone-number-validator): check your phone number's reputation score.

## Actions you can take

Retell provides two services to improve your call pickup rate:

1. **[Verified phone number](/build/telephony/verified-phone)**: Retell will register your number with the phone carrier, which will not be marked as spam by carriers.
2. **[Branded call](/build/telephony/branded-call)**: Callee will see your number as your business name, instead of the generic Retell number.

Both services require a verified [business profile](/build/telephony/business-profile) first. If an application is rejected, see [how to handle a rejection](/build/telephony/branded-call-rejection). To stop a service later, see [stop caller ID service](/build/telephony/delete-service).

## iOS 26 call screening

iPhones on iOS 26 may deploy a [call screening feature](https://support.apple.com/guide/iphone/screen-and-block-calls-iphe4b3f7823/ios). At the start of the call, Siri asks unknown numbers to provide their name and reason for calling.
To pass this screen, update your agent prompt to respond clearly. For example:

```
## FAQ
Q: Hi, if you record your name and reason for calling, I'll see if this person is available
A: In one sentence, introduce yourself and explain why you are calling. <wait for response>
```

<div className="flex flex-row gap-16">
  <Frame caption="iOS Call Screen">
    <img style={{ height: '500px', borderRadius: '16px' }} src="https://mintcdn.com/retellai/nRxhsVoMBWNZU20m/images/iOS_call_screen.png?fit=max&auto=format&n=nRxhsVoMBWNZU20m&q=85&s=4662d60d9f84b23883335985f4841845" alt="iOS Call Screen" width="1206" height="2466" data-path="images/iOS_call_screen.png" />
  </Frame>
</div>
