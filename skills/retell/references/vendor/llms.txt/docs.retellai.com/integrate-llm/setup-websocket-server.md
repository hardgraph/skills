> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Set up your server

> Build the LLM WebSocket server that a Retell custom LLM agent connects to: handle config and begin messages, respond to response_required events.

By the end of this guide you'll have a Retell agent that greets you and replies to everything you say with a fixed sentence. No LLM yet. That proves the connection works before you add a model. [Connecting your LLM](/integrate-llm/integrate-llm) is the next guide.

Code below is Node.js (Express) and Python (FastAPI). Any stack that serves WebSockets works, since the protocol is the same in every language.

## Before you start

* **A server that can hold a WebSocket open.** Serverless and edge runtimes generally can't. Vercel edge functions and Lambda-style handlers won't work. Use a long-running process (a container, a VM, or a persistent Node/Python server).
* **TLS in production.** Use `wss://` or `https://`. Retell treats them the same, mapping `https:` to `wss:`. Plain `ws://` and `http://` work too, but send transcripts unencrypted.
* **Read the protocol.** The [LLM WebSocket reference](/api-references/llm-websocket) is the field-by-field spec. This guide covers only the fields you need to get a call working.

Install the dependencies for the code in this section, including the LLM client you'll add in the next guide:

<CodeGroup>
  ```bash Node.js theme={"dark"}
  npm install express express-ws ws openai
  npm install -D typescript tsx @types/express @types/express-ws @types/ws @types/node
  ```

  ```bash Python theme={"dark"}
  pip install "fastapi[standard]" openai
  ```
</CodeGroup>

<Note>
  Retell sends no auth headers on this WebSocket. To restrict who can connect, allowlist Retell's outbound IP address `100.20.5.228`, or put a secret in the URL you configure (a path segment or query string) and reject connections that don't carry it.
</Note>

