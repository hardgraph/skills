> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Transition conditions

> Control when a Retell conversation flow agent moves between nodes: prompt conditions evaluated by the LLM, deterministic equation conditions, and else edges.

Transition conditions decide whether and where the agent moves next. Each edge out of a node pairs a condition with a destination: when the condition is met, the agent transitions to that node. This is where you get the most control over a flow, and where careful testing pays off most.

## Condition types

There are two types of transition conditions:

* **Prompt**: A natural-language condition evaluated by the LLM against the conversation.
* **Equation**: A deterministic comparison over [dynamic variables](/build/dynamic-variables) — no LLM involved.

Example prompt conditions:

* `User said something about booking a meeting`
* `User said something about cancelling a meeting`
* `User claims to be over 18`
* `User said they lived in New York or Los Angeles`

Example equation conditions:

```
- {{user_age}} > 18
- {{current_time}} > 9 AND {{current_time}} < 18
- {{user_location}} == "New York"
- {{user_location}} != "New York"
- "New York, Los Angeles" CONTAINS {{user_location}}
- {{user_age}} < 18 OR {{user_location}} == "New York"
- {{name}} exists
```

<Note>
  Equation conditions can only reference dynamic variables. For information the agent learns during the call, either use a prompt condition or first capture the value with an [extract DV node](/build/conversation-flow/extract-dv-node) and branch on it afterwards.
</Note>

## Special edges

Besides regular condition edges, a node can have:

* **Else edge**: The fallback. It fires when no other condition matches, so the flow never gets stuck. Use it deliberately — on a node with an else edge, any user turn that matches nothing else moves through it.
* **Always edge**: Transitions unconditionally as soon as the user responds, skipping condition evaluation entirely. Useful when the node just needs one reply (e.g. an acknowledgment) before moving on.
* **Skip Response edge**: Created when the node's **Skip Response** setting is on. The node transitions once the agent finishes speaking, without waiting for the user — good for disclaimers or hand-off lines.

## Evaluation order

When a transition check runs, conditions are evaluated in this order:

1. **Always edge** — if the node has one, the agent transitions through it right after the user responds. Nothing else is checked.
2. **Equation conditions** — evaluated deterministically before any prompt condition. Among equation edges, evaluation runs top to bottom and the first condition that evaluates true wins.
3. **Prompt conditions** — if no equation matched, the LLM evaluates all prompt conditions together, alongside any [global node](/build/conversation-flow/global-node) entry conditions, and picks the single best match — or none.
4. **Else edge** — if nothing matched and the node has an else edge, the agent transitions through it.
5. **Stay** — otherwise the agent stays in the current node and keeps the conversation going.

## When transitions are checked

The trigger depends on the node type:

* **Conversation and subagent nodes**: after each user turn — or, with Skip Response on, as soon as the agent finishes speaking.
* **Function, code, MCP, and extract DV nodes**: after the tool result arrives (when waiting for the result).
* **SMS node**: after the SMS succeeds or fails.
* **Call transfer and transfer agent nodes**: after the transfer result comes back (e.g. the transfer failed).
* **Logic split node**: immediately on entry.

When testing in the dashboard (audio or text), the current node is highlighted on the canvas, so you can watch exactly when each transition happens.

## Equation reference

An equation condition holds one or more equations — up to 50 — combined with **ANY** (at least one must be true) or **ALL** (all must be true).

| Operator                   | Compares | Behavior                                                                                                            |
| -------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------- |
| `==`, `!=`                 | Strings  | Exact string match (or not). No numeric coercion — `"18"` and `"18.0"` are different strings.                       |
| `>`, `>=`, `<`, `<=`       | Numbers  | Numeric comparison. If either side is empty or not a number, the equation evaluates to **false** — it never throws. |
| `Contains`, `Not Contains` | Strings  | Whether the left value includes the right value as a substring.                                                     |
| `exists`, `does not exist` | Variable | Whether the dynamic variable has been set. An empty string still counts as set.                                     |

The `exists` check is useful when information may or may not be available before the call:

```
- {{user_email}} exists
- {{user_phone}} does not exist
```

<Frame caption="An equation condition using the exists operator.">
  <img src="https://mintcdn.com/retellai/YTmPqPUaDmLakDGZ/images/cf/equation-exists.png?fit=max&auto=format&n=YTmPqPUaDmLakDGZ&q=85&s=d7e2c521747433b7695323d5ff49b005" alt="Equation editor showing a condition that checks whether a dynamic variable exists" width="1286" height="953" data-path="images/cf/equation-exists.png" />
