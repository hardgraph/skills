> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Testing pricing

> How Retell bills agent testing: text tests (Playground, simulation, batch) cost per message and voice tests (web, phone) cost per minute at production rates.

Testing an agent bills at the **same rates as production**. There's no separate test tier and no free testing sandbox, so a message or a minute spent testing costs what it would on a live call. Plan test usage like any other usage, and check the [Retell pricing page](https://www.retellai.com/pricing) for current rates.

## How testing is billed

| Method                                                                 | Billed                    | Rate                                                                        |
| ---------------------------------------------------------------------- | ------------------------- | --------------------------------------------------------------------------- |
| [LLM Playground](/test/llm-playground) (Manual Chat)                   | Per message               | Chat rate for your agent's model                                            |
| [Simulation testing](/test/llm-simulation-testing) (AI Simulated Chat) | Per message               | Chat rate for your agent's model, plus the model playing the user           |
| [Batch testing](/test/batch-test-simulation)                           | Per message, plus grading | Chat rates above, summed across every case, plus one analysis unit per case |
| [Web call testing](/test/test-web)                                     | Per minute                | Voice rate (no telephony)                                                   |
| [Phone call testing](/test/test-phone)                                 | Per minute                | Voice rate plus telephony                                                   |

## Text testing

The Playground, simulation testing, and batch testing run in text and bill **per message**, at the chat rate for the model that produced each message. Rates range from roughly **$0.001 to $0.05 per message** depending on the model, so a cheaper model keeps iteration inexpensive. See the [pricing page](https://www.retellai.com/pricing) for the per-model rates.

Three things that add up:

* **A simulated run bills two models.** The **LLM setting** you pick applies to the simulated user only. Your agent's replies bill at the model in the agent's own [LLM settings](/build/llm-options), so a cheap simulated user doesn't make a run cheap if the agent runs an expensive model.
* **Batches multiply.** A batch bills every case on every run, with no discount. Running 20 cases three times each bills 60 runs.
* **Grading bills too.** Each case in a batch adds one post-call-analysis unit for the pass or fail verdict, on top of the conversation. Unsaved Playground runs aren't graded, so they don't carry this.

## Voice testing

[Web call testing](/test/test-web) and [phone call testing](/test/test-phone) place a real voice call and bill **per minute**, at the same rate as a production call: voice infrastructure, text-to-speech, and the LLM add up to roughly **$0.07 to $0.31 per minute** depending on the model and voice.

* **Phone testing adds telephony** (about $0.015 per minute for a Retell number) and requires a [phone number](/deploy/purchase-number). Retell numbers cost $2.00 per month.
* **Web testing has no telephony line**, since it runs in the browser.

Because a voice minute costs far more than a text message, run most of your checks in text simulation and reserve web or phone calls for final validation.

## Conductor

[Conductor](/test/testing-with-conductor) is free up to a daily allowance: **30 messages per user and 200 per workspace each day**, reset at midnight Pacific. Past that, Conductor bills **\$0.20 per message**, but only if a workspace admin has turned on pay-as-you-go for Conductor. With it off, Conductor stops answering until the allowance resets. Your account also needs to be in good standing, so an expired trial or a failed payment blocks it either way.

Any simulations Conductor runs on your behalf bill as normal per-message simulation runs. Check <code className="ui-btn">Billing → Usage</code> for the exact amount.

## Free allowances

* **\$10 in free credits** when you sign up.
* **20 free concurrent calls**.
* **Conductor**: 30 messages per user and 200 per workspace each day, reset at midnight Pacific.
* **AI QA**: the first 100 minutes are free, then \$0.10 per minute. See [AI QA](/ai-qa/overview).

Apart from Conductor's daily messages, testing draws on the same balance as production, so treat test usage as billable. One thing the signup credit doesn't cover: buying a [phone number](/deploy/purchase-number) requires a card on file, so [phone call testing](/test/test-phone) needs payment set up even if you still have credit left.

## See also

* [Retell pricing](https://www.retellai.com/pricing) for current per-model and per-minute rates.
* [Testing overview](/test/test-overview) compares the testing methods.
