> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Connect your LLM

> Stream LLM completions to a Retell voice agent over the LLM WebSocket, build prompts from the live transcript, and cancel responses Retell discards.

Your server answers Retell's requests with a hardcoded sentence after [setting up the WebSocket](/integrate-llm/setup-websocket-server). Now generate those answers with an LLM.

Two parts need care: stream the output so the agent starts speaking sooner, and stop generating responses Retell has already discarded so you don't pay for tokens nobody hears.

## Why time to first sentence matters

Retell starts speaking as soon as it has your first complete sentence. So the caller's wait is your **time to first sentence** (time to first token plus however long the model takes to finish that sentence), not the time to generate the whole response.

## Build the prompt from the transcript

Retell sends the full conversation so far in `transcript`, as a list of utterances with `role` set to `agent` or `user`. Map `agent` to your provider's assistant role and everything else to user.

A voice prompt needs a few things a chat prompt doesn't. Tell the model to write speech (short sentences, no markdown, no lists), because anything else gets read aloud literally. Tell it the transcript comes from speech recognition and may contain errors, so it infers meaning instead of asking the caller to repeat themselves. And give it the current date and time in the appropriate timezone.

When `interaction_type` is `reminder_required`, the caller has gone quiet. Append a short instruction so the model nudges rather than answering a question nobody asked.

## Stream the response

<CodeGroup>
  ```typescript llm.ts theme={"dark"}
  import OpenAI from "openai";
  import { WebSocket } from "ws";
  import { ResponseRequiredRequest } from "./types";

  const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });

  const AGENT_PROMPT = `You are Ava, the scheduling assistant for Bright Smile Dental.
  You book, move, and cancel appointments. Office hours are 9am to 5pm on weekdays.
  If a caller asks for anything clinical, offer to take a message for the dentist.`;

  const SYSTEM_PROMPT = `You are a voice agent on a live phone call. Everything you write is
  spoken aloud, so write the way people talk: short sentences, one idea at a time, no lists,
  no markdown, no emoji. Keep replies under 30 words unless the caller asks for detail.

  The transcript comes from speech recognition and will contain errors. If you can tell what
  the caller meant, respond to that. Only ask them to repeat when you genuinely cannot tell,
  and when you do, say it conversationally ("sorry, you cut out there") rather than mentioning
  transcription.

  ## Role
  ${AGENT_PROMPT}`;

  const TIME_ZONE = "America/Los_Angeles";

  function buildMessages(request: ResponseRequiredRequest) {
    const now = new Date().toLocaleString("en-US", { timeZone: TIME_ZONE });

    const messages: OpenAI.Chat.ChatCompletionMessageParam[] = [
      {
        role: "system",
        content: `${SYSTEM_PROMPT}\n\nThe current date and time is ${now} (${TIME_ZONE}).`,
      },
    ];

    for (const utterance of request.transcript) {
      messages.push({
        role: utterance.role === "agent" ? "assistant" : "user",
        content: utterance.content,
      });
    }

    if (request.interaction_type === "reminder_required") {
      messages.push({
        role: "user",
        content: "(The caller has gone quiet. Check in briefly.)",
      });
    }

    return messages;
  }

  export async function draftResponse(
    request: ResponseRequiredRequest,
    ws: WebSocket,
    isStale: () => boolean,
  ) {
    let abandoned = false;

    try {
      const stream = await openai.chat.completions.create({
        model: "gpt-4.1-mini",
        messages: buildMessages(request),
        stream: true,
        temperature: 0.3,
        max_tokens: 200,
      });

      for await (const chunk of stream) {
        if (isStale()) {
          abandoned = true;
          stream.controller.abort();
          break;
        }

        const content = chunk.choices[0]?.delta?.content;
        if (!content) continue;

        ws.send(
          JSON.stringify({
            response_type: "response",
            response_id: request.response_id,
            content,
            content_complete: false,
          }),
        );
      }
    } catch (err) {
      console.error("LLM stream failed:", err);
    } finally {
      // Close the response even when the model errored, or the agent stays silent.
      if (!abandoned) {
        ws.send(
          JSON.stringify({
            response_type: "response",
            response_id: request.response_id,
            content: "",
            content_complete: true,
          }),
        );
      }
    }
  }
  ```

  ```python llm.py theme={"dark"}
  import os
  from datetime import datetime
  from typing import Callable
  from zoneinfo import ZoneInfo

  from openai import AsyncOpenAI

  from custom_types import ResponseRequiredRequest, ResponseResponse

  openai = AsyncOpenAI(api_key=os.environ["OPENAI_API_KEY"])

  AGENT_PROMPT = """You are Ava, the scheduling assistant for Bright Smile Dental.
  You book, move, and cancel appointments. Office hours are 9am to 5pm on weekdays.
  If a caller asks for anything clinical, offer to take a message for the dentist."""

  SYSTEM_PROMPT = f"""You are a voice agent on a live phone call. Everything you write is
  spoken aloud, so write the way people talk: short sentences, one idea at a time, no lists,
  no markdown, no emoji. Keep replies under 30 words unless the caller asks for detail.

  The transcript comes from speech recognition and will contain errors. If you can tell what
  the caller meant, respond to that. Only ask them to repeat when you genuinely cannot tell,
  and when you do, say it conversationally ("sorry, you cut out there") rather than mentioning
  transcription.

  ## Role
  {AGENT_PROMPT}"""

  TIME_ZONE = "America/Los_Angeles"


  def build_messages(request: ResponseRequiredRequest):
      now = datetime.now(ZoneInfo(TIME_ZONE)).strftime("%A, %B %d, %Y at %I:%M %p")

      messages = [
          {
              "role": "system",
              "content": f"{SYSTEM_PROMPT}\n\nThe current date and time is {now} ({TIME_ZONE}).",
          }
      ]

      for utterance in request.transcript:
          role = "assistant" if utterance.role == "agent" else "user"
          messages.append({"role": role, "content": utterance.content})

      if request.interaction_type == "reminder_required":
          messages.append(
              {
                  "role": "user",
                  "content": "(The caller has gone quiet. Check in briefly.)",
              }
          )

      return messages


  async def draft_response(request: ResponseRequiredRequest, is_stale: Callable[[], bool]):
      try:
          stream = await openai.chat.completions.create(
              model="gpt-4.1-mini",
              messages=build_messages(request),
              stream=True,
              temperature=0.3,
              max_tokens=200,
          )

          async for chunk in stream:
              if is_stale():
                  await stream.close()
                  return

              if not chunk.choices:
                  continue

              content = chunk.choices[0].delta.content
              if not content:
                  continue

              yield ResponseResponse(
                  response_id=request.response_id,
                  content=content,
                  content_complete=False,
              )
      except Exception as err:
          print(f"LLM stream failed: {err}")

      # Close the response even when the model errored, or the agent stays silent.
      yield ResponseResponse(
          response_id=request.response_id,
          content="",
          content_complete=True,
      )
  ```
