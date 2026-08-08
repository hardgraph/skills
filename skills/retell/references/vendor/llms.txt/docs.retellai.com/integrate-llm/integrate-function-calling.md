> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Add function calling

> Give a Retell custom LLM agent tools to book appointments, transfer to a human, press IVR digits, and end calls, all logged in the transcript.

Callers want to book something, reach a person, or hear an answer from your database. Function calling is how the model asks your code to do that work.

Keep two layers separate:

* **Your LLM's function calling** decides *when* to act. This is your provider's tool-use API: you define the tools, the model picks one. Retell isn't involved.
* **Retell's response fields** carry out the actions Retell owns: ending the call, transferring it, pressing digits. You attach these to a `response` event.

Everything else, like querying your database or hitting your booking API, is just code you run.

## A concrete example

Bright Smile Dental runs an appointment line. The agent needs to book into the practice management system while the caller waits, hand off to the front desk when someone's upset, and hang up cleanly when the caller says goodbye. That's three tools: `book_appointment`, `transfer_to_front_desk`, and `end_call`.

## Actions Retell performs for you

Set these fields on a `response` event. Retell runs the action **after the content in that response has been fully spoken**, so the field pairs naturally with a closing line.

| Field                       | Type    | What happens                                                                                                                                                                        |
| --------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `end_call`                  | boolean | Retell hangs up once the content finishes. If the caller interrupts before the agent finishes speaking, the hangup is discarded.                                                    |
| `transfer_number`           | string  | Cold-transfers the call to that number after the content finishes. Works for Retell numbers and imported numbers. On custom telephony via dial-to-SIP, implement transfer yourself. |
| `show_transferee_as_caller` | boolean | On transfer, shows the original caller's number to the transferee instead of the Retell number. Defaults to `false`.                                                                |
| `digit_to_press`            | string  | Sends DTMF tones for the given digits after the content finishes. Usually paired with empty content.                                                                                |
| `no_interruption_allowed`   | boolean | The caller can't interrupt this content. Retell drops interruption sensitivity to 0 for the duration, then restores your agent's setting.                                           |

<Warning>
  `end_call`, `transfer_number`, and `digit_to_press` are mutually exclusive. Retell runs at most one per `response_id`, and `end_call` takes precedence, then `transfer_number`. Set only the one you want.
</Warning>

## End the call

The simplest useful tool. Define a function with a `message` parameter so the model supplies a goodbye line. Most providers return either a tool call or text, not both, so without that parameter the agent hangs up in silence.

