> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# List Chats

> List chats with unified cursor pagination response.



## OpenAPI

````yaml openapi-final post /v3/list-chats
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
  /v3/list-chats:
    post:
      description: List chats with unified cursor pagination response.
      operationId: listChats
      requestBody:
        required: false
        content:
          application/json:
            schema:
              type: object
              properties:
                filter_criteria:
                  $ref: '#/components/schemas/ChatFilter'
                  description: Filter criteria for chats to retrieve.
                sort_order:
                  type: string
                  enum:
                    - ascending
                    - descending
                  default: descending
                  description: >-
                    Sort chats by `start_timestamp` in ascending or descending
                    order.
                limit:
                  type: integer
                  default: 50
                  maximum: 1000
                  description: Maximum number of chats to return.
                skip:
                  type: integer
                  minimum: 0
                  default: 0
                  description: Number of records to skip for pagination.
                pagination_key:
                  type: string
                  description: Opaque pagination cursor from a previous response.
                include_total:
                  type: boolean
                  default: false
                  description: >-
                    Whether to include `total` (count of all chats matching
                    `filter_criteria`, ignoring `limit`/`skip`/`pagination_key`)
                    in the response. Defaults to false. Each enabled request
                    triggers an additional aggregate query, so opt in only when
                    the total is needed.
              not:
                required:
                  - skip
                  - pagination_key
      responses:
        '200':
          description: Successfully retrieved chats.
          content:
            application/json:
              schema:
                allOf:
                  - $ref: '#/components/schemas/PaginatedResponseBase'
                  - type: object
                    properties:
                      items:
                        type: array
                        items:
                          $ref: '#/components/schemas/V3ChatResponse'
                      total:
                        type: integer
                        description: >-
                          Total number of chats matching `filter_criteria`. Only
                          present when `include_total` is true.
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
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

            const chats = await client.chat.list();

            console.log(chats.has_more);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            chats = client.chat.list()
            print(chats.has_more)
