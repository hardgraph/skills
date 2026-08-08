> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Make a web call

> Add browser voice calls with the Retell Web SDK: create a web call server-side to get an access token, then start and control it from your frontend.

A web call connects a user to your voice agent directly in the browser, using their microphone and speakers. No phone number is involved: audio streams over the internet through the [Retell Web SDK](https://github.com/RetellAI/retell-client-js-sdk), so there is no telephony setup or per-minute telephony cost.

This guide covers the full setup: create the call on your server, start it in the browser, and handle live call events.

## When to use web calls

* **Voice inside your product.** Add a support agent to your help center, a sales assistant to a landing page, or voice-guided onboarding, with a UI you fully control.
* **Custom call experiences.** The SDK exposes call state, the live transcript, and raw audio, so you can build captions, talking-avatar animations, or your own call controls.
* **Development and testing.** A web call is the fastest way to talk to an agent while building it. For a quick test without writing code, use the dashboard's [web call testing](/test/test-web).

If you want a voice entry point on your website without building a frontend, embed the [website widget](/deploy/chat-widget) instead — it only takes a script tag. To reach users on their phones, see [outbound calls](/deploy/outbound-call) and [inbound calls](/deploy/inbound-call).

For example, an e-commerce site adds a "Talk to support" button to its order page. Clicking it starts a web call that passes the customer's name and order ID as [dynamic variables](/build/dynamic-variables), so the agent greets the customer by name and looks up the right order without asking for it.

## How it works

A web call has two halves:

1. **Your server** calls the [create web call API](/api-references/create-web-call) with your Retell API key and receives an `access_token`.
2. **Your frontend** passes that token to the Web SDK's `startCall()`, which connects the user's microphone and speakers to the agent.

This split protects your API key: the key can create calls and read your account data, so it must never ship in frontend code. The access token is safe to hand to the browser because it grants entry to one call only.

<Warning>
  Start the call within 30 seconds of creating it. After that, the access token
  is invalidated and the call is marked with an error.
</Warning>

## Set up web calls

<Steps>
  <Step title="Install the Web SDK">
    ```bash theme={"dark"}
    npm install retell-client-js-sdk
    ```
  </Step>

  <Step title="Create a server endpoint to get an access token">
    On your server, call [create web call](/api-references/create-web-call) with the `agent_id` of the agent that should answer, and return the `access_token` to your frontend.

    ```javascript server.js theme={"dark"}
    import express from "express";

    const app = express();
    app.use(express.json());

    app.post("/api/create-web-call", async (req, res) => {
      const response = await fetch("https://api.retellai.com/v2/create-web-call", {
        method: "POST",
        headers: {
          Authorization: `Bearer ${process.env.RETELL_API_KEY}`,
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          agent_id: "agent_oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD",
          // Optional: inject per-call values into the agent's prompt
          retell_llm_dynamic_variables: { customer_name: "John Doe" },
          // Optional: attach your own IDs to the call for later lookup
          metadata: { internal_customer_id: "cust_123" },
        }),
      });

      if (!response.ok) {
        return res.status(500).json({ error: "Failed to create web call" });
      }

      const call = await response.json();
      res.json({ accessToken: call.access_token, callId: call.call_id });
    });

    app.listen(8080);
    ```

    `agent_id` is the only required field. The optional fields you'll reach for most:

    | Field                          | What it does                                                                                                                                                          |
    | ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | `retell_llm_dynamic_variables` | String key-value pairs injected into the agent's prompt and tool descriptions as [dynamic variables](/build/dynamic-variables).                                       |
    | `metadata`                     | An arbitrary object stored on the call for your own bookkeeping, such as an internal customer ID. Not used for processing; returned when you retrieve the call later. |
    | `agent_version`                | The [agent version](/agent/version) to use for this call.                                                                                                             |
    | `agent_override`               | Override parts of the agent's configuration for this call only, without changing the base agent.                                                                      |
    | `current_node_id`              | Start a conversation flow agent at a specific node. Ignored for Retell LLM agents.                                                                                    |
    | `current_state`                | Start a Retell LLM agent that uses states in a specific state. Ignored for conversation flow agents.                                                                  |

    See the [create web call API reference](/api-references/create-web-call) for the full request and response schema.
  </Step>

  <Step title="Start the call from the browser">
    Fetch the access token from your endpoint and pass it to `startCall()`.

    ```javascript theme={"dark"}
    import { RetellWebClient } from "retell-client-js-sdk";

    const retellWebClient = new RetellWebClient();

    // Run this from a click handler so the browser allows microphone
    // access and audio playback.
    async function startCall() {
      const response = await fetch("/api/create-web-call", { method: "POST" });
      const { accessToken } = await response.json();

      await retellWebClient.startCall({ accessToken });
    }
    ```

    The browser prompts the user for microphone permission on the first call. Microphone access only works in a secure context, so serve your page over HTTPS (localhost also works during development).

    `startCall()` accepts these options:

    | Option                | Type             | Description                                                                                                    |
    | --------------------- | ---------------- | -------------------------------------------------------------------------------------------------------------- |
    | `accessToken`         | string, required | The token returned by create web call. Valid for 30 seconds after creation.                                    |
    | `sampleRate`          | number           | Sample rate for audio capture and playback. See [audio basics](/knowledge/audio-basics) for how to choose one. |
    | `captureDeviceId`     | string           | Device ID of the microphone to capture from.                                                                   |
    | `playbackDeviceId`    | string           | Device ID of the speaker to play through.                                                                      |
    | `emitRawAudioSamples` | boolean          | When `true`, the SDK emits `audio` events containing raw PCM audio as `Float32Array`. Defaults to `false`.     |
  </Step>

  <Step title="Listen to call events">
    The SDK emits events for call state, agent speech, and the live transcript. Use them to drive your UI.

    ```javascript theme={"dark"}
    retellWebClient.on("call_started", () => console.log("Call started"));

    retellWebClient.on("update", (update) => {
      console.log(update.transcript);
    });

    retellWebClient.on("agent_start_talking", () => {
      // Start your talking animation
    });

    retellWebClient.on("agent_stop_talking", () => {
      // Stop your talking animation
    });

    retellWebClient.on("call_ended", () => console.log("Call ended"));

    retellWebClient.on("error", (error) => {
      console.error("Call error:", error);
      retellWebClient.stopCall();
    });
    ```

    The full list of events:

    | Event                 | When it fires                                                                                                         |
    | --------------------- | --------------------------------------------------------------------------------------------------------------------- |
    | `call_started`        | The call connected and the microphone is live.                                                                        |
    | `call_ready`          | The agent joined the call and its audio is ready.                                                                     |
    | `agent_start_talking` | The agent starts speaking an utterance. Useful for animations.                                                        |
    | `agent_stop_talking`  | The agent finishes the utterance.                                                                                     |
    | `update`              | A live update arrived. `update.transcript` contains the last 5 sentences of the transcript to keep the payload small. |
    | `metadata`            | A metadata event arrived from the server.                                                                             |
    | `node_transition`     | A conversation flow agent moved to a new node. Only fires for conversation flow agents.                               |
    | `audio`               | Raw PCM audio being played back, as `Float32Array`. Only fires when `emitRawAudioSamples` is `true`.                  |
    | `call_ended`          | The call disconnected, whether the user stopped it, the agent hung up, or an error occurred.                          |
    | `error`               | Something went wrong. The payload is an error message; call `stopCall()` to clean up.                                 |
  </Step>

  <Step title="End the call">
    ```javascript theme={"dark"}
    retellWebClient.stopCall();
    ```

    The agent can also end the call from its side, for example through an end-call function. Either way, the SDK emits `call_ended`.
  </Step>
</Steps>

## Control audio during the call

Mute and unmute the user's microphone without ending the call:

```javascript theme={"dark"}
retellWebClient.mute();   // stop sending microphone audio
retellWebClient.unmute(); // resume sending microphone audio
```

Some browsers block audio playback until the user interacts with the page. If you start a call outside a click handler and hear nothing, call `startAudioPlayback()` inside one:

```javascript theme={"dark"}
await retellWebClient.startAudioPlayback();
```

## After the call

The live `update` event only carries recent transcript sentences, so collect full results after the call ends:

* [Register a webhook](/features/register-webhook) to receive `call_started`, `call_ended`, and `call_analyzed` events on your server.
* Fetch the complete transcript, recording, and analysis with the [get call API](/api-references/get-call), or review the call in [session history](/features/session-history).

## Example project

For a complete working example with a React frontend and a Node.js server, see the [web call demo repository](https://github.com/RetellAI/retell-frontend-reactjs-demo).

## FAQ

<AccordionGroup>
  <Accordion title="Why does my call fail before it starts?">
    The access token expires 30 seconds after `create-web-call` returns it. Create the call when the user clicks to start, not when the page loads.
  </Accordion>

  <Accordion title="Why can't the user hear the agent?">
    Browsers block audio playback until the user interacts with the page. Start the call from a click handler, or call `startAudioPlayback()` inside one.
  </Accordion>

  <Accordion title="Why doesn't the microphone work?">
    The user may have denied the browser's microphone permission, or the page isn't served over HTTPS. Microphone access requires a secure context; localhost works during development.
  </Accordion>

  <Accordion title="How do I pass customer data into the call?">
    Use `retell_llm_dynamic_variables` for values the agent should use in conversation, like the customer's name. Use `metadata` for values you only need to look up later, like an internal customer ID. See [dynamic variables](/build/dynamic-variables).
  </Accordion>

  <Accordion title="Do web calls count toward my concurrency limit?">
    Yes. Web calls draw from the same [concurrency](/deploy/concurrency) pool as outbound phone calls.
  </Accordion>

  <Accordion title="How do I get the full transcript after the call?">
    The `update` event only carries the last 5 sentences. After the call ends, fetch the full transcript with the [get call API](/api-references/get-call) or receive it through a [webhook](/features/register-webhook).
  </Accordion>
</AccordionGroup>