</CodeGroup>

## Handle discarded responses

Retell asks for a response as soon as the caller sounds finished, which keeps the silence short by starting your generation before a response is expected. The cost is that some requests get discarded: when the caller was only pausing mid-thought and keeps going, Retell drops the pending response and asks again with a higher `response_id`.

Retell accepts content only for the `response_id` it most recently requested, and only until you mark that response complete. Content sent under an older `response_id` is dropped without an error, so a discarded response can never reach the caller.

Stopping a discarded generation early is still worth doing, because it costs tokens and provider capacity for audio nobody hears. Track the newest `response_id` per connection and stop as soon as yours is superseded.

<CodeGroup>
  ```typescript Node.js theme={"dark"}
  import { draftResponse } from "./llm";

  app.ws("/llm-websocket/:call_id", (ws: WebSocket, req: Request) => {
    let latestResponseId = 0;

    // ... send config and begin message

    ws.on("message", (data: RawData, isBinary: boolean) => {
      if (isBinary) {
        ws.close(1007, "Expected a text frame.");
        return;
      }
      const request = JSON.parse(data.toString());

      switch (request.interaction_type) {
        case "ping_pong":
          ws.send(
            JSON.stringify({
              response_type: "ping_pong",
              timestamp: request.timestamp,
            }),
          );
          break;

        case "response_required":
        case "reminder_required":
          latestResponseId = request.response_id;
          draftResponse(
            request,
            ws,
            () => request.response_id !== latestResponseId,
          );
          break;
      }
    });
  });
  ```

  ```python Python theme={"dark"}
  import asyncio
  import json

  from llm import draft_response


  @app.websocket("/llm-websocket/{call_id}")
  async def llm_websocket(websocket: WebSocket, call_id: str):
      await websocket.accept()
      latest_response_id = 0

      # ... send config and begin message

      async def respond(request: ResponseRequiredRequest):
          is_stale = lambda: request.response_id != latest_response_id
          async for event in draft_response(request, is_stale):
              if is_stale():
                  return
              await websocket.send_json(event.model_dump(exclude_none=True))

      try:
          while True:
              request = json.loads(await websocket.receive_text())
              interaction_type = request["interaction_type"]

              if interaction_type == "ping_pong":
                  await websocket.send_json(
                      {
                          "response_type": "ping_pong",
                          "timestamp": request["timestamp"],
                      }
                  )

              elif interaction_type in ("response_required", "reminder_required"):
                  latest_response_id = request["response_id"]
                  asyncio.create_task(
                      respond(ResponseRequiredRequest(**request))
                  )
      except WebSocketDisconnect:
          print(f"LLM WebSocket closed for call: {call_id}")
  ```
