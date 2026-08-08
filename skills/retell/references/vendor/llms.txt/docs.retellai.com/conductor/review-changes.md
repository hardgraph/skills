> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Review & apply changes

> Review each change Conductor proposes to your Retell agent and accept or reject it. Nothing is applied until you approve — you stay fully in control.

When you ask Conductor to change something, it doesn't edit your agent directly. Instead, it shows you a **proposal** — a clear list of the changes it wants to make. You review them and decide what to apply.

<Frame>
  <img src="https://mintcdn.com/retellai/3a75AO214qaM767u/images/conductor/conductor-change-proposal.png?fit=max&auto=format&n=3a75AO214qaM767u&q=85&s=777a042e06ad267d4a30cae28058872e" alt="A change proposal card from Conductor" width="742" height="654" data-path="images/conductor/conductor-change-proposal.png" />
</Frame>

## Reading a proposal

Each proposal lists the individual changes Conductor wants to make to your agent — for example, an edit to a step, a new setting, or a reworded message. Each item shows what's being changed so you can tell at a glance whether it's what you wanted.

## See exactly what's changing

Conductor doesn't just tell you what it changed — it shows you, right where the change happens. You never have to guess.

### Before-and-after preview on the setting itself

Each proposed change appears **directly on the affected setting or step**, so you can see it in context. By default, the setting shows the **new (proposed)** value, with a sparkle marker indicating it's a Conductor suggestion.

Use the **view toggle** on the change to flip between:

* **View incoming changes** — what Conductor wants the value to be.
* **View current** — what the value is right now.

This makes it easy to compare the old and new side by side before you decide.

<Frame>
  <img src="https://mintcdn.com/retellai/3a75AO214qaM767u/images/conductor/conductor-change-toggle.png?fit=max&auto=format&n=3a75AO214qaM767u&q=85&s=7263894432b63dc00ea53826b0ca910f" alt="Toggling between the current and incoming value on a setting" width="694" height="1026" data-path="images/conductor/conductor-change-toggle.png" />
</Frame>

### Move through changes one at a time

When a proposal touches several settings, each change shows a counter like **1 / 3**. As you move between changes, Conductor **scrolls the affected setting into view and briefly highlights it**, so you always know exactly what you're looking at — even if it's on another part of the agent.

<Frame>
  <img src="https://mintcdn.com/retellai/3a75AO214qaM767u/images/conductor/conductor-change-counter.png?fit=max&auto=format&n=3a75AO214qaM767u&q=85&s=d473d028a4969d87ee4d8fae48d20388" alt="A change with a 1 of 3 counter highlighted in the builder" width="670" height="184" data-path="images/conductor/conductor-change-counter.png" />
</Frame>

### Detailed (JSON) diff

For more complex values, you can open the **JSON diff** to see a precise line-by-line comparison:

* **Removed** content is shown struck-through.
* **Added** content is highlighted in green.

This is useful when a change affects structured settings and you want to confirm the exact difference.

<Frame>
  <img src="https://mintcdn.com/retellai/3a75AO214qaM767u/images/conductor/conductor-json-diff.png?fit=max&auto=format&n=3a75AO214qaM767u&q=85&s=8c6c70d48af987bd567406b88960343e" alt="A JSON diff showing removed and added lines for a change" width="652" height="312" data-path="images/conductor/conductor-json-diff.png" />
</Frame>

## Accept or reject

You can decide on changes **one at a time** or **all at once**:

* **Accept** (✓) — Conductor applies that change to your agent. You'll see it reflected in the builder right away.
* **Reject** (✗) — that change is discarded and the setting stays exactly as it was.

Working through changes individually is helpful when you like some suggestions but not others.

### Applied, reverted, and undo

After you decide, each change shows its outcome:

* **Change applied** — the new value is now in your agent.
* **Reverted to original** — the change was rejected and nothing was altered.

Changed your mind? Use **Undo** on a decided change to flip your choice before you move on.

<Frame>
  <img src="https://mintcdn.com/retellai/3a75AO214qaM767u/images/conductor/conductor-change-applied.png?fit=max&auto=format&n=3a75AO214qaM767u&q=85&s=c2fbc41fac290b76ea55b82d4b52bad5" alt="A change showing the applied state with an undo button" width="746" height="300" data-path="images/conductor/conductor-change-applied.png" />
</Frame>

<Note>
  If some changes can't be applied, Conductor lets you know and reviews the failed items before continuing — so your agent never ends up in a half-finished state.
</Note>

## Finish reviewing before sending a new message

While a proposal is waiting for your decision, the message box is locked and shows:

> *Accept or reject all changes before prompting Conductor.*

This makes sure you and Conductor are always looking at the same version of your agent. Once you've accepted or rejected the proposal, you can keep chatting.

<Frame>
  <img src="https://mintcdn.com/retellai/3a75AO214qaM767u/images/conductor/conductor-review-locked.png?fit=max&auto=format&n=3a75AO214qaM767u&q=85&s=678edcfaf1c6f256375ecbb86573dd0a" alt="The message box locked until changes are reviewed" width="670" height="948" data-path="images/conductor/conductor-review-locked.png" />
</Frame>

## Proposals for a different agent

If you open a proposal that was created for a **different** agent, Conductor won't let you apply it on the wrong agent. You'll see a message with a **Go to that agent** button — click it to open the correct agent in a new tab, where you can review and apply the changes safely.

<Frame>
  <img src="https://mintcdn.com/retellai/3a75AO214qaM767u/images/conductor/conductor-different-agent.png?fit=max&auto=format&n=3a75AO214qaM767u&q=85&s=1fc023e13743414b84ae872e535cf53f" alt="Notice that a proposal belongs to a different agent" width="690" height="226" data-path="images/conductor/conductor-different-agent.png" />
</Frame>

## Tips

* **Review before accepting.** Skim each change so you know what's being applied.
* **Reject freely.** If a suggestion isn't right, reject it and tell Conductor what to do differently.
* **Test after big changes.** Use [testing tools](/conductor/test-and-improve) to confirm your agent behaves the way you expect.
