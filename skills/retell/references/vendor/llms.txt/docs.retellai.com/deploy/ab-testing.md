> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# A/B Testing

> Run Retell A/B tests by splitting inbound and outbound voice and chat traffic between agents to compare prompts, voices, and flows in production.

A/B testing lets you compare multiple agents or scripts by sending a percentage of traffic to each. You define a traffic split (for example, 80% to one agent and 20% to another) so you can test different prompts, voices, or flows without changing your main agent.

## Where A/B testing is available

A/B testing is supported for:

* **Inbound calls** — Calls received on a phone number
* **Outbound calls** — Calls made from a phone number
* **Inbound chats** — Incoming chat conversations
* **Outbound chats** — Chat sessions you initiate

## How it works

* **Multiple agents**: Bind or assign two or more agents to the same number or chat configuration.
* **Traffic split**: Set the percentage of traffic each agent receives (e.g., 80% / 20%). Traffic is distributed according to this split.
* **Use cases**: Test different scripts on marketing or support calls, try prompt or voice changes, or compare conversation flows in chat.

## Where to configure

**Phone numbers (calls):** On the phone number configuration page, the A/B Testing toggle appears next to the Call Agent field. Turn it on to bind multiple agents and set a traffic split. The screenshot below shows the control with A/B testing disabled.

<Frame caption="A/B Testing control on the phone number configuration page (disabled)">
  <img src="https://mintcdn.com/retellai/OUoENzk-DjeUQ4lb/images/deploy/phone-call/ab-testing-disabled.png?fit=max&auto=format&n=OUoENzk-DjeUQ4lb&q=85&s=25946aa3ee6fa45200c2bde81cefc3cf" alt="Phone number configuration showing Call Agent dropdown and A/B Testing toggle in the off position" width="1024" height="93" data-path="images/deploy/phone-call/ab-testing-disabled.png" />
</Frame>

After you turn the toggle on, you can add one or more agents and set the traffic percentage for each (for example, one agent at 100% or a split such as 80% / 20%).

<Frame caption="A/B Testing enabled: agent variants and traffic split">
  <img src="https://mintcdn.com/retellai/OUoENzk-DjeUQ4lb/images/deploy/phone-call/ab-testing-enabled.png?fit=max&auto=format&n=OUoENzk-DjeUQ4lb&q=85&s=af20d15125759a970ce2cd9a2a6d9fcb" alt="Call Agent section with A/B Testing on, showing one agent variant with 100% traffic and edit option" width="1024" height="96" data-path="images/deploy/phone-call/ab-testing-enabled.png" />
</Frame>

Click **Edit** to open the A/B Testing modal. There you choose which agents to use and set a weight (percentage) for each. You can add multiple agents and remove or adjust them; the weights must total 100%. Click **Deploy** to save.

<Frame caption="A/B Testing modal: configure agents and percentage weights">
  <img src="https://mintcdn.com/retellai/OUoENzk-DjeUQ4lb/images/deploy/phone-call/ab-testing-modal.png?fit=max&auto=format&n=OUoENzk-DjeUQ4lb&q=85&s=5ce22efaa611769f780edd3e86f03303" alt="A/B Testing modal with Agent and Weight columns, two agents at 50% each, Add button and Deploy/Cancel actions" width="1024" height="502" data-path="images/deploy/phone-call/ab-testing-modal.png" />
</Frame>

After you click **Deploy**, the configuration is active. The Call Agent section shows each agent and its traffic percentage (for example, a 50/50 split). You can click **Edit** anytime to change the agents or weights.

<Frame caption="A/B Testing after deploy: active traffic split">
  <img src="https://mintcdn.com/retellai/OUoENzk-DjeUQ4lb/images/deploy/phone-call/ab-testing-deployed.png?fit=max&auto=format&n=OUoENzk-DjeUQ4lb&q=85&s=63c0babd4cedf06ac8d12c075011d290" alt="Call Agent section with A/B Testing on, showing Agent A and Agent B each at 50% with Edit option" width="1024" height="134" data-path="images/deploy/phone-call/ab-testing-deployed.png" />
</Frame>

| Context                       | Configure in                                                                                                                                                                                                           |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Inbound calls**             | Number configuration: bind multiple agents to the number and set the traffic split. See [Receive calls](/deploy/inbound-call).                                                                                         |
| **Outbound calls**            | Number configuration: bind multiple agents for outbound and set the traffic split. See [Outbound calls](/deploy/outbound-call).                                                                                        |
| **Chat (inbound & outbound)** | Chat agent or widget configuration where you assign the chat agent; enable A/B testing and set the traffic split. See [Create Chat Completion](/deploy/create-chat-completion) and [Chat widget](/deploy/chat-widget). |

## View analytics by agent version

After you deploy an A/B test, you can compare performance by specific agent versions in the Analytics dashboard.

1. Open **Analytics** in the Retell dashboard.
2. Set a date range that covers your experiment period.
3. In the filter bar, click **Agent** (or click **Filter** and add **Agent**).
4. In the left panel, select the agent(s) used in your A/B test.
5. In the right panel, select the specific version(s) you want to analyze (for example, `Version 3` vs `Version 4` of the same agent).
6. Click **Save** to apply the filter.

All charts on the page will update to the selected agent-version scope, so you can compare metrics (such as call success rate, duration, and latency) for each variant.

For more details on Analytics, see [Get analytics insight](/features/analytics-dashboard).

## A/B testing vs dynamic agent selection

<Tabs>
  <Tab title="A/B testing">
    Use when you want a **random percentage-based split**. Traffic is distributed according to the percentages you set, with no per-call or per-chat logic.
  </Tab>

  <Tab title="Dynamic selection (webhook / API)">
    Use when you want to **choose the agent per call or per chat** based on caller, context, or other data. For inbound calls, use the [Inbound Call Webhook](/features/inbound-call-webhook). For outbound calls or chats, pass the agent (or other overrides) in the API when you create the call or chat.
  </Tab>
</Tabs>
