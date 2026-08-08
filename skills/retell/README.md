# retell

![retell cover](./assets/readme-cover.png)

Reference skill for [Retell AI](https://retellai.com/) — the platform for building realtime
conversational voice agents. It steers an agent through configuring a Retell agent (LLM +
transcription + TTS), wiring Twilio/Vonage/SIP telephony or WebRTC web calls, the
`register-call` + TwiML bridge, bring-your-own-LLM custom reasoning, function-calling tools,
interruption and latency tuning, and call analytics — without relying on stale version recall.

## Install

```bash
npx skills add hardgraph/skills --skill retell
```

## Use this skill for

- Building an AI voice agent that talks on phone calls or in a browser
- Connecting a Retell call to Twilio, Vonage, or SIP
- Choosing a Retell-managed agent vs a bring-your-own-LLM (custom LLM) integration
- Implementing function-calling / tools inside a voice agent
- Tuning interruption, barge-in, and end-to-end call latency
- Embedding a WebRTC web call in a page
- Reading Retell call analytics and recordings

## What is included

- [`SKILL.md`](./SKILL.md) — the Retell mental model, the telephony-bridge flow, and the
  custom-LLM decision.
- [`references/vendor/llms.txt/`](./references/vendor/llms.txt/) — a reproducible verbatim mirror
  of the Retell documentation via its official
  [llms.txt index](https://docs.retellai.com/llms.txt).

## Source

Reference material is reproduced from the
[Retell documentation](https://docs.retellai.com) via its official
[llms.txt index](https://docs.retellai.com/llms.txt).
