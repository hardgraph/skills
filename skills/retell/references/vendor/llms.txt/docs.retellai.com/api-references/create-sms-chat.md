> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Outbound SMS

> Start an outbound SMS chat conversation with a phone number using the specified agent. The agent must be configured for chat mode. The initial SMS message will be automatically generated and sent based on the agent's configuration.



## OpenAPI

````yaml openapi-final post /create-sms-chat
openapi: 3.0.3
info:
  title: Retell SDK
  version: 3.0.0
  x-retell-spec-revision: 2026-08-01-ad84c11
  contact:
    name: Retell Support
    url: https://www.retellai.com/
    email: support@retellai.com
  license:
    name: Apache 2.0
    url: https://www.apache.org/licenses/LICENSE-2.0.html
servers:
  - url: https://api.retellai.com
    description: The production server.
security:
  - api_key: []
paths:
  /create-sms-chat:
    post:
      description: >-
        Start an outbound SMS chat conversation with a phone number using the
        specified agent. The agent must be configured for chat mode. The initial
        SMS message will be automatically generated and sent based on the
        agent's configuration.
      operationId: createSmsChat
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - from_number
                - to_number
              properties:
                from_number:
                  type: string
                  minLength: 1
                  description: >-
                    The phone number to send SMS from in E.164 format. Must be a
                    number purchased from Retell or imported to Retell with SMS
                    capability.
                  example: '+12137771234'
                to_number:
                  type: string
                  minLength: 1
                  description: The phone number to send SMS to in E.164 format
                  example: '+14155551234'
                override_agent_id:
                  type: string
                  minLength: 1
                  example: oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD
                  description: >-
                    For this particular chat, override the agent used with this
                    agent id. This does not bind the agent to this number, this
                    is for one time override.
                override_agent_version:
                  $ref: '#/components/schemas/AgentVersionReference'
                  description: >-
                    For this particular chat, override the agent version used
                    with this version. This does not bind the agent version to
                    this number, this is for one time override.
                metadata:
                  type: object
                  description: >-
                    An arbitrary object for storage purpose only. You can put
                    anything here like your internal customer id associated with
                    the chat. Not used for processing. You can later get this
                    field from the chat object.
                retell_llm_dynamic_variables:
                  type: object
                  additionalProperties:
                    type: string
                  example:
                    customer_name: John Doe
                  description: >-
                    Add optional dynamic variables in key value pairs of string
                    that injects into your Response Engine prompt and tool
                    description. Only applicable for Response Engine.
      responses:
        '200':
          description: SMS chat created and initial message sent successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ChatResponse'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '402':
          $ref: '#/components/responses/PaymentRequired'
        '422':
          $ref: '#/components/responses/UnprocessableContent'
        '429':
          $ref: '#/components/responses/TooManyRequests'
        '500':
          $ref: '#/components/responses/InternalServerError'
      x-codeSamples:
        - lang: JavaScript
          source: |-
            import Retell from 'retell-sdk';

            const client = new Retell({
              apiKey: process.env['RETELL_API_KEY'], // This is the default and can be omitted
            });

            const chatResponse = await client.chat.createSMSChat({
              from_number: '+12137771234',
              to_number: '+14155551234',
            });

            console.log(chatResponse.agent_id);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            chat_response = client.chat.create_sms_chat(
                from_number="+12137771234",
                to_number="+14155551234",
            )
            print(chat_response.agent_id)