This replaces `draftResponse` from [connecting your LLM](/integrate-llm/integrate-llm#stream-the-response). It keeps that version's two guarantees, and both matter more now than they did before: `isStale` stops work on a response Retell has already discarded, and the `finally` block closes the response even when the model errors. Your OpenAI client and prompt constants don't change.

<CodeGroup>
  ```typescript llm.ts theme={"dark"}
  const tools: OpenAI.Chat.ChatCompletionTool[] = [
    {
      type: "function",
      function: {
        name: "end_call",
        description: "End the call. Only when the caller has clearly said goodbye.",
        parameters: {
          type: "object",
          properties: {
            message: {
              type: "string",
              description: "What to say before hanging up.",
            },
          },
          required: ["message"],
        },
      },
    },
  ];

  export interface ToolResult {
    id: string;
    name: string;
    arguments: string;
    result: string;
  }

  function buildMessages(
    request: ResponseRequiredRequest,
    toolResult?: ToolResult,
  ) {
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

    // Replay the tool call and its result so the model answers from it.
    if (toolResult) {
      messages.push({
        role: "assistant",
        content: null,
        tool_calls: [
          {
            id: toolResult.id,
            type: "function",
            function: {
              name: toolResult.name,
              arguments: toolResult.arguments,
            },
          },
        ],
      });
      messages.push({
        role: "tool",
        tool_call_id: toolResult.id,
        content: toolResult.result,
      });
    }

    return messages;
  }

  export async function draftResponse(
    request: ResponseRequiredRequest,
    ws: WebSocket,
    isStale: () => boolean,
    toolResult?: ToolResult,
  ) {
    // Set once this call has closed the response, or abandoned a stale one.
    let responseClosed = false;

    try {
      const stream = await openai.chat.completions.create({
        model: "gpt-4.1-mini",
        messages: buildMessages(request, toolResult),
        stream: true,
        temperature: 0,
        max_tokens: 200,
        tools,
      });

      let toolCallId = "";
      let toolName = "";
      let toolArguments = "";

      for await (const chunk of stream) {
        if (isStale()) {
          responseClosed = true;
          stream.controller.abort();
          return;
        }

        const delta = chunk.choices[0]?.delta;
        if (!delta) continue;

        const toolCall = delta.tool_calls?.[0];

        if (toolCall) {
          // The id and name arrive once, then arguments stream in as JSON fragments.
          if (toolCall.id) toolCallId = toolCall.id;
          if (toolCall.function?.name) toolName = toolCall.function.name;
          toolArguments += toolCall.function?.arguments ?? "";
        } else if (delta.content) {
          ws.send(
            JSON.stringify({
              response_type: "response",
              response_id: request.response_id,
              content: delta.content,
              content_complete: false,
            }),
          );
        }
      }

      if (toolName === "end_call") {
        const { message } = JSON.parse(toolArguments);
        ws.send(
          JSON.stringify({
            response_type: "response",
            response_id: request.response_id,
            content: message,
            content_complete: true,
            end_call: true,
          }),
        );
        responseClosed = true;
      }
    } catch (err) {
      console.error("LLM stream failed:", err);
    } finally {
      // Close the response even when the model errored, or the agent stays silent.
      if (!responseClosed) {
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
  import json

  TOOLS = [
      {
          "type": "function",
          "function": {
              "name": "end_call",
              "description": "End the call. Only when the caller has clearly said goodbye.",
              "parameters": {
                  "type": "object",
                  "properties": {
                      "message": {
                          "type": "string",
                          "description": "What to say before hanging up.",
                      },
                  },
                  "required": ["message"],
              },
          },
      },
  ]


  async def draft_response(request: ResponseRequiredRequest, is_stale: Callable[[], bool]):
      response_closed = False

      try:
          stream = await openai.chat.completions.create(
              model="gpt-4.1-mini",
              messages=build_messages(request),
              stream=True,
              temperature=0,
              max_tokens=200,
              tools=TOOLS,
          )

          tool_call_id = ""
          tool_name = ""
          tool_arguments = ""

          async for chunk in stream:
              if is_stale():
                  await stream.close()
                  return

              if not chunk.choices:
                  continue

              delta = chunk.choices[0].delta

              if delta.tool_calls:
                  tool_call = delta.tool_calls[0]
                  # The id and name arrive once, then arguments stream in as JSON fragments.
                  if tool_call.id:
                      tool_call_id = tool_call.id
                  if tool_call.function and tool_call.function.name:
                      tool_name = tool_call.function.name
                  if tool_call.function:
                      tool_arguments += tool_call.function.arguments or ""

              elif delta.content:
                  yield ResponseResponse(
                      response_id=request.response_id,
                      content=delta.content,
                      content_complete=False,
                  )

          if tool_name == "end_call":
              yield ResponseResponse(
                  response_id=request.response_id,
                  content=json.loads(tool_arguments)["message"],
                  content_complete=True,
                  end_call=True,
              )
              response_closed = True
      except Exception as err:
          print(f"LLM stream failed: {err}")

      # Close the response even when the model errored, or the agent stays silent.
      if not response_closed:
          yield ResponseResponse(
              response_id=request.response_id,
              content="",
              content_complete=True,
          )
  ```
</CodeGroup>

Set `temperature: 0` when the model has tools available. Lower temperature meaningfully improves how reliably the model picks the right function and formats its arguments.

## Do work while the agent talks

Ending a call is easy because nothing happens afterward. Booking an appointment is the harder, more common shape: say something so the caller isn't listening to silence, do the work, then say what happened.

`content_complete` is what makes this work. Send the holding line with `content_complete: false`. The agent speaks it and Retell keeps the response open, waiting for more. Do your work, then feed the result back to the model and stream the real answer under the same `response_id`.

The rest of this section is Node.js, but the sequence of events is the same in any language.

Declare the tool alongside `end_call`. Give it a `message` parameter for the holding line, the same way `end_call` carries the goodbye:

```typescript llm.ts theme={"dark"}
{
  type: "function",
  function: {
    name: "book_appointment",
    description: "Book or move an appointment once you have both a date and a time.",
    parameters: {
      type: "object",
      properties: {
        date: { type: "string", description: "ISO date, for example 2026-08-04." },
        time: { type: "string", description: "24-hour time, for example 10:00." },
        message: {
          type: "string",
          description: "A short line to say while the booking runs.",
        },
      },
      required: ["date", "time", "message"],
    },
  },
},
```

Your booking function should return a failure as data rather than throwing. The model can offer another slot if it's told the slot was taken; it can't do anything with an exception.

```typescript llm.ts theme={"dark"}
async function bookAppointment(date: string, time: string) {
  const res = await fetch("https://your-api.example.com/appointments", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ date, time }),
  });

  if (!res.ok) {
    return { status: "failed", reason: "That slot is no longer available." };
  }
  return { status: "confirmed", ...(await res.json()) };
}
```

Then handle the tool call next to the `end_call` branch in `draftResponse`:

```typescript llm.ts theme={"dark"}
if (toolName === "book_appointment") {
  const args = JSON.parse(toolArguments);

  // Speak the holding line, but keep the response open. The trailing space keeps
  // the follow-up from running into it in the transcript.
  ws.send(
    JSON.stringify({
      response_type: "response",
      response_id: request.response_id,
      content: `${args.message} `,
      content_complete: false,
    }),
  );

  // Skip the write if Retell already superseded this response. This only catches a
  // supersede that has already landed, so bookAppointment still needs to be idempotent.
  if (isStale()) {
    responseClosed = true;
    return;
  }

  // Tell Retell a tool is running, so it shows up in the transcript.
  ws.send(
    JSON.stringify({
      response_type: "tool_call_invocation",
      tool_call_id: toolCallId,
      name: "book_appointment",
      arguments: toolArguments,
    }),
  );

  const result = await bookAppointment(args.date, args.time);

  ws.send(
    JSON.stringify({
      response_type: "tool_call_result",
      tool_call_id: toolCallId,
      content: JSON.stringify(result),
      successful: result.status === "confirmed",
    }),
  );

  // Feed the result back to the model and stream the follow-up. The nested call
  // closes the response in its own finally block.
  responseClosed = true;
  await draftResponse(request, ws, isStale, {
    id: toolCallId,
    name: "book_appointment",
    arguments: toolArguments,
    result: JSON.stringify(result),
  });
  return;
}
```

While a response is open, Retell waits rather than filling the gap itself. It won't ask for a reminder or a new response until you send `content_complete: true`, so the only thing that can cut your work short is the caller speaking.

## Transfer and press digits

Both are `response` fields, so they follow the same pattern as `end_call`: the agent finishes speaking, then Retell acts.

<CodeGroup>
  ```typescript Transfer to a human theme={"dark"}
  ws.send(
    JSON.stringify({
      response_type: "response",
      response_id: request.response_id,
      content: "Let me get you to the front desk. One moment.",
      content_complete: true,
      transfer_number: "+14155550123",
      show_transferee_as_caller: true,
    }),
  );
  ```

  ```typescript Press IVR digits theme={"dark"}
  ws.send(
    JSON.stringify({
      response_type: "response",
      response_id: request.response_id,
      content: "",
      content_complete: true,
      digit_to_press: "1",
    }),
  );
  ```
</CodeGroup>

Transfers initiated this way are cold transfers: the call is handed off and your agent drops out. For warm transfers or transfer logic on your own telephony, run it from your server.

## Record tool calls in the transcript

Retell doesn't see your tool calls, so by default they're invisible in the call record. Send `tool_call_invocation` and `tool_call_result` events and Retell weaves them into the transcript at the exact word where they happened.

That pays off in two places:

* **After the call**, `transcript_with_tool_calls` in the [Get Call API response](/api-references/get-call) shows what the agent did and when, not just what it said. This is what makes a custom LLM call debuggable.
* **During the call**, if you also set `transcript_with_tool_calls: true` in your `config` event, Retell includes the woven transcript in every `update_only` and `response_required` event. Retell then keeps that history for you, though you still map it into your provider's message format yourself.

Send the invocation as the tool starts and the result when it returns, using the same `tool_call_id` for both:

```json theme={"dark"}
{
  "response_type": "tool_call_invocation",
  "tool_call_id": "call_a1b2c3",
  "name": "book_appointment",
  "arguments": "{\"date\": \"2026-08-14\", \"time\": \"10:00\"}"
}

{
  "response_type": "tool_call_result",
  "tool_call_id": "call_a1b2c3",
  "content": "Booked for Aug 14 at 10am with Dr. Chen.",
  "successful": true
}
```

`arguments` is a stringified JSON object, and `content` is a plain string, so stringify structured results yourself. `successful` is optional and marks the outcome in the transcript. Leaving it out doesn't mean the call failed, it means you didn't say, so set it explicitly when you care about telling a failed tool call from a successful one after the fact.

## Going to production

The examples above are deliberately minimal. A few things they don't handle that a live agent needs:

* **Don't double-book.** This happens on ordinary calls: Retell asks for a response, discards it when the caller keeps talking, and asks again, so your tool runs twice for a single request. **An idempotency key is the only real fix.** The `isStale()` check above helps only when the newer request has already arrived, and a fast tool commits its write a second or two before that happens. Derive the key from the call ID plus the tool arguments, which are usually identical across the duplicate calls, and make the repeat a no-op.
* **Track state, don't rely on the prompt.** Once past two or three tools, a state machine that controls which tools and prompt are active at each step beats hoping the model reads the transcript correctly. See [best practices](/integrate-llm/llm-best-practice).
* **Report failures out loud.** When a tool errors, send the failure back to the model as a tool result, and mark it with `successful: false`, so the agent offers an alternative instead of going quiet.

## FAQ

<AccordionGroup>
  <Accordion title="Why does the agent hang up without saying goodbye?">
    You sent `end_call: true` with empty content, or the model returned a tool call with no `message` parameter to speak. Retell ends the call as soon as the content in that response finishes, and empty content finishes immediately. Give the tool a `message` parameter and put it in `content`.
  </Accordion>

  <Accordion title="Why was my end_call or transfer ignored?">
    Most likely the caller interrupted before the agent finished speaking, which discards the pending action. Otherwise, check that you didn't set both `end_call` and `transfer_number` on the same response; only `end_call` runs. Setting `no_interruption_allowed: true` on the closing line prevents the interruption case.
  </Accordion>

  <Accordion title="My tool ran twice for one caller request. Why?">
    Retell asked for a response, discarded it when the caller kept talking, and asked again with a new `response_id`. Your code ran the tool on both, usually with identical arguments. This is the most common way a custom LLM agent double-books, and no setting turns it off. Make the write idempotent, keyed on the call ID plus the arguments. Checking `response_id` first helps, but only when the newer request has already arrived.
  </Accordion>

  <Accordion title="Do I have to send tool_call_invocation and tool_call_result?">
    No, they're optional bookkeeping. Skip them and your tools still run; they just won't appear in the call transcript, which makes debugging a failed call much harder. Send them.
  </Accordion>
</AccordionGroup>

## Next steps

* [Best practices](/integrate-llm/llm-best-practice) for keeping latency low once tools are in the loop
* [Troubleshooting](/integrate-llm/troubleshooting) when the agent goes silent or the call drops
* [LLM WebSocket reference](/api-references/llm-websocket) for every field on every event
