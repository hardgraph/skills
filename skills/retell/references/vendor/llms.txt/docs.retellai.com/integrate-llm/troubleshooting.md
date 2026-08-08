> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Troubleshooting

> Debug a Retell custom LLM integration: map error_llm_websocket disconnection reasons to fixes for silent agents, dropped calls, and WebSocket errors.

Start with the call record. Open the call in [Call History](/features/session-history) and check two things: the **disconnection reason**, and the **Detail Logs**, which contain the LLM WebSocket lifecycle for that call.

Useful log lines to search for:

| Log line                                 | Means                                                     |
| ---------------------------------------- | --------------------------------------------------------- |
| `LLM Ws init attempt 0`                  | Retell is trying to open the connection                   |
| `LLM ws open`                            | The connection succeeded                                  |
| `LLM Ws init timeout on attempt N`       | Your server didn't accept the connection within 7 seconds |
| `Ws max retry attempt reached`           | Retell gave up connecting                                 |
| `LLM ws closed: <code>`                  | The connection closed, with the WebSocket close code      |
| `Got 1006 and reconnecting`              | Abnormal closure; Retell is rebuilding the connection     |
| `Error in parsing LLM websocket message` | Retell received something from you it couldn't parse      |

## Disconnection reasons

Four disconnection reasons point at the LLM WebSocket. The [full reason table](/reliability/debug-call-disconnect) covers the rest.

| Reason                                | What happened                                                                                                                                                                                                        | Where to look                                                                                                                                                  |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `error_llm_websocket_open`            | Retell never got the connection open. It makes 3 attempts, 7-second timeout each, 3 seconds apart, then gives up. You also get this reason mid-call if your server closes the socket without a status code (`1005`). | The URL itself, DNS, TLS, whether the process is listening, whether the path matches your route. For the mid-call case, whether your close path passes a code. |
| `error_llm_websocket_lost_connection` | Keepalives stopped arriving and Retell used up its reconnects, or a reconnect couldn't reach your server.                                                                                                            | Whether you're echoing `ping_pong`, and whether your host or proxy has an idle timeout.                                                                        |
| `error_llm_websocket_runtime`         | The socket errored mid-call, your server closed it with an unexpected code, or it kept closing abnormally (`1006`) until Retell ran out of reconnects.                                                               | Unhandled exceptions in your handler, and the close code in `LLM ws closed`.                                                                                   |
| `error_llm_websocket_corrupt_payload` | Close code `1007`. Retell sends this when it receives a binary frame instead of text.                                                                                                                                | Whether you're sending text frames of stringified JSON, not buffers.                                                                                           |

Closing the socket with code `1000` from your side is treated as a deliberate hangup, so the call ends with `agent_hangup` rather than an error. Don't close with `1000` to signal a problem.

## The agent never speaks

Work down this list in order.

