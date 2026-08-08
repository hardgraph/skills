> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Write a single prompt

> Write a Retell single prompt agent using sectional prompts and clear instructions so the agent reliably follows your script across the full phone conversation.

Prompt Engineering might be the most important part of your agent, it can make or break your agent.
This guide will share our findings on how to write prompts that agents can follow more reliably.

This guide is a work in progress and will be updated as we learn more.
If you have any suggestions or feedback, please let us know.

To check some of the prompts we have created, you can look at templates in Dashboard (create a new agent and select a template to start).

### Sectional Prompts

When writing prompts, it is important to break down the system prompts into smaller sections, where each section has its
focus, like identity, style, guideline, task & goals. This has a couple of benefits:

* reusable
* easier to maintain
* easier for LLM to understand

```
## Identity
You are a friendly AI assistant for Retell AI. ....

## Style Guardrails
Be concise: ...
Be conversational: ...
...

## Response Guideline
Return dates in their spoken forms: ...
Ask up to one question at a time: ...
...

## Task
1. Greet the user
...

```

### Write Task as Steps

During a call, if you want the agent to follow a specific procedure to lead the conversation, we recommend writing the task as steps.
This will help LLM understand what to ask at each step and how to proceed. You can have some logic in the steps as well. This also
ensures agent does not pack all questions in one go.

```
## Task
1. Ask for user's name.
2. Ask if user needs a refund, need a replacement, or just retrieving information.
  - if user needs a refund, transition to refund state.
  - if user needs a replacement, transition to replacement state.
3. If user is just retrieving information, ask for the order number.
...

```

If you noticed that the agent is still not stopping at the right time, or you need to go through some steps and not stop
at each step, you can write it like the following
by adding `wait for user response` to be more explicit:

```
## Task
1. Inform user why you are calling.
2. Ask for user's name.

wait for user response

3. Ask if user needs a refund, need a replacement, or just retrieving information.

wait for user response
  - if user needs a refund, transition to refund state.
  - if user needs a replacement, transition to replacement state.


4. Ask for the order number.

wait for user response
...

```

### Prompt Engineer Video Guidance

<iframe width="360" height="200" src="https://www.youtube.com/embed/UREnD6v7fKE?si=khhpR-MPMR690L-H" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen />

See community templates in [docs](https://docs.google.com/document/d/1hx6hdTEjAR4y4xXZ7RLMH2byQNVW1ABxC8S4FwvTx_Y/edit?tab=t.0#heading=h.wf5bktkelope)
