> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# LLM WebSocket

> LLM WebSocket protocol for streaming requests from Retell to your custom LLM server and returning responses, tool calls, and DTMF actions during a live call.

<Note>
  We recommend [single prompt](/build/prompt) or [conversation flow](/build/conversation-flow/overview) for most agents. They get new features first, and come with built-in tool sets and latency tuning.
</Note>

<Note>This page is the field-by-field protocol spec. For step-by-step instructions, start with the [custom LLM overview](/integrate-llm/overview), then [set up your server](/integrate-llm/setup-websocket-server), [connect your LLM](/integrate-llm/integrate-llm), and [add function calling](/integrate-llm/integrate-function-calling). If a call isn't working, see [troubleshooting](/integrate-llm/troubleshooting).</Note>

## Overview

This socket shall connect directly to your server, where you would get live transcript and
other relevant inputs from us, and provide responses back to us using your custom LLM.
In short, this WebSocket controls what the agent says, and controls actions like ending the call.

Retell AI will initiate this WebSocket when starting the call,
and your server should get prepared to handle it.

## Endpoint

<Note>WebSocket Endpoint: `{your-server-websocket-endpoint}/{call_id}`</Note>

### Path Parameters

<ParamField query="call_id" type="string" required>
  Unique call id to identify the call.
</ParamField>

## Protocol

Retell and your server would conform to the following protocol sending the WebSocket messages to communicate.

All message event types are "text", where the `data` attribute of message event is
a JSON object stringified.

### Event Flow