components:
  schemas:
    AgentVersionReference:
      oneOf:
        - type: string
          minLength: 1
          maxLength: 20
          pattern: >-
            ^(latest|latest_published|(?!(?:latest|latest_published|v\d+)$)[a-z][a-z0-9_-]{0,19})$
          example: latest_published
        - type: integer
          minimum: 0
          example: 1
      description: >-
        Agent version reference. Supports a numeric version (for example 3) or a
        tag/environment name (for example "prod"). The string "latest" resolves
        to the most recently created version (the largest version number), and
        "latest_published" resolves to the most recently published version. When
        a tag is provided, resolution uses that exact tag assignment (including
        its dynamic variables). If the tag exists but is currently unassigned,
        it resolves to latest. When a numeric version, latest, or
        latest_published is provided, resolution applies dynamic variables from
        the preferred tag for that resolved version (most recently assigned), if
        any.
    ChatResponse:
      type: object
      required:
        - chat_id
        - agent_id
        - chat_status
      properties:
        chat_id:
          type: string
          example: Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6
          description: Unique id of the chat.
        agent_id:
          type: string
          example: oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD
          description: Corresponding chat agent id of this chat.
        version:
          type: integer
          example: 1
          description: The version of the agent
          nullable: true
        retell_llm_dynamic_variables:
          type: object
          additionalProperties:
            type: string
          example:
            customer_name: John Doe
          description: >-
            Add optional dynamic variables in key value pairs of string that
            injects into your Response Engine prompt and tool description. Only
            applicable for Response Engine.
        collected_dynamic_variables:
          type: object
          additionalProperties:
            type: string
          example:
            last_node_name: Test node
          description: >-
            Dynamic variables collected from the chat. Only available after the
            chat ends.
        chat_status:
          type: string
          enum:
            - ongoing
            - ended
            - error
          example: ongoing
          description: >
            Status of chat.


            - `ongoing`: Chat session is ongoing, chat agent can receive new
            message and generate response.

            - `ended`: Chat session has ended, and no longer can generate new
            response.

            - `error`: Chat encountered error.
        chat_type:
          type: string
          enum:
            - api_chat
            - sms_chat
          example: api_chat
          description: Type of the chat
        custom_attributes:
          type: object
          additionalProperties:
            oneOf:
              - type: string
              - type: number
              - type: boolean
          description: Custom attributes for the chat
        start_timestamp:
          type: integer
          example: 1703302407333
          description: >-
            Begin timestamp (milliseconds since epoch) of the chat. Available
            after chat starts.
        end_timestamp:
          type: integer
          example: 1703302428855
          description: >-
            End timestamp (milliseconds since epoch) of the chat. Available
            after chat ends.
          nullable: true
        transcript:
          type: string
          example: |
            Agent: hi how are you doing?
            User: Doing pretty well. How are you?
            Agent: That's great to hear! I'm doing well too, thanks! What's up?
            User: I don't have anything in particular.
            Agent: Got it, just checking in!
            User: Alright. See you.
            Agent: have a nice day
          description: Transcription of the chat.
        message_with_tool_calls:
          type: array
          items:
            $ref: '#/components/schemas/MessageOrToolCall'
          description: Transcript of the chat weaved with tool call invocation and results.
        metadata:
          type: object
          description: >-
            An arbitrary object for storage purpose only. You can put anything
            here like your internal customer id associated with the chat. Not
            used for processing. You can later get this field from the chat
            object.
        chat_cost:
          type: object
          properties:
            product_costs:
              type: array
              description: List of products with their unit prices and costs in cents
              items:
                $ref: '#/components/schemas/ProductCost'
            combined_cost:
              type: number
              description: Combined cost of all individual costs in cents
              example: 70
        chat_analysis:
          $ref: '#/components/schemas/ChatAnalysis'
          description: >-
            Post chat analysis that includes information such as sentiment,
            status, summary, and custom defined data to extract. Available after
            chat ends. Subscribe to `chat_analyzed` webhook event type to
            receive it once ready.
    MessageOrToolCall:
      oneOf:
        - $ref: '#/components/schemas/Message'
        - $ref: '#/components/schemas/ToolCallInvocationMessage'
        - $ref: '#/components/schemas/ToolCallResultMessage'
        - $ref: '#/components/schemas/NodeTransitionMessage'
        - $ref: '#/components/schemas/StateTransitionMessage'
        - $ref: '#/components/schemas/InjectedMessage'
        - $ref: '#/components/schemas/SmsMessage'
    ProductCost:
      type: object
      required:
        - product
        - cost
      properties:
        product:
          type: string
          description: Product name that has a cost associated with it.
          example: elevenlabs_tts
        unit_price:
          type: number
          description: Unit price of the product in cents per second.
          example: 1
        cost:
          type: number
          description: Cost for the product in cents for the duration of the call.
          example: 60
        is_transfer_leg_cost:
          type: boolean
          description: True if this cost item is for a transfer segment.
    ChatAnalysis:
      type: object
      properties:
        chat_summary:
          type: string
          example: >-
            The agent messages user to ask question about his purchase inquiry.
            The agent asked several questions regarding his preference and asked
            if user would like to book an appointment. The user happily agreed
            and scheduled an appointment next Monday 10am.
          description: A high level summary of the chat.
        user_sentiment:
          type: string
          enum:
            - Negative
            - Positive
            - Neutral
            - Unknown
          example: Positive
          description: Sentiment of the user in the chat.
        chat_successful:
          type: boolean
          example: true
          description: >-
            Whether the agent seems to have a successful chat with the user,
            where the agent finishes the task, and the call was complete without
            being cutoff.
        custom_analysis_data:
          type: object
          description: >-
            Custom analysis data that was extracted based on the schema defined
            in chat agent post chat analysis data. Can be empty if nothing is
            specified.
    Message:
      allOf:
        - $ref: '#/components/schemas/MessageBase'
        - required:
            - message_id
            - created_timestamp
    ToolCallInvocationMessage:
      allOf:
        - $ref: '#/components/schemas/ToolCallInvocationMessageBase'
        - required:
            - message_id
            - created_timestamp
    ToolCallResultMessage:
      allOf:
        - $ref: '#/components/schemas/ToolCallResultMessageBase'
        - required:
            - message_id
            - created_timestamp
    NodeTransitionMessage:
      allOf:
        - $ref: '#/components/schemas/NodeTransitionMessageBase'
        - required:
            - message_id
            - created_timestamp
    StateTransitionMessage:
      allOf:
        - $ref: '#/components/schemas/StateTransitionMessageBase'
        - required:
            - message_id
            - created_timestamp
    InjectedMessage:
      allOf:
        - $ref: '#/components/schemas/InjectedMessageBase'
        - required:
            - message_id
            - created_timestamp
    SmsMessage:
      allOf:
        - $ref: '#/components/schemas/SmsMessageBase'
        - required:
            - message_id
            - created_timestamp
    MessageBase:
      type: object
      required:
        - role
        - content
      properties:
        message_id:
          type: string
          example: Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6
          description: Unique id of the message
        role:
          type: string
          enum:
            - agent
            - user
          description: Documents whether this message is sent by agent or user.
          example: agent
        content:
          type: string
          description: Content of the message
          example: hi how are you doing?
        created_timestamp:
          type: integer
          description: Create timestamp of the message
          example: 1703302428855
    ToolCallInvocationMessageBase:
      type: object
      required:
        - role
        - tool_call_id
        - name
        - arguments
      properties:
        message_id:
          type: string
          example: Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6
          description: Unique id of the message
        role:
          type: string
          enum:
            - tool_call_invocation
          description: This is a tool call invocation.
        tool_call_id:
          type: string
          description: Tool call id, globally unique.
        name:
          type: string
          description: Name of the function in this tool call.
        arguments:
          type: string
          description: Arguments for this tool call, it's a stringified JSON object.
        thought_signature:
          type: string
          description: >-
            Optional thought signature from Google Gemini thinking models. This
            is used internally to maintain reasoning chain in multi-turn
            function calling.
        created_timestamp:
          type: integer
          description: Create timestamp of the message
          example: 1703302428855
    ToolCallResultMessageBase:
      type: object
      required:
        - role
        - tool_call_id
        - content
      properties:
        message_id:
          type: string
          example: Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6
          description: Unique id of the message
        role:
          type: string
          enum:
            - tool_call_result
          description: This is the result of a tool call.
        tool_call_id:
          type: string
          description: Tool call id, globally unique.
        content:
          type: string
          description: Result of the tool call, can be a string, a stringified json, etc.
        successful:
          type: boolean
          description: Whether the tool call was successful.
        created_timestamp:
          type: integer
          description: Create timestamp of the message
          example: 1703302428855
    NodeTransitionMessageBase:
      type: object
      required:
        - role
      properties:
        message_id:
          type: string
          example: Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6
          description: Unique id of the message
        role:
          type: string
          enum:
            - node_transition
          description: This is a node transition.
        former_node_id:
          type: string
          description: Former node id
        former_node_name:
          type: string
          description: Former node name
        new_node_id:
          type: string
          description: New node id
        new_node_name:
          type: string
          description: New node name
        transition_type:
          type: string
          enum:
            - global
            - global_go_back
            - interrupt_go_back
            - normal
          description: >-
            How this node was reached. "global" means a global node transition,
            "global_go_back" means returning from a global node,
            "interrupt_go_back" means going back due to user interruption, and
            "normal" means a regular edge transition.
        created_timestamp:
          type: integer
          description: Create timestamp of the message
          example: 1703302428855
    StateTransitionMessageBase:
      type: object
      required:
        - role
      properties:
        message_id:
          type: string
          example: Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6
          description: Unique id of the message
        role:
          type: string
          enum:
            - state_transition
          description: This is a state transition.
        former_state_name:
          type: string
          description: Former state name
        new_state_name:
          type: string
          description: New state name
        created_timestamp:
          type: integer
          description: Create timestamp of the message
          example: 1703302428855
    InjectedMessageBase:
      type: object
      required:
        - role
        - content
      properties:
        message_id:
          type: string
          example: Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6
          description: Unique id of the message
        role:
          type: string
          enum:
            - injected
          description: >-
            External context injected into the conversation via the
            update-live-call API. Not spoken by either party.
        content:
          type: string
          description: The injected context text.
          example: Customer just opened a support ticket about billing.
        created_timestamp:
          type: integer
          description: Create timestamp of the message
          example: 1703302428855
    SmsMessageBase:
      type: object
      required:
        - role
        - content
      properties:
        message_id:
          type: string
          example: Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6
          description: Unique id of the message
        role:
          type: string
          enum:
            - sms
          description: >-
            SMS message exchanged during the call (for example received from the
            user). Woven into the transcript and shown to the agent, but not
            part of the spoken conversation.
        content:
          type: string
          description: Text content of the SMS message.
          example: Here is the photo you asked for.
        multimedia:
          type: array
          items:
            $ref: '#/components/schemas/SmsMultimediaItem'
          description: >-
            Multimedia attachments (MMS). Display only; not relayed into the
            spoken conversation.
        created_timestamp:
          type: integer
          description: Create timestamp of the message
          example: 1703302428855
    SmsMultimediaItem:
      type: object
      required:
        - url
      properties:
        url:
          type: string
          description: URL of the multimedia attachment.
        summary:
          type: string
          description: Optional textual summary of the attachment.
  responses:
    BadRequest:
      description: Bad Request
      content:
        application/json:
          schema:
            type: object
            properties:
              status:
                type: string
                enum:
                  - error
              message:
                type: string
                example: Invalid request format, please check API reference.
    Unauthorized:
      description: Unauthorized
      content:
        application/json:
          schema:
            type: object
            properties:
              status:
                type: string
                enum:
                  - error
              message:
                type: string
                example: API key is missing or invalid.
    PaymentRequired:
      description: Payment Required
      content:
        application/json:
          schema:
            type: object
            properties:
              status:
                type: string
                enum:
                  - error
              message:
                type: string
                example: Trial has ended, please add payment method.
    UnprocessableContent:
      description: Unprocessable Content
      content:
        application/json:
          schema:
            type: object
            properties:
              status:
                type: string
                enum:
                  - error
              message:
                type: string
                example: Cannot find requested asset under given api key.
    TooManyRequests:
      description: Too Many Requests
      content:
        application/json:
          schema:
            type: object
            properties:
              status:
                type: string
                enum:
                  - error
              message:
                type: string
                example: Account rate limited, please throttle your requests.
    InternalServerError:
      description: Internal Server Error
      content:
        application/json:
          schema:
            type: object
            properties:
              status:
                type: string
                enum:
                  - error
              message:
                type: string
                example: An unexpected server error occurred.
  securitySchemes:
    api_key:
      type: http
      scheme: bearer
      bearerFormat: string
      description: >-
        Authentication header containing API key (find it in dashboard). The
        format is "Bearer YOUR_API_KEY"

````