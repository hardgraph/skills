> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Free messages & pay-as-you-go

> Understand your daily free Conductor messages, turn on pay-as-you-go to keep working after they run out, and set a monthly spending limit for your workspace.

Conductor gives every user a set of free messages each day. If you need to keep going after those run out, a workspace admin can turn on **pay-as-you-go** so Conductor keeps working — billed by the message.

## Your daily free messages

Each user gets **30 free messages per day**. Your free messages reset daily at **midnight PT**.

Your workspace also shares a cap of **200 free messages per day** across all users. Once your workspace's free messages run out, additional usage is billed to your workspace at **\$0.20 per message** — even if you still have free messages of your own left. If pay-as-you-go isn't turned on, Conductor pauses until free messages reset at midnight PT.

You can see how many free messages you have left on the **messages pill** next to the message box in the Conductor panel. Click the pill to open a summary of your usage.

* **When you have free messages left**, the pill shows the number remaining.
* **When your free messages run out**, the pill shows **Daily free limit reached** and Conductor pauses until either your messages reset at midnight PT or pay-as-you-go is turned on.

## Keep going with pay-as-you-go

Turn on pay-as-you-go to keep using Conductor after the daily free messages run out. Additional usage is billed to your workspace at **\$0.20 per message**.

<Note>
  Only workspace admins can turn on pay-as-you-go or change the monthly limit. If you don't see these options, ask an admin for your workspace.
</Note>

### Turn it on from Conductor

<Steps>
  <Step title="Open the messages pill">
    Click the messages pill next to the message box in the Conductor panel.
  </Step>

  <Step title="Enable pay-as-you-go">
    Click **Enable pay-as-you-go**.
  </Step>

  <Step title="Choose a monthly limit">
    In the **Enable pay as you go** dialog, set a **Monthly usage limit**:

    * **Unlimited** — no monthly cap on pay-as-you-go usage.
    * **Fixed** — enter a dollar amount per month (USD). Once your workspace reaches this amount, Conductor pauses until the limit is raised or your free messages reset.
  </Step>

  <Step title="Confirm">
    Click **Enable**. Conductor keeps working once your daily free messages run out.
  </Step>
</Steps>

Once pay-as-you-go is on, the messages pill shows **On**. Click it to see **Usage this month** and the **\$0.20 per message** rate, and to **Adjust limit** at any time.

### Turn it on from Settings

You can also manage pay-as-you-go from the **Limits** settings page:

<Steps>
  <Step title="Open Limits">
    Go to [**Settings → Limits**](https://dashboard.retellai.com/settings/limits).
  </Step>

  <Step title="Find Conductor messages">
    Locate the **Conductor messages** card.
  </Step>

  <Step title="Turn on the toggle">
    Switch the toggle on to open the pay-as-you-go dialog, then choose **Unlimited** or a **Fixed** monthly limit and confirm.
  </Step>
</Steps>

The card shows your current setting — either your **Monthly limit** in USD or **Unlimited** — along with the **\$0.20 per message** rate. Use **Adjust Limit** to change it, or switch the toggle off to turn pay-as-you-go back off.

## When your workspace limit is reached

If you set a **Fixed** monthly limit and your workspace reaches it, the messages pill shows **Workspace limit reached** and Conductor pauses. To keep going, a workspace admin can open the messages pill (or the **Conductor messages** card in Settings) and **Adjust limit** to raise the monthly cap. Otherwise, your free messages reset at midnight PT.

## Frequently asked questions

<AccordionGroup>
  <Accordion title="Do unused messages roll over to the next day?">
    No. Unused messages expire at the end of each day and do not roll over.
  </Accordion>

  <Accordion title="Does a longer conversation use more messages?">
    No. Each message you send counts as one, regardless of conversation length or complexity.
  </Accordion>

  <Accordion title="Who pays for overage messages?">
    Overage charges are billed to the workspace, not individual users.
  </Accordion>

  <Accordion title="Why am I being billed when I still have free messages left?">
    Your workspace shares a cap of 200 free messages per day across all users. Once your workspace's free messages run out, additional usage is billed to the workspace at \$0.20 per message, even if you haven't used all of your own 30 free messages yet.
  </Accordion>

  <Accordion title="Can I continue using Conductor after I run out of free messages?">
    Yes, if your workspace has pay-as-you-go enabled. Otherwise, Conductor pauses until your daily messages reset.
  </Accordion>

  <Accordion title="What happens when the monthly overage spending limit is reached?">
    Conductor stops using paid messages. Users must wait until a workspace admin increases the usage limit or changes it to Unlimited.
  </Accordion>

  <Accordion title="What does Unlimited mean?">
    If an Admin selects Unlimited, there is no monthly cap on paid overage usage. Users can continue using Conductor after their free daily messages are exhausted, and each additional message is billed at \$0.20 per message.
  </Accordion>
</AccordionGroup>