The connection would start by your server sending an optional [config event](/api-references/llm-websocket#config-event) to Retell,
and a [response event](/api-references/llm-websocket#response-event)
that serves as the begin message for agent to speak.
Set content to empty string if you want the agent to wait for user to start the conversation.

Retell would send back a [call details event](/api-references/llm-websocket#call-details-event) if that's enabled in config.
Retell and your server would periodically send [ping pong events](/api-references/llm-websocket#ping-pong-event)
to keep the connection alive if configured in config.

As the call goes, Retell would send over live transcript and other updates in
[update only events](/api-references/llm-websocket#update-only-event), and determine when it is appropriate to
ask for responses / reminders in
[response and reminder required events](/api-references/llm-websocket#response-and-reminder-required-events). Your server will be
sending back [response events](/api-references/llm-websocket#response-event) accordingly.
Not all your responses would get spoken out, because the user might
continue to speak even when Retell thought it would be agent's turn.

If at certain point, you want agent to jump in the conversation and speak something immediately, you can send an
[agent interrupt event](/api-references/llm-websocket#agent-interrupt-event).

### Retell -> Your Server Event Spec

There will be a couple of events that Retell can send to your server in this websocket,
to differentiate between them, check the `interaction_type` field:

* `ping_pong`: (optional) to check for disconnection and keep the connection alive
* `update_only`: (required) to send real-time updates about the call like live transcript
* `response_required`: (required) ask for response content from your server
* `reminder_required`: (required) ask for reminder content from your server

#### Ping Pong Event

When you set `auto_reconnect` to true in the [config event](/api-references/llm-websocket#config-event),
Retell will send ping\_pong events to your server
every 2s to keep the connection alive.

<ParamField query="interaction_type" type="enum<string>" required>
  Differentiate what this event is.

  Available options: `ping_pong`
</ParamField>

<ParamField query="timestamp" type="integer" required>
  Timestamp (milliseconds since epoch) of when Retell sent this event.
  You can use this to calculate the time taken for the round trip.
</ParamField>

#### Call Details Event

When you set `call_details` to true in the [config event](/api-references/llm-websocket#config-event),
Retell will send call details events to your server right away so that you can save the time of retrieving call detail from
[Get Call API](/api-references/get-call).

<ParamField query="interaction_type" type="enum<string>" required>
  Differentiate what this event is.

  Available options: `call_details`
</ParamField>

<ParamField query="call" type="object" required>
  Contains the response from [Register Call API](/api-references/register-call).
</ParamField>

#### Update Only Event

Retell would send an event when transcript updates -- either user speaks, or agent speaks.
Retell also sends this event when turntaking happens.

<ParamField query="interaction_type" type="enum<string>" required>
  Event type. This event is simply an update containing the latest transcript or turntaking information, no response required.

  Available options: `update_only`
</ParamField>

<ParamField query="transcript" type="object[]" required>
  Complete live transcript collected in the call so far. Presented in the form of a list of
  utterances.

  See `transcript_object` field of [Get Call API Response](/api-references/get-call) for the detailed schema
  for the object in the list.
</ParamField>

<ParamField query="transcript_with_tool_calls" type="object[]">
  Transcript of the call weaved with tool call invocation and results. Populated when `transcript_with_tool_calls` field is set to
  true in the [config event](/api-references/llm-websocket#config-event).

  It precisely captures when (at what utterance, which word) the tool was invoked and what was the result
  if the tool calls were sent timely in this LLM websocket. See
  [tool call invocation event](/api-references/llm-websocket#tool-call-invocation-event)
  and [tool call result event](/api-references/llm-websocket#tool-call-result-event) for more information of how to send
  tool call invocations and results.

  See `transcript_with_tool_calls` field of [Get Call API Response](/api-references/get-call) for the detailed schema
  for the object in the list.
</ParamField>

<ParamField query="turntaking" type="enum<string>">
  Indicates change of speaker (turn taking). This field will be present when speaker changes to user (user turn), or right before
  agent is about to speak (agent turn). This field can be helpful in determining when to call functions in the call.

  Available options: `agent_turn`, `user_turn`
</ParamField>

#### Response and Reminder Required Events

Retell would continuously assess if it's a good time for agent to speak, and would
ask for content for response / reminder when appropriate, but not all
responses Retell asks for would get spoken out (as user might continue to speak).

<ParamField query="interaction_type" type="enum<string>" required>
  Determines what we need from your server.

  * `response_required`: Require a response from your server for the current live transcript.
  * `reminder_required`: User has not spoken for a while, a reminder is needed from your server.

  Available options: `response_required`, `reminder_required`
</ParamField>

<ParamField query="response_id" type="integer" required>
  This unique auto-incrementing id is used to track the response Retell needs, and used to identify the
  responses streamed from your server, as you can
  send multiple events to stream back responses, and we need an id to group them.

  When a new response is needed, a new event with response id will
  be sent, and all previous responses will be discarded.
</ParamField>

<ParamField query="transcript" type="object[]" required>
  Complete live transcript collected in the call so far. Presented in the form of a list of
  utterances.

  See `transcript_object` field of [Get Call API Response](/api-references/get-call) for the detailed schema
  for the object in the list.
</ParamField>

<ParamField query="transcript_with_tool_calls" type="object[]">
  Transcript of the call weaved with tool call invocation and results. Populated when `transcript_with_tool_calls` field is set to
  true in the [config event](/api-references/llm-websocket#config-event).

  It precisely captures when (at what utterance, which word) the tool was invoked and what was the result
  if the tool calls were sent timely in this LLM websocket. See
  [tool call invocation event](/api-references/llm-websocket#tool-call-invocation-event)
  and [tool call result event](/api-references/llm-websocket#tool-call-result-event) for more information of how to send
  tool call invocations and results.

  See `transcript_with_tool_calls` field of [Get Call API Response](/api-references/get-call) for the detailed schema
  for the object in the list.
</ParamField>

### Your Server -> Retell Event Spec

There will be a couple of events that your server can send to Retell in this websocket,
to differentiate between them, set the `response_type` field:

* `config`: (optional) the initial config for configuring reconnection, whether to send call details etc.
* `ping_pong`: (optional) to check for disconnection and keep the connection alive
* `response`: (required) to send back responses to the user when requested
* `agent_interrupt`: (optional) to jump in conversation and speak content in it immediately, interrupts both
  agent and user.
* `tool_call_invocation`: (optional) to bookkeep and weave tool call invocations and results in the transcript.
* `tool_call_result`: (optional) to bookkeep and weave tool call invocations and results in the transcript.
* `metadata`: (optional) to pass some data from the server where the LLM is running to the frontend during a web call.

#### Config Event

You can send a config at connection open to configure reconnection, whether to send call details, etc.

<ParamField query="response_type" type="enum<string>" required>
  Differentiate what this event is.

  Available options: `config`
</ParamField>

<ParamField query="config" type="object" required>
  Configuration object to control whether to auto reconnect, and whether Retell sends a call detail over.

  <Expandable title="properties">
    <ResponseField name="auto_reconnect" type="boolean">
      If set to true, Retell will send ping pong events to your server, and would expect ping pong events back
      from your server every 2s. Once there's 5s without ping pong event, Retell would close the current connection
      and restart a new connection to your server for up to 2 times.
    </ResponseField>

    <ResponseField name="call_details" type="boolean">
      If set to true, Retell will send call details over to your server right away. See
      [call details event](/api-references/llm-websocket#call-details-event) for more information.
    </ResponseField>

    <ResponseField name="transcript_with_tool_calls" type="boolean">
      If set to true, Retell will populate an additional field in the [update only events](/api-references/llm-websocket#update-only-event),
      [response and reminder required events](/api-references/llm-websocket#response-and-reminder-required-events).
      This additional field will contain
      transcript of the call weaved with tool call invocation and results.

      You need to send tool call invocations and results to us in websocket so that we can construct it. See
      [tool call invocation event](/api-references/llm-websocket#tool-call-invocation-event)
      and [tool call result event](/api-references/llm-websocket#tool-call-result-event) for more information of how to send
      tool call invocations and results.
    </ResponseField>
  </Expandable>
</ParamField>

#### Update Agent Event

You can send agent update events at any time of the call to update some of the agent configurations.
We might add more configuration to this event in the future.

This can be useful when you want to modify the agent behavior during the call, like when
you want to use reminders as a way to let agent continue speaking, or you wish to make agent
less responsive when user is looking up information.

<ParamField query="response_type" type="enum<string>" required>
  Differentiate what this event is.

  Available options: `update_agent`
</ParamField>

<ParamField query="agent_config" type="object" required>
  Set what agent configuration you want to update. All fields are optional.

  <Expandable title="properties">
    <ResponseField name="responsiveness" type="number">
      Controls how responsive the agent is. Value ranging from \[0,1].
      Lower value means less responsive agent (wait more, respond slower),
      while higher value means faster exchanges (respond when it can).
    </ResponseField>

    <ResponseField name="interruption_sensitivity" type="number">
      Controls how sensitive the agent is to user interruptions. Value
      ranging from \[0,1]. Lower value means it will take longer / more
      words for user to interrupt agent, while higher value means it's
      easier for user to interrupt agent.
    </ResponseField>

    <ResponseField name="reminder_trigger_ms" type="number">
      If set (in milliseconds), will trigger a reminder to the agent to
      speak if the user has been silent for the specified duration after
      some agent speech. Must be a positive number.
    </ResponseField>

    <ResponseField name="reminder_max_count" type="number">
      If set, controls how many times agent would remind user when user is
      unresponsive. Must be a non-negative integer. Set to 0 to disable agent from
      reminding.
    </ResponseField>
  </Expandable>
</ParamField>

#### Ping Pong Event

When you set `auto_reconnect` to true in the [config event](/api-references/llm-websocket#config-event),
you need to send ping\_pong events to signal that your server
is still alive. Retell would expect a ping\_pong event back every 2s, and would close the connection if there's no ping\_pong
event for 5s.

<ParamField query="response_type" type="enum<string>" required>
  Differentiate what this event is.

  Available options: `ping_pong`
</ParamField>

<ParamField query="timestamp" type="integer" required>
  Timestamp (milliseconds since epoch) of when your server sent this event.
  Retell would use this to calculate the time taken for the round trip.
</ParamField>

#### Response Event

Your server needs to respond to [response and reminder required events](/api-references/llm-websocket#response-and-reminder-required-events)
so that agent can speak in time.
You can stream the response or send it in one go, although streaming is
recommended for lower latency. When a newer event that requires response / reminder is received, you can
stop responding to the previous response / reminder required events, as it would not get used. You still need to respond to
previous response / reminder required event if newer update\_only events are received.

<ParamField query="response_type" type="enum<string>" required>
  Differentiate what this event is.

  Available options: `response`
</ParamField>

<ParamField query="response_id" type="integer" required>
  Indicates which requested response this is answering.
</ParamField>

<ParamField query="content" type="string" required>
  Partial or full response content.
</ParamField>

<ParamField query="content_complete" type="boolean" required>
  Whether the content is complete. When streaming responses back, only the last event of the response
  should have this field set to true.
</ParamField>

<ParamField query="no_interruption_allowed" type="boolean">
  If set to true, agent would not get interrupted by user for content in this event. Useful for conveying important information.
</ParamField>

<ParamField query="end_call" type="boolean">
  If set to true, Retell would end the call after content associated with this id is fully spoken. If agent was interrupted
  during speaking, the end call signal would get discarded.
</ParamField>

<ParamField query="transfer_number" type="string">
  If set, Retell would transfer the call to the number specified here after content associated with this id is fully spoken.
  Only applicable to Retell numbers or imported numbers. If your voice agent is using custom telephony via the dial to
  SIP endpoint route, you need to write your own call transfer logic.
</ParamField>

<ParamField query="show_transferee_as_caller" type="boolean" default="false">
  If set to true, the transferee will see the original caller's number instead of the Retell number when the call is transferred.
</ParamField>

<ParamField query="digit_to_press" type="string">
  If set, Retell would press the input digit or digits to send DTMF tones after content associated with this id is fully spoken
  (although you probably do not want the voice agent to speak anything when pressing digits).
</ParamField>

#### Agent Interrupt Event

If at certain point, you want to jump in the conversation and speak something immediately, you can send this event.
It will stop the agent speech if the agent is speaking, or it will interrupt the user if the user is speaking.

<ParamField query="response_type" type="enum<string>" required>
  Differentiate what this event is.

  Available options: `agent_interrupt`
</ParamField>

<ParamField query="interrupt_id" type="integer" required>
  Used to group the interrupt events. This is a unique id maintained in your server. If interrupt events with same
  ids are received, the content would get spoken in order of the interrupt events received. If interrupt events with
  different ids are received, the previous interrupt events are discarded.
</ParamField>

<ParamField query="content" type="string" required>
  Partial or full response content.
</ParamField>

<ParamField query="content_complete" type="boolean" required>
  Whether the content is complete. When streaming responses back, only the last event of the response
  should have this field set to true.
</ParamField>

<ParamField query="no_interruption_allowed" type="boolean">
  If set to true, agent would not get interrupted by user for content in this event.
  This is recommended to set to true here because without this setting,
  if user is talking, and agent interrupts here, the two parties would
  speak at the same time, and agent would get interrupted quickly.
</ParamField>

<ParamField query="end_call" type="boolean">
  If set to true, and if this response is used for agent to speak,
  we would end the call after content is fully spoken.
</ParamField>

<ParamField query="transfer_number" type="string">
  If set, we will transfer the call to the number specified here after content is fully spoken.
  Only applicable to numbers purchased through Retell.
  For call transfer with your own Twilio account, you can trigger it from your server directly.
</ParamField>

<ParamField query="digit_to_press" type="string">
  If set, Retell would press the input digit or digits to send DTMF tones after content associated with this id is fully spoken
  (although you probably do not want the voice agent to speak anything when pressing digits).
</ParamField>

#### Tool Call Invocation Event

If you send the tool call invocation and results in the websocket,
Retell would populate the `transcript_with_tool_calls` field in the [Get Call API Response](/api-references/get-call)
after call ends. We would weave the transcript and precisely capture when (at what utterance, which word)
the tool was invoked and what was the result.

If you also set `transcript_with_tool_calls` to true in the [config event](/api-references/llm-websocket#config-event),
Retell would populate the `transcript_with_tool_calls`
field in the [update only event](/api-references/llm-websocket#update-only-event) with the tool call invocations and results.
This is helpful when you do not wish to maintain a copy of transcript and function calls locally during the call session
and wish to have Retell manage it for you.

<ParamField query="response_type" type="enum<string>" required>
  Differentiate what this event is.

  Available options: `tool_call_invocation`
</ParamField>

<ParamField query="tool_call_id" type="string" required>
  Tool call id, globally unique.
</ParamField>

<ParamField query="name" type="string" required>
  Name of the function in this tool call.
</ParamField>

<ParamField query="arguments" type="string" required>
  Arguments for this tool call, it's a stringified JSON object.
</ParamField>

#### Tool Call Result Event

If you send the tool call invocation and results in the websocket,
Retell would populate the `transcript_with_tool_calls` field in the [Get Call API Response](/api-references/get-call)
after call ends. We would weave the transcript and precisely capture when (at what utterance, which word)
the tool was invoked and what was the result.

If you also set `transcript_with_tool_calls` to true in the [config event](/api-references/llm-websocket#config-event),
Retell would populate the `transcript_with_tool_calls`
field in the [update only event](/api-references/llm-websocket#update-only-event) with the tool call invocations and results.
This is helpful when you do not wish to maintain a copy of transcript and function calls locally during the call session
and wish to have Retell manage it for you.

<ParamField query="response_type" type="enum<string>" required>
  Differentiate what this event is.

  Available options: `tool_call_result`
</ParamField>

<ParamField query="tool_call_id" type="string" required>
  Tool call id, globally unique.
</ParamField>

<ParamField query="content" type="string" required>
  Result of the tool call, can be a string, a stringified json, etc.
</ParamField>

<ParamField query="successful" type="boolean">
  Whether the tool call succeeded. Recorded against the tool call in the transcript.
  Omitting it means the outcome was not reported, not that the tool call failed, so
  set it explicitly if you want to tell the two apart after the call.
</ParamField>

#### Metadata Event

Sometimes you may wish to pass some data from the server where the LLM is running to the frontend during a web call,
for animation purposes or other stuff. It can be challenging to make sure the frontend of the call
can connect to the server where the LLM is running. In this case, you can send metadata events to Retell and Retell will
forward it to the frontend.

See [frontend metadata event](/api-references/audio-websocket#metadata-event) for the forwarded event.

<ParamField query="response_type" type="enum<string>" required>
  Differentiate what this event is.

  Available options: `metadata`
</ParamField>

<ParamField query="metadata" type="object" required>
  You can put anything here that can be json serialized.
</ParamField>

## Sample Events

### Retell -> Your Server Sample Events

<CodeGroup>
  ```json Ping Pong theme={"dark"}
  {
    "interaction_type": "ping_pong",
    "timestamp": 1703302407333
  }
  ```

  ```json Call Details theme={"dark"}
  {
    "interaction_type": "call_details",
    "call": {
      "call_type": "phone_call",
      "from_number": "+12137771234",
      "to_number": "+12137771235",
      "direction": "inbound",
      "call_id": "Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6",
      "agent_id": "oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD",
      "call_status": "registered",
      "metadata": {},
      "retell_llm_dynamic_variables": {
        "customer_name": "John Doe"
      },
      "opt_out_sensitive_data_storage": true
    }
  }
  ```

  ```json Response Required theme={"dark"}
  {
    "interaction_type": "response_required",
    "timestamp": 3,
    "transcript": [
      {
        "role": "agent",
        "content": "Hey how can I help you?",
        "words": [Array]
      },
      {
        "role": "user",
        "content": "Hey. How are you?",
        "words": [Array]
      }
    ]
  }
  ```

  ```json Update Only theme={"dark"}
  {
    "interaction_type": "update_only",
    "transcript": [
      {
        "role": "agent",
        "content": "Hey how can I help you?",
        "words": [Array]
      },
      {
        "role": "user",
        "content": "Hey. How are you?",
        "words": [
          {
            "word": "Hey.",
            "start": 4.375,
            "end": 4.615
          },
          {
            "word": "How",
            "start": 4.615,
            "end": 4.855
          },
          {
            "word": "are",
            "start": 4.855,
            "end": 5.030156
          },
          {
            "word": "you?",
            "start": 5.030156,
            "end": 5.2053127
          }
        ]
      }
    ],
    "turntaking": "agent_turn"
  }
  ```

  ```json Reminder Required theme={"dark"}
  {
    "interaction_type": "reminder_required",
    "transcript": [
      {
        "role": "agent",
        "content": "Hey how can I help you?",
        "words": [Array]
      },
      {
        "role": "user",
        "content": "Hey. How are you?",
        "words": [Array]
      },
      {
        "role": "agent",
        "content": "I'm doing fine. How can I help you?",
        "words": [Array]
      }
    ]
  }
  ```
</CodeGroup>

### Your Server -> Retell Sample Events

<CodeGroup>
  ```json Config theme={"dark"}
  {
    "response_type": "config",
    "config": {
      "auto_reconnect": true,
      "call_details": true
    }
  }
  ```

  ```json Agent Update theme={"dark"}
  {
    "response_type": "update_agent",
    "agent_config": {
      "responsiveness": 0.5,
      "interruption_sensitivity": 0.5,
      "reminder_trigger_ms": 5000,
      "reminder_max_count": 3
    }
  }
  ```

  ```json Ping Pong theme={"dark"}
  {
    "response_type": "ping_pong",
    "timestamp": 1703302407333
  }
  ```

  ```json Response theme={"dark"}
  {
    "response_type": "response",
    "response_id": 3,
    "content": "I'm doing great, ",
    "content_complete": false
  }

  {
    "response_type": "response",
    "response_id": 3,
    "content": "thank you.",
    "content_complete": true
  }

  // Later on, ending the call
  {
    "response_type": "response",
    "response_id": 10,
    "content": "Goodbye.",
    "content_complete": true,
    "end_call": true
  }
  ```

  ```json Agent Interrupt theme={"dark"}
  {
    "response_type": "agent_interrupt",
    "interrupt_id": 1,
    "content": "Please stop right there, do not",
    "content_complete": false,
    "no_interruption_allowed": true
  }

  {
    "response_type": "agent_interrupt",
    "interrupt_id": 1,
    "content": " click on that button yet!",
    "content_complete": true,
    "no_interruption_allowed": true
  }
  ```

  ```json Tool Call Invocation theme={"dark"}
  {
    "response_type": "tool_call_invocation",
    "tool_call_id": "some_id_here",
    "name": "book_appointment",
    "arguments": "{\"date\": \"2022-01-01\", \"time\": \"10:00\"}"
  }
  ```

  ```json Tool Call Result theme={"dark"}
  {
    "response_type": "tool_call_result",
    "tool_call_id": "some_id_here",
    "content": "Appointment booked successfully."
  }
  ```

  ```json Metadata theme={"dark"}
  {
    "response_type": "metadata",
    "metadata": {
      "avatar_emotion": "Angry",
      "user_id": "1234"
    }
  }
  ```
</CodeGroup>
