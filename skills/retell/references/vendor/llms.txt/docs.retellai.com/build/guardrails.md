> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Guardrails

> Use Retell guardrails to detect prohibited topics in agent responses and user input, replacing flagged content with a safe placeholder so calls continue.

Guardrails are a built-in content moderation layer that checks agent responses and user messages for prohibited topics. When a guardrail triggers, the prohibited content is automatically replaced with a safe placeholder message, keeping the call going without interruption.

## How Guardrails Work

Guardrails apply in two ways:

* **Output guardrails** check what the agent says. If the agent's response contains a prohibited topic, that response is replaced with a placeholder message before being spoken.
* **Input guardrails** check what the user says. If the user's message contains a prohibited topic, the agent responds with a placeholder message instead of processing the request.

In both cases, the call continues normally after the placeholder is delivered. Guardrails do not end the call, transfer the call, or trigger any other action — they only replace the problematic message.

## Configuring Guardrails

You can configure guardrails when creating or updating an agent, either through the dashboard or the API. In the dashboard, guardrail settings are under **Security & Fallback Settings**.

<Info>
  Guardrails add about 50ms of latency to calls.
</Info>

<Frame caption="Guardrail configuration in the dashboard">
  <img src="https://mintcdn.com/retellai/3ssxg22la9RgW0-T/images/guardrail/guardrail_main.png?fit=max&auto=format&n=3ssxg22la9RgW0-T&q=85&s=465219030ccb01e09c977e75deb1e383" alt="Main guardrail settings screen showing output and input topic toggles" width="50%" data-path="images/guardrail/guardrail_main.png" />
</Frame>

### Output Topics

These categories detect prohibited content in agent responses:

| Topic                           | Description                                               |
| ------------------------------- | --------------------------------------------------------- |
| `harassment`                    | Harassing or abusive language                             |
| `self_harm`                     | Content related to self-harm                              |
| `sexual_exploitation`           | Sexually exploitative content                             |
| `violence`                      | Violent content                                           |
| `defense_and_national_security` | Defense and national security topics                      |
| `illicit_and_harmful_activity`  | Illicit or harmful activities                             |
| `gambling`                      | Gambling-related content                                  |
| `regulated_professional_advice` | Regulated professional advice (legal, medical, financial) |
| `child_safety_and_exploitation` | Child safety and exploitation content                     |

<Frame caption="Output guardrail topic options">
  <img src="https://mintcdn.com/retellai/3ssxg22la9RgW0-T/images/guardrail/output_guardrail.png?fit=max&auto=format&n=3ssxg22la9RgW0-T&q=85&s=20e3c058d6985e5e8b11781e741c5084" alt="Output guardrail categories for agent responses" width="50%" data-path="images/guardrail/output_guardrail.png" />
</Frame>

### Input Topics

One input topic is available; it detects attempts to jailbreak or manipulate the agent.

| Topic                             | Description                                   |
| --------------------------------- | --------------------------------------------- |
| `platform_integrity_jailbreaking` | Attempts to jailbreak or manipulate the agent |

<Frame caption="Input guardrail topic options">
  <img src="https://mintcdn.com/retellai/3ssxg22la9RgW0-T/images/guardrail/input_guardrail.png?fit=max&auto=format&n=3ssxg22la9RgW0-T&q=85&s=dd2d3ab589e9baff6c1ff4ada1590ecb" alt="Input guardrail categories for user messages" width="50%" data-path="images/guardrail/input_guardrail.png" />
</Frame>
