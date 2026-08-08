---
name: retell
description: Realtime conversational voice AI platform. Use when building an AI voice agent that speaks in real time — configuring a Retell agent (LLM + transcription + TTS), wiring Twilio/Vonage/SIP telephony or WebRTC web calls, handling function-calling tools and interruption/barge-in, using the realtime WebSocket protocol or SDKs, embedding web calls, or reading call analytics — across Retell Studio and the Retell API.
license: proprietary
metadata:
  display-name: Retell AI
  category: AI and voice
  tags: [voice-agents, conversational-ai, realtime, tts, telephony, webrtc]
---

# Retell AI

Retell is a platform for building **realtime conversational voice agents** — programs that hold a
spoken conversation on a phone call or in a browser. Unlike a one-shot TTS pipeline, a voice agent
must listen, think, speak, and let the caller interrupt, all within hundreds of milliseconds.
Retell runs that loop: it streams audio, transcribes it, calls an LLM, synthesises speech, and
manages turn-taking, while you configure the brain and connect the transport.

## Mental model

A Retell **agent** is a configuration, not a server you host: which LLM reasons, which transcription
model listens, which TTS speaks, what the system prompt and voice are, and which tools it can call.
A **call** is a live session that runs that agent over a transport — telephony (PSTN via Twilio,
Vonage, SIP) or a WebRTC web call. Retell owns the realtime media loop; you own the agent definition
and the integration with your telephony number or web UI.

The two integration shapes:

- **Retell-managed agent**: configure the LLM, prompt, TTS, and tools in Retell (Studio or API).
  Retell calls the LLM for you. Fastest to ship; the agent state lives in Retell.
- **Bring-your-own-LLM (custom LLM)**: Retell handles audio, transcription, and TTS, but calls a
  webhook you host for the reasoning step. You keep full control of the LLM, context, RAG, and tool
  execution. Use this when the agent's brain must read your systems or use a model Retell does not
  host.

## Telephony calls

For phone calls, Retell does not itself own phone numbers in most regions; it bridges to a carrier.
The common path is **Twilio**: you buy a Twilio number, point its incoming webhook at your backend,
your backend calls Retell's `register-call` endpoint to get a `call_id`, then returns TwiML that
`<Connect>`s to Retell using that `call_id`. Retell then owns the media for that leg. The same
pattern applies to Vonage and raw SIP — the carrier handles the PSTN leg, Retell handles the agent
leg, and your server ties the `call_id` to the carrier's call.

The `register-call` step is the seam: it tells Retell "an agent call is starting, here is its id and
metadata," and returns the token TwiML needs. Getting this round-trip slow (or returning TwiML
before registration completes) is the most common reason a call connects but plays silence.

## Web calls (WebRTC)

For in-browser voice, embed Retell's Web SDK (or build against the WebRTC adapter). The browser
establishes a WebRTC session with Retell directly — no phone number, no TwiML. This is the path for
web-based chat assistants, kiosks, and any agent reached from a page rather than a dialpad.

## The realtime loop and latency

A conversation feels natural only under ~800 ms total turn latency. The budget is spent across:
transcription (streaming partial results), LLM time-to-first-token, TTS first-audio, and network.
Retell pipelines these — it starts synthesising as the LLM streams, and plays the first audio chunk
before the full response is generated. The levers you control that move the budget:

- **streaming transcription + streaming LLM + streaming TTS** — never wait for a complete transcript
  before reasoning, nor a complete LLM response before speaking.
- **interruption / barge-in** — when the caller speaks mid-response, Retell stops TTS playback. The
  sensitivity threshold is a tuning parameter: too high and the agent talks over people; too low and
  its own audio echos trigger false interruptions.
- **shorter system context** — a long context window slows first-token and can degrade the model's
  instruction-following mid-call.

## Function calling and tools

Agents can call tools — "check order status," "transfer the call," "book a slot" — via function
calling. With a Retell-managed LLM, declare tools in the agent config and Retell invokes them. With
a custom LLM, you implement the tool loop yourself in the webhook and emit the right
`function_call` / `function_response` events on the realtime channel. Tools that block (a slow DB
query) stall the conversation — return a holding behaviour or pre-fetch when you can.

## Retell Studio vs API

**Retell Studio** is the web UI for creating agents, picking models, testing calls in-browser, and
reading analytics. **The API** does the same programmatically and is what CI and dynamic agent
creation use. Production setups usually define a base agent in Studio and create per-call variants
or retrieve analytics through the API.

## What you must build vs what Retell provides

| You build                                   | Retell provides                              |
| ------------------------------------------- | -------------------------------------------- |
| Telephony webhook + TwiML/SIP glue          | The realtime media loop and turn-taking      |
| Custom LLM endpoint (optional)              | Streaming transcription and TTS             |
| Tools / function handlers + your data       | Agent config, WebRTC, call orchestration     |
| Phone numbers (via Twilio/Vonage)           | Call recording, transcription, analytics     |

## Current vs mutable

Voice platforms change their API and SDK surfaces frequently. Treat these as mutable and resolve
from the live documentation rather than memory:

- **`register-call` request/response shape** and the TwiML `<Connect>` parameters — the exact fields
  and the agent/call id handling evolve.
- **Web / mobile SDK APIs** — method names and the WebRTC adapter surface change across versions.
- **Custom-LLM webhook event protocol** — the realtime event names and the function-call flow are
  revised between releases.
- **Supported model list** — available transcription, LLM, and TTS providers change over time.

For an exact endpoint body, an SDK method, or a webhook event name, read the live Retell
documentation rather than recalling a version.

## References

- [Retell documentation home](https://docs.retellai.com/)
- [Make a phone call (Twilio)](https://docs.retellai.com/examples/twilio-phone-call)
- [Make a web call](https://docs.retellai.com/examples/make-web-call)
- [Custom LLM integration](https://docs.retellai.com/examples/custom-llm)
- [Function calling / tools](https://docs.retellai.com/examples/function-calling)
- [Retell API reference](https://docs.retellai.com/reference/api-reference)
- [Web SDK](https://docs.retellai.com/sdk/web-sdk)