components:
  schemas:
    ChatFilter:
      type: object
      description: >-
        Filter criteria for chats. All conditions are implicitly connected with
        AND.
      properties:
        agent:
          type: array
          items:
            $ref: '#/components/schemas/AgentFilter'
          description: Filter by agent(s). Agent filters are connected by OR.
        agent_tag:
          allOf:
            - $ref: '#/components/schemas/EnumFilter'
            - description: Filter by agent environment tag(s) (e.g. "prod", "staging").
        chat_id:
          $ref: '#/components/schemas/StringFilter'
          description: Filter by chat ID.
        chat_status:
          allOf:
            - $ref: '#/components/schemas/EnumFilter'
            - description: Filter by chat status.
              properties:
                value:
                  items:
                    enum:
                      - ongoing
                      - ended
                      - error
        disconnection_reason:
          allOf:
            - $ref: '#/components/schemas/EnumFilter'
            - description: Filter by disconnection reason.
              properties:
                value:
                  items:
                    $ref: '#/components/schemas/DisconnectionReason'
        user_sentiment:
          allOf:
            - $ref: '#/components/schemas/EnumFilter'
            - description: Filter by user sentiment.
              properties:
                value:
                  items:
                    enum:
                      - Negative
                      - Positive
                      - Neutral
                      - Unknown
        chat_successful:
          $ref: '#/components/schemas/BooleanFilter'
          description: Filter by whether the chat was successful.
        start_timestamp:
          oneOf:
            - $ref: '#/components/schemas/NumberFilter'
            - $ref: '#/components/schemas/RangeFilter'
          description: Filter by chat start timestamp (epoch ms).
        end_timestamp:
          oneOf:
            - $ref: '#/components/schemas/NumberFilter'
            - $ref: '#/components/schemas/RangeFilter'
          description: Filter by chat end timestamp (epoch ms).
        duration_ms:
          oneOf:
            - $ref: '#/components/schemas/NumberFilter'
            - $ref: '#/components/schemas/RangeFilter'
          description: Filter by chat duration in milliseconds.
        combined_cost:
          oneOf:
            - $ref: '#/components/schemas/NumberFilter'
            - $ref: '#/components/schemas/RangeFilter'
          description: Filter by combined cost of the chat.
        custom_analysis_data:
          type: array
          items:
            $ref: '#/components/schemas/CustomFieldFilter'
          description: Filter by custom analysis data fields.
        custom_attributes:
          type: array
          items:
            $ref: '#/components/schemas/CustomFieldFilter'
          description: Filter by custom attributes fields.
    PaginatedResponseBase:
      type: object
      properties:
        pagination_key:
          type: string
          description: Pagination key for the next page.
        has_more:
          type: boolean
          description: Whether more results are available.
    V3ChatResponse:
      allOf:
        - $ref: '#/components/schemas/ChatResponse'
        - type: object
          description: V3 list chats response. Transcript fields are intentionally omitted.
          not:
            anyOf:
              - required:
                  - transcript
              - required:
                  - message_with_tool_calls
              - required:
                  - scrubbed_message_with_tool_calls
              - required:
                  - pre_session_message_with_tool_calls
              - required:
                  - scrubbed_pre_session_message_with_tool_calls
              - required:
                  - post_session_message_with_tool_calls
              - required:
                  - scrubbed_post_session_message_with_tool_calls
    AgentFilter:
      type: object
      required:
        - agent_id
      properties:
        agent_id:
          type: string
          minLength: 1
          description: The agent ID to filter on.
        version:
          type: array
          items:
            type: number
          description: >-
            Specific versions to filter on. If not provided, all versions are
            included.
    EnumFilter:
      type: object
      required:
        - type
        - op
        - value
      properties:
        type:
          type: string
          enum:
            - enum
        op:
          type: string
          enum:
            - in
          description: 'in: value is one of the listed values'
        value:
          type: array
          items:
            type: string
    StringFilter:
      type: object
      required:
        - type
        - op
        - value
      properties:
        type:
          type: string
          enum:
            - string
        op:
          type: string
          enum:
            - eq
            - ne
            - sw
            - ew
            - co
          description: >-
            eq: equal, ne: not equal, sw: starts with, ew: ends with, co:
            contains
        value:
          type: string
    DisconnectionReason:
      type: string
      enum:
        - user_hangup
        - agent_hangup
        - call_transfer
        - voicemail_reached
        - ivr_reached
        - inactivity
        - max_duration_reached
        - concurrency_limit_reached
        - no_concurrency_fallback
        - no_valid_payment
        - scam_detected
        - dial_busy
        - dial_failed
        - dial_no_answer
        - invalid_destination
        - telephony_provider_permission_denied
        - telephony_provider_unavailable
        - sip_routing_error
        - marked_as_spam
        - user_declined
        - error_llm_websocket_open
        - error_llm_websocket_lost_connection
        - error_llm_websocket_runtime
        - error_llm_websocket_corrupt_payload
        - error_no_audio_received
        - error_asr
        - error_retell
        - error_unknown
        - error_user_not_joined
        - registered_call_timeout
        - transfer_bridged
        - transfer_cancelled
        - manual_stopped
        - call_take_over
    BooleanFilter:
      type: object
      required:
        - type
        - op
        - value
      properties:
        type:
          type: string
          enum:
            - boolean
        op:
          type: string
          enum:
            - eq
        value:
          type: boolean
    NumberFilter:
      type: object
      required:
        - type
        - op
        - value
      properties:
        type:
          type: string
          enum:
            - number
        op:
          type: string
          enum:
            - eq
            - ne
            - gt
            - ge
            - lt
            - le
          description: >-
            eq: equal, ne: not equal, gt: greater than, ge: greater than or
            equal, lt: less than, le: less than or equal
        value:
          type: number
    RangeFilter:
      type: object
      required:
        - type
        - op
        - value
      properties:
        type:
          type: string
          enum:
            - range
        op:
          type: string
          enum:
            - bt
          description: 'bt: between'
        value:
          type: array
          minItems: 2
          maxItems: 2
          items:
            type: number
          description: '[lower_bound, upper_bound]'
    CustomFieldFilter:
      description: A filter on a custom field, identified by key.
      allOf:
        - $ref: '#/components/schemas/ValueFilter'
        - type: object
          required:
            - key
          properties:
            key:
              type: string
              description: The field name to filter on.
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
    ValueFilter:
      oneOf:
        - $ref: '#/components/schemas/StringFilter'
        - $ref: '#/components/schemas/NumberFilter'
        - $ref: '#/components/schemas/BooleanFilter'
        - $ref: '#/components/schemas/RangeFilter'
        - $ref: '#/components/schemas/EnumFilter'
        - $ref: '#/components/schemas/PresentFilter'
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
    PresentFilter:
      type: object
      required:
        - type
        - op
      properties:
        type:
          type: string
          enum:
            - present
        op:
          type: string
          enum:
            - pr
            - np
          description: 'pr: present (has value), np: not present'
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