</CodeGroup>

## Use call context

Set `call_details: true` in your `config` event and Retell sends the whole call object as soon as the socket opens. It has what you need to personalize the first turn without an API round trip:

* `from_number`, `to_number`, `direction` — who's calling and which way. Phone calls only; these fields are absent on web calls.
* `retell_llm_dynamic_variables` — the [dynamic variables](/build/dynamic-variables) passed when the call was created, such as a customer name pulled from your CRM
* `metadata` — anything you attached at call creation
* `call_id`, `agent_id`, `call_type` — for your own logging and for branching on web versus phone

A common pattern: hold the begin message until `call_details` arrives, then greet by name. Drop the begin message from your connection setup and add this case to the message handler above.

```typescript Node.js theme={"dark"}
case "call_details": {
  const name = request.call.retell_llm_dynamic_variables?.customer_name;
  ws.send(
    JSON.stringify({
      response_type: "response",
      response_id: 0,
      content: name
        ? `Hi ${name}, thanks for calling Bright Smile Dental.`
        : "Thanks for calling Bright Smile Dental. How can I help?",
      content_complete: true,
    }),
  );
  break;
}
```

Because `llm_websocket_url` itself supports dynamic variables, you can also route by call. A URL of `wss://your-domain.com/llm-websocket?tenant={{tenant_id}}` reaches your server with the tenant already resolved.

## Try it

Restart your server and start a call from the agent's **Test Audio** panel. The agent should hold a real conversation, start speaking within a beat of you finishing, and stop cleanly when you interrupt.

Check the call in [Call History](/features/session-history) afterward. The transcript and the [latency breakdown](/reliability/check-actual-latency) tell you whether your time to first sentence is where it needs to be. The `llm` field covers your generation including the WebSocket round trip, and `llm_websocket_network_rtt` isolates that round trip, so the difference is your own generation time.

## FAQ

<AccordionGroup>
  <Accordion title="Do I have to answer every response_required event?">
    Yes, unless a newer one has already arrived. Retell waits for content until you send `content_complete: true`, so an ignored request leaves the agent silent. Answering with an empty string plus `content_complete: true` is a valid way to say nothing.
  </Accordion>

  <Accordion title="What happens if my LLM provider errors or times out?">
    Nothing on Retell's side; it just keeps waiting. Always send a final event with `content_complete: true` in a `finally` block, as the code above does. For provider outages, consider a fallback model or a spoken apology so the call degrades instead of going dead.
  </Accordion>

  <Accordion title="Can I keep conversation state on my server instead of using the transcript?">
    Yes, and it's often better. Retell sends the full transcript every time, but nothing stops you from keeping your own per-call state keyed on the call ID from the URL: retrieved documents, tool results, a state machine position. Use the transcript as the source of truth for what was said, and your own state for everything else.
  </Accordion>

  <Accordion title="Why does the agent talk over the caller, or wait too long?">
    That's turn-taking, which Retell controls, not your LLM. Tune `responsiveness` and `interruption_sensitivity` in [interaction settings](/build/interaction-configuration). You can also change them mid-call by sending an [`update_agent` event](/api-references/llm-websocket#update-agent-event), which is useful for making the agent less eager while it waits on a slow lookup.
  </Accordion>
</AccordionGroup>

## Next step

[Add function calling](/integrate-llm/integrate-function-calling) so the agent can book appointments, transfer to a human, and end the call.