</Frame>

## Add and edit conditions

<Steps>
  <Step title="Add a condition">
    Click the node, then click the **+** button to add a transition condition. Choose **Prompt** or **Equation**.

    <Frame caption="Adding a transition condition from the node's edge list.">
      <img src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/cf/add-transition-condition.png?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=cbfe8baf022467b5d7098a19bb424388" alt="Node edge list with the + button open, offering a choice between a prompt and an equation transition condition" width="284" height="183" data-path="images/cf/add-transition-condition.png" />
    </Frame>
  </Step>

  <Step title="Write the condition">
    For a prompt condition, type the condition text directly on the edge.

    For an equation condition, the equation editor opens. Click **Add equation** to add rows, the trash icon to delete one, and switch **ANY** to **ALL** to require every equation to be true.

    <Frame caption="The equation editor with ANY/ALL and per-row controls.">
      <img src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/cf/equation-editor.png?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=b8230b5f0933ccdef7b6ef9ef2b6cb0f" alt="Equation editor dialog showing equation rows with left value, operator, right value, an ANY/ALL selector, and add/delete controls" width="384" height="340" data-path="images/cf/equation-editor.png" />
    </Frame>
  </Step>

  <Step title="Reorder equations">
    Drag the six-dot handle on the left of an equation to move it up or down. Order matters: the first equation condition that evaluates true wins.

    <Frame caption="Drag the handle to reorder equations.">
      <img src="https://mintcdn.com/retellai/YiYvhov2NLXI5cg3/images/cf/equation-editor-reorder.png?fit=max&auto=format&n=YiYvhov2NLXI5cg3&q=85&s=0c39e0b98b5c48eb5efe0b7d76a5bf9d" alt="Equation editor with a row being dragged by its six-dot handle to change evaluation order" width="910" height="842" data-path="images/cf/equation-editor-reorder.png" />
    </Frame>
  </Step>
</Steps>

## Write conditions that transition reliably

The LLM sees the current node's instruction while evaluating conditions, but each condition should stand on its own — describe what the user said or the state reached, without leaning on the instruction text:

* `When user indicates they want to book a meeting`
* `User declines the invitation`
* `User responds to question of their age`
* For function nodes, you can reference the result: `CRM lookup returned successful result`

To keep the agent from getting stuck, cover every case you expect at that point in the conversation. General cases (like objection handling) can live on [global nodes](/build/conversation-flow/global-node), so node-level conditions only need to cover what's specific to that step.

For equation conditions, cover every branch the dynamic variables can take. For example, to treat callers from New York and Los Angeles differently when `{{user_location}}` is known before the call:

```
- {{user_location}} == "New York"
- {{user_location}} == "Los Angeles"
```

## Improve transition accuracy

If you observe an incorrect transition:

* Rewrite the condition to be more specific about what the user said.
* Add transition [finetune examples](/build/conversation-flow/finetune-examples) that show the model real transcripts with the correct decision.

## FAQ

<AccordionGroup>
  <Accordion title="The user said something unrelated to every condition — what happens?">
    In order: if a global node's condition matches, the agent transitions there. Otherwise, if the node has an else edge, the agent moves through it. If neither, the agent stays in the current node and responds from its instruction.
  </Accordion>

  <Accordion title="How do I see the transitions for a past call?">
    Open the call transcript in the history tab — it shows each transition with the node names it moved from and to. Name your nodes descriptively so this is easy to read.
  </Accordion>

  <Accordion title="Is there a limit on the number of transition conditions?">
    There's no limit on conditions per node, though each equation condition holds at most 50 equations. Keep the list focused — the more prompt conditions a node has, the harder it is for the LLM to pick the right one.
  </Accordion>

  <Accordion title="The agent transitioned to a node whose condition was never met — why?">
    Check your [global prompt](/build/conversation-flow/global-setting) and node instructions for numbered labels like "PRIORITY 1", "Step 2", or "ATTEMPT 1". When the agent picks the next node, numbers in these lists can be confused with its transition options, so a transition can fire even though its condition was never met. Rename the items to letters ("PRIORITY A") or descriptive names ("New service intent") — the instructions themselves can stay the same.
  </Accordion>
</AccordionGroup>