<Steps>
  <Step title="Confirm the connection opens at all">
    Look for `LLM ws open` in the Detail Logs. If you only see init attempts and timeouts, this is a connectivity problem, not a protocol one. Skip to [connection failures](#connection-failures).
  </Step>

  <Step title="Check that you sent a begin message">
    Retell waits for a `response` event with `response_id: 0` before the agent says anything. If your greeting is empty by design, the agent stays silent until the caller speaks.
  </Step>

  <Step title="Check content_complete">
    Retell keeps a response open until it receives an event with `content_complete: true`. Miss that flag and the turn never finishes: the agent stops speaking and no error is raised anywhere. It only recovers when the caller speaks again, which takes over the turn and triggers a fresh request. Send the flag as the final event of every response, including in your error path.
  </Step>

  <Step title="Check the response_id matches">
    Retell only accepts content for the `response_id` it most recently asked for. Answer with `response_id: 4` when it asked with `5` and your content is dropped silently. Echo back the exact `response_id` from the request.
  </Step>

  <Step title="Validate the field types">
    Retell rejects a `response` event when `content_complete` isn't a boolean, `content` isn't a string, or `response_id` isn't an integer. It logs the problem and keeps the connection open, so the event just vanishes. `"content_complete": "true"` is a string, not a boolean, and is the common version of this mistake.
  </Step>

  <Step title="Look for parse errors">
    `Error in parsing LLM websocket message` in the logs means your JSON was malformed or the frame wasn't what Retell expected. Retell logs it and carries on, so the call continues with an agent that says nothing.
  </Step>
</Steps>

## Connection failures

**Check the URL protocol.** Use `wss://` (or `https://`) for a TLS endpoint and `ws://` (or `http://`) for a plain one. Retell maps `https:` to `wss:` and `http:` to `ws:`, so those pairs are interchangeable. What fails is a mismatch: pointing `wss://` or `https://` at a server without TLS never completes the handshake.

**Don't append the call ID.** Configure the base path only: `wss://your-domain.com/llm-websocket`. Retell appends `/{call_id}`. A trailing slash is fine either way, since Retell normalizes it before appending.

**Confirm your route accepts the extra path segment.** Your handler must match a path with the call ID on the end, like `/llm-websocket/:call_id` in Express or `/llm-websocket/{call_id}` in FastAPI. A route registered at exactly `/llm-websocket` won't match what Retell connects to.

**Check that your framework can serve WebSockets at all.** On Python, `pip install fastapi uvicorn` installs no WebSocket library, and the failure looks exactly like a routing bug: uvicorn answers every upgrade request with `404 Not Found` and logs one `No supported WebSocket library detected` warning. Your route is fine. Install `fastapi[standard]`, `uvicorn[standard]`, or `websockets`, then look for `connection open` in the uvicorn log instead of a `404`.

**Test it without a call.** Connect to your full URL with Postman or `websocat` and send a JSON frame shaped like a `response_required` event:

```bash theme={"dark"}
websocat wss://your-domain.com/llm-websocket/test-call-id
```

```json theme={"dark"}
{ "interaction_type": "response_required", "response_id": 1, "transcript": [] }
```

If your server doesn't answer with a `response` event, the problem is in your code, not in Retell's connection. If it does answer, and Retell still can't connect, suspect a firewall or IP allowlist.

**Check your allowlist.** If you've restricted inbound traffic, Retell's outbound IP is `100.20.5.228`.

## The call drops after a few seconds

**Check whether you sent `end_call: true`.** It's easy to attach to the wrong response. Search your logs for it.

**Check whether your runtime can hold a WebSocket.** Serverless and edge functions can't. Vercel edge functions, Lambda-style handlers, and Cloudflare Workers all end the invocation instead of holding the connection. Deploy a long-running process.

**Check whether you're echoing `ping_pong`.** If you set `auto_reconnect: true` in your `config` event, you must send a `ping_pong` event back within 5 seconds of each one Retell sends. Miss the window and Retell closes the connection and reconnects; after 2 reconnects the call ends with `error_llm_websocket_lost_connection`.

A blocking response handler is the usual culprit here. If your code awaits a full LLM generation before reading the next frame, the keepalives queue up behind it and you fall outside the window. Handle each message concurrently.

## The call drops at a round number of minutes

That's an idle or lifetime timeout on the host or proxy in front of your server, not Retell. Common cases: Replit's non-reserved instances time out at 5 minutes, and load balancers, reverse proxies, and API gateways often have their own WebSocket idle timeouts.

Turning on `auto_reconnect` helps, because the 2-second keepalive traffic keeps a purely *idle* timeout from firing. It won't save you from a hard connection lifetime cap. For that, raise the timeout on the proxy.

## Responses get generated but never spoken

Usually correct behavior. Retell asks for a response whenever it thinks the agent's turn has arrived, and discards it if the caller keeps talking. It then asks again with a higher `response_id`. Several requests where only the last one gets spoken is what a normal call looks like.

To stop paying for the discarded ones, see [handling discarded responses](/integrate-llm/integrate-llm#handle-discarded-responses).

## Tool calls don't appear in the transcript

Retell can't see your tool calls. Send `tool_call_invocation` and `tool_call_result` events and it weaves them into the transcript. See [recording tool calls](/integrate-llm/integrate-function-calling#record-tool-calls-in-the-transcript).

## FAQ

<AccordionGroup>
  <Accordion title="Retell sends no auth headers. How do I secure my endpoint?">
    Two options, and they combine well. Allowlist Retell's outbound IP `100.20.5.228` at your firewall. And because `llm_websocket_url` supports [dynamic variables](/build/dynamic-variables), you can put a secret in the URL, as a path segment or query string, and reject connections without it.
  </Accordion>

  <Accordion title="What is the (unintelligible audio) utterance in my transcript?">
    When Retell detects that the caller spoke but transcribes no words (background noise, a cough, speech too quiet to resolve), it sends the utterance with the content `(unintelligible audio)` instead of an empty string. That way your model knows a turn happened rather than seeing a blank. Pass it through to your prompt; a good voice system prompt handles it by asking the caller to repeat themselves conversationally.
  </Accordion>

  <Accordion title="My agent works on web calls but not phone calls.">
    The LLM WebSocket is identical for both, so look at telephony rather than your server. Check the disconnection reason: a `dial_*` or `sip_*` reason means the call never reached the point of needing your LLM. See [debug outbound calls](/reliability/debug-outbound-call).
  </Accordion>

  <Accordion title="Can I see what Retell sent my server for a specific call?">
    Yes. Detail Logs record the events in both directions, with transcripts trimmed to the last 2 utterances to keep them readable. Log the raw frames on your side too, keyed on the call ID from the URL, so you can line the two up.
  </Accordion>

  <Accordion title="The agent responds but the latency is bad. Where do I start?">
    Check the [latency breakdown](/reliability/check-actual-latency) on the call to confirm the delay is yours and not Retell's. For a custom LLM, `llm` covers your generation *including* the WebSocket round trip, and `llm_websocket_network_rtt` isolates that round trip, so subtract one from the other to get your own generation time. Because Retell starts speaking at your first complete sentence, the number to optimize is time to first sentence, not total generation time. See [best practices](/integrate-llm/llm-best-practice).
  </Accordion>
</AccordionGroup>