<Steps>
  <Step title="Add a WebSocket endpoint">
    Retell connects to your `llm_websocket_url` with the call ID appended as the final path segment. If you configure `wss://your-domain.com/llm-websocket`, Retell connects to `wss://your-domain.com/llm-websocket/{call_id}`. Capture that segment. It's how you tie a connection to a call.

    <CodeGroup>
      ```typescript Node.js theme={"dark"}
      import express, { Request } from "express";
      import expressWs from "express-ws";
      import { RawData, WebSocket } from "ws";

      const app = expressWs(express()).app;
      const PORT = 3000;

      app.ws("/llm-websocket/:call_id", (ws: WebSocket, req: Request) => {
        const callId = req.params.call_id;
        console.log("LLM WebSocket open for call:", callId);

        ws.on("error", (err) => console.error("LLM WebSocket error:", err));
        ws.on("close", () => console.log("LLM WebSocket closed for call:", callId));

        ws.on("message", (data: RawData, isBinary: boolean) => {
          if (isBinary) {
            ws.close(1007, "Expected a text frame.");
            return;
          }
          console.log(JSON.parse(data.toString()));
        });
      });

      app.listen(PORT, () => console.log(`Listening on ${PORT}`));
      ```

      ```python Python theme={"dark"}
      import json
      from fastapi import FastAPI, WebSocket, WebSocketDisconnect

      app = FastAPI()

      @app.websocket("/llm-websocket/{call_id}")
      async def llm_websocket(websocket: WebSocket, call_id: str):
          await websocket.accept()
          print(f"LLM WebSocket open for call: {call_id}")

          try:
              while True:
                  print(json.loads(await websocket.receive_text()))
          except WebSocketDisconnect:
              print(f"LLM WebSocket closed for call: {call_id}")
      ```
    </CodeGroup>

    Save this as `server.ts` or `server.py`, then run it with `npx tsx server.ts` or `uvicorn server:app --port 3000`. This guide adds `types.ts` or `custom_types.py` next to it, and the [next one](/integrate-llm/integrate-llm) adds `llm.ts` or `llm.py`. Keep all of them in the same directory, since the imports in each are flat.

    Every message is a text frame containing stringified JSON. Retell never sends binary frames, and it closes the connection with code `1007` if you send one.
  </Step>

  <Step title="Send config and a begin message">
    Your server speaks first. Send two messages as soon as the connection opens:

    A **config** event turns on optional protocol features. `auto_reconnect` starts the keepalive exchange so a dropped connection gets rebuilt instead of killing the call. `call_details` makes Retell push the whole call object to you immediately, saving you a [Get Call API](/api-references/get-call) round trip.

    A **response** event with `response_id: 0` is the begin message, the first thing the agent says. Set `content` to an empty string to have the agent wait for the caller to speak first.

    <CodeGroup>
      ```typescript Node.js theme={"dark"}
      const BEGIN_MESSAGE = "How may I help you?";

      ws.send(
        JSON.stringify({
          response_type: "config",
          config: {
            auto_reconnect: true,
            call_details: true,
          },
        }),
      );

      ws.send(
        JSON.stringify({
          response_type: "response",
          response_id: 0,
          content: BEGIN_MESSAGE,
          content_complete: true,
        }),
      );
      ```

      ```python Python theme={"dark"}
      BEGIN_MESSAGE = "How may I help you?"

      await websocket.send_json(
          {
              "response_type": "config",
              "config": {
                  "auto_reconnect": True,
                  "call_details": True,
              },
          }
      )

      await websocket.send_json(
          {
              "response_type": "response",
              "response_id": 0,
              "content": BEGIN_MESSAGE,
              "content_complete": True,
          }
      )
      ```
    </CodeGroup>

    <Warning>
      Setting `auto_reconnect: true` obligates you to echo `ping_pong` events, which you'll add in the next step. If Retell goes 5 seconds without one, it closes the connection and reconnects. After 2 reconnects it gives up and ends the call with `error_llm_websocket_lost_connection`.
    </Warning>

    If your greeting depends on call data, such as a caller's name from a dynamic variable, send `config` immediately but hold the begin message until the `call_details` event arrives.
  </Step>

  <Step title="Answer each interaction type">
    Now handle what Retell sends. Only `response_required` and `reminder_required` need a spoken answer, and `ping_pong` needs an echo. `update_only` and `call_details` need no reply at all.

    Reply with the same `response_id` Retell asked with. A response carrying any other `response_id` is discarded silently.

    <CodeGroup>
      ```typescript Node.js theme={"dark"}
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
            ws.send(
              JSON.stringify({
                response_type: "response",
                response_id: request.response_id,
                content: "I am sorry, can you say that again?",
                content_complete: true,
              }),
            );
            break;

          case "call_details":
            console.log("Call details:", request.call);
            break;

          case "update_only":
            // Live transcript and turn taking. Nothing to answer.
            break;
        }
      });
      ```

      ```python Python theme={"dark"}
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
              await websocket.send_json(
                  {
                      "response_type": "response",
                      "response_id": request["response_id"],
                      "content": "I am sorry, can you say that again?",
                      "content_complete": True,
                  }
              )

          elif interaction_type == "call_details":
              print("Call details:", request["call"])

          # update_only carries the live transcript. Nothing to answer.
      ```
    </CodeGroup>

    Set `content_complete: true` on the last event of a response. Retell keeps waiting for more content until it sees that flag, so a response that never completes leaves the turn hanging: the agent stops speaking, no error is raised, and it only recovers once the caller speaks again.
  </Step>

  <Step title="Expose your server and point an agent at it">
    Retell has to reach your server over the public internet.

    * **Deployed:** use your own domain, `wss://your-domain.com/llm-websocket`
    * **Local:** tunnel it with [ngrok](https://ngrok.com/) (`ngrok http 3000`) and use the forwarding host, `wss://xxxx.ngrok-free.app/llm-websocket`

    Create a Custom LLM agent in the dashboard, then paste the URL into the **Custom LLM URL** field on the agent.

    <Frame caption="Replace the placeholder in Custom LLM URL with your own endpoint.">
      <img src="https://mintcdn.com/retellai/P818NZjsPPgF1yY8/images/custom-llm-url-field.png?fit=max&auto=format&n=P818NZjsPPgF1yY8&q=85&s=10310100a0ff1ae6880eb8f18fff83c3" alt="Agent details panel for a Custom LLM agent, with the Custom LLM URL field outlined in blue and holding the default placeholder https://replace-with-your-llm-url.com" width="1600" height="425" data-path="images/custom-llm-url-field.png" />
    </Frame>

    The URL supports [dynamic variables](/build/dynamic-variables), so `wss://your-domain.com/llm-websocket?tenant={{tenant_id}}` resolves per call. Use it to route calls to different backends, or to pass a shared secret.
  </Step>

  <Step title="Make a web call">
    Open the **Test Audio** tab on the agent and click **Run Test**. It's the only test surface a custom LLM agent has: the **Test LLM** tab is hidden for them, and [simulation testing](/test/llm-simulation-testing) rejects them.

    You should hear "How may I help you?", and every time you speak the agent should answer "I am sorry, can you say that again?". Your server logs should show a `call_details` event, then `update_only` events as you talk, then a `response_required` event for each reply.
  </Step>
</Steps>

## Message types

Typed definitions for the events in this guide. Reuse them across the rest of the section.

<CodeGroup>
  ```typescript types.ts theme={"dark"}
  export interface Utterance {
    role: "agent" | "user";
    content: string;
  }

  // Retell -> your server
  export interface PingPongRequest {
    interaction_type: "ping_pong";
    timestamp: number;
  }

  export interface CallDetailsRequest {
    interaction_type: "call_details";
    call: Record<string, any>;
  }

  export interface UpdateOnlyRequest {
    interaction_type: "update_only";
    transcript: Utterance[];
    turntaking?: "agent_turn" | "user_turn";
  }

  export interface ResponseRequiredRequest {
    interaction_type: "response_required" | "reminder_required";
    transcript: Utterance[];
    response_id: number;
  }

  export type CustomLlmRequest =
    | PingPongRequest
    | CallDetailsRequest
    | UpdateOnlyRequest
    | ResponseRequiredRequest;

  // Your server -> Retell
  export interface ConfigResponse {
    response_type: "config";
    config: {
      auto_reconnect?: boolean;
      call_details?: boolean;
      transcript_with_tool_calls?: boolean;
    };
  }

  export interface PingPongResponse {
    response_type: "ping_pong";
    timestamp: number;
  }

  export interface ResponseResponse {
    response_type: "response";
    response_id: number;
    content: string;
    content_complete: boolean;
    no_interruption_allowed?: boolean;
    end_call?: boolean;
    transfer_number?: string;
    show_transferee_as_caller?: boolean;
    digit_to_press?: string;
  }

  export type CustomLlmResponse =
    | ConfigResponse
    | PingPongResponse
    | ResponseResponse;
  ```

  ```python custom_types.py theme={"dark"}
  from typing import Any, Dict, List, Literal, Optional, Union

  from pydantic import BaseModel


  class Utterance(BaseModel):
      role: Literal["agent", "user"]
      content: str


  # Retell -> your server
  class PingPongRequest(BaseModel):
      interaction_type: Literal["ping_pong"]
      timestamp: int


  class CallDetailsRequest(BaseModel):
      interaction_type: Literal["call_details"]
      call: Dict[str, Any]


  class UpdateOnlyRequest(BaseModel):
      interaction_type: Literal["update_only"]
      transcript: List[Utterance]
      turntaking: Optional[Literal["agent_turn", "user_turn"]] = None


  class ResponseRequiredRequest(BaseModel):
      interaction_type: Literal["response_required", "reminder_required"]
      transcript: List[Utterance]
      response_id: int


  CustomLlmRequest = Union[
      PingPongRequest, CallDetailsRequest, UpdateOnlyRequest, ResponseRequiredRequest
  ]


  # Your server -> Retell
  class ConfigResponse(BaseModel):
      response_type: Literal["config"] = "config"
      config: Dict[str, bool]


  class PingPongResponse(BaseModel):
      response_type: Literal["ping_pong"] = "ping_pong"
      timestamp: int


  class ResponseResponse(BaseModel):
      response_type: Literal["response"] = "response"
      response_id: int
      content: str
      content_complete: bool
      no_interruption_allowed: Optional[bool] = None
      end_call: Optional[bool] = None
      transfer_number: Optional[str] = None
      show_transferee_as_caller: Optional[bool] = None
      digit_to_press: Optional[str] = None


  CustomLlmResponse = Union[ConfigResponse, PingPongResponse, ResponseResponse]
  ```
</CodeGroup>

## FAQ

<AccordionGroup>
  <Accordion title="How many times does Retell retry if my server is unreachable?">
    On the initial connection, Retell makes up to 3 attempts with a 7-second timeout each and 3 seconds between them. If all fail, the call ends with `error_llm_websocket_open`. Mid-call, Retell rebuilds a dropped connection instead of giving up: up to 2 reconnects when keepalives stop arriving, and up to 4 when the socket closes abnormally (code `1006`).
  </Accordion>

  <Accordion title="Do I have to send a config event?">
    No. Skip it and you get the defaults: no keepalives, no call details, no tool-call transcripts. The begin message alone is enough to start a call. Send `config` first if you send it at all, since Retell acts on it as it arrives.
  </Accordion>

  <Accordion title="Can I run this on Vercel, Lambda, or Cloudflare Workers?">
    Not on serverless or edge functions. They can't hold a WebSocket open for the length of a call. Deploy a long-running process instead. Also watch for idle timeouts on hosts that have them: a platform that kills connections at 5 minutes will drop every call that runs longer.
  </Accordion>

  <Accordion title="Why did my response never get spoken?">
    Most often the `response_id` didn't match what Retell asked for, or `content_complete: true` never arrived. Retell also discards a response outright when the caller keeps talking and asks again with a new `response_id`, which is expected. See [handling discarded responses](/integrate-llm/integrate-llm#handle-discarded-responses).
  </Accordion>

  <Accordion title="Can I test the endpoint without placing a call?">
    Yes. Connect to it with Postman or `websocat` and send a JSON frame shaped like a `response_required` event. Your server should answer with a `response` event. This separates connectivity problems from protocol problems.
  </Accordion>
</AccordionGroup>

## Next step

Your agent answers with a fixed sentence. [Connect your LLM](/integrate-llm/integrate-llm) to stream real responses.
