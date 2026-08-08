> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# List Calls

> List calls with unified cursor pagination response.

<Note>
  `POST /v3/list-calls` is the current list-calls endpoint. It supersedes the legacy [`GET /list-calls`](/api-references/list-calls_deprecated) — there is no `v2/list-calls`. To keep responses lean, the v3 payload omits `transcript`, `transcript_object`, `transcript_with_tool_calls`, and `recording_url`; call [`GET /v1/get-call/{call_id}`](/api-references/get-call) with a specific `call_id` to fetch those fields.
</Note>


## OpenAPI

````yaml openapi-final post /v3/list-calls
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
  /v3/list-calls:
    post:
      description: List calls with unified cursor pagination response.
      operationId: listCalls
      requestBody:
        required: false
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/V3ListCallsRequest'
      responses:
        '200':
          description: Successfully retrieved calls.
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
                          $ref: '#/components/schemas/V3CallResponse'
                      total:
                        type: integer
                        description: >-
                          Total number of calls matching `filter_criteria`. Only
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

            const calls = await client.call.list();

            console.log(calls.has_more);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            calls = client.call.list()
            print(calls.has_more)
components:
  schemas:
    V3ListCallsRequest:
      type: object
      properties:
        filter_criteria:
          $ref: '#/components/schemas/CallFilter'
        sort_order:
          type: string
          enum:
            - ascending
            - descending
          default: descending
          description: Sort calls by `start_timestamp` in ascending or descending order.
        limit:
          type: integer
          default: 50
          maximum: 1000
          description: Maximum number of calls to return.
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
            Whether to include `total` (count of all calls matching
            `filter_criteria`, ignoring `limit`/`skip`/`pagination_key`) in the
            response. Defaults to false. Each enabled request triggers an
            additional aggregate query, so opt in only when the total is needed.
      not:
        required:
          - skip
          - pagination_key
    PaginatedResponseBase:
      type: object
      properties:
        pagination_key:
          type: string
          description: Pagination key for the next page.
        has_more:
          type: boolean
          description: Whether more results are available.
    V3CallResponse:
      oneOf:
        - $ref: '#/components/schemas/V3WebCallResponse'
        - $ref: '#/components/schemas/V3PhoneCallResponse'
    CallFilter:
      type: object
      description: >-
        Filter criteria for calls. All conditions are implicitly connected with
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
        call_id:
          oneOf:
            - $ref: '#/components/schemas/StringFilter'
            - $ref: '#/components/schemas/EnumFilter'
          description: Filter by call ID.
        batch_call_id:
          $ref: '#/components/schemas/StringFilter'
          description: Filter by batch call ID.
        call_status:
          allOf:
            - $ref: '#/components/schemas/EnumFilter'
            - description: Filter by call status.
              properties:
                value:
                  items:
                    enum:
                      - not_connected
                      - ongoing
                      - ended
                      - error
        in_voicemail:
          $ref: '#/components/schemas/BooleanFilter'
          description: Filter by whether the call is in voicemail.
        disconnection_reason:
          allOf:
            - $ref: '#/components/schemas/EnumFilter'
            - description: Filter by disconnection reason.
              properties:
                value:
                  items:
                    $ref: '#/components/schemas/DisconnectionReason'
        from_number:
          $ref: '#/components/schemas/StringFilter'
          description: Filter by from number.
        to_number:
          $ref: '#/components/schemas/StringFilter'
          description: Filter by to number.
        call_type:
          allOf:
            - $ref: '#/components/schemas/EnumFilter'
            - description: Filter by call type.
              properties:
                value:
                  items:
                    enum:
                      - web_call
                      - phone_call
        direction:
          allOf:
            - $ref: '#/components/schemas/EnumFilter'
            - description: Filter by call direction.
              properties:
                value:
                  items:
                    enum:
                      - inbound
                      - outbound
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
        data_storage_setting:
          allOf:
            - $ref: '#/components/schemas/EnumFilter'
            - description: Filter by data storage setting.
              properties:
                value:
                  items:
                    enum:
                      - everything
                      - everything_except_pii
                      - basic_attributes_only
        call_successful:
          $ref: '#/components/schemas/BooleanFilter'
          description: Filter by whether the call was successful.
        start_timestamp:
          oneOf:
            - $ref: '#/components/schemas/NumberFilter'
            - $ref: '#/components/schemas/RangeFilter'
          description: Filter by call start timestamp (epoch ms).
        end_timestamp:
          oneOf:
            - $ref: '#/components/schemas/NumberFilter'
            - $ref: '#/components/schemas/RangeFilter'
          description: Filter by call end timestamp (epoch ms).
        duration_ms:
          oneOf:
            - $ref: '#/components/schemas/NumberFilter'
            - $ref: '#/components/schemas/RangeFilter'
          description: Filter by call duration in milliseconds.
        combined_cost:
          oneOf:
            - $ref: '#/components/schemas/NumberFilter'
            - $ref: '#/components/schemas/RangeFilter'
          description: Filter by combined cost of the call.
        e2e_latency_p50:
          oneOf:
            - $ref: '#/components/schemas/NumberFilter'
            - $ref: '#/components/schemas/RangeFilter'
          description: Filter by end-to-end latency p50.
        tool_calls:
          type: array
          items:
            $ref: '#/components/schemas/ToolCallFilter'
          description: >-
            Filter by tool call criteria. Tool call filters are connected by
            AND.
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
        metadata:
          type: array
          items:
            $ref: '#/components/schemas/CustomFieldFilter'
          description: Filter by metadata fields.
        dynamic_variables:
          type: array
          items:
            allOf:
              - $ref: '#/components/schemas/StringFilter'
              - type: object
                required:
                  - key
                properties:
                  key:
                    type: string
                    description: The dynamic variable name to filter on.
          description: Filter by dynamic variables.
    V3WebCallResponse:
      allOf:
        - type: object
          required:
            - call_type
            - access_token
          properties:
            call_type:
              type: string
              enum:
                - web_call
              example: web_call
              description: >-
                Type of the call. Used to distinguish between web call and phone
                call.
            access_token:
              type: string
              example: eyJhbGciOiJIUzI1NiJ9.eyJ2aWRlbyI6eyJyb29tSm9p
              description: >-
                Access token to enter the web call room. This needs to be passed
                to your frontend to join the call.
        - $ref: '#/components/schemas/V3CallBase'
    V3PhoneCallResponse:
      allOf:
        - type: object
          required:
            - call_type
            - from_number
            - to_number
            - direction
          properties:
            call_type:
              type: string
              enum:
                - phone_call
              example: phone_call
              description: >-
                Type of the call. Used to distinguish between web call and phone
                call.
            from_number:
              type: string
              example: '+12137771234'
              description: The caller number.
            to_number:
              type: string
              example: '+12137771235'
              description: The callee number.
            direction:
              type: string
              enum:
                - inbound
                - outbound
              example: inbound
              description: Direction of the phone call.
            telephony_identifier:
              type: object
              description: >-
                Telephony identifier of the call, populated when available.
                Tracking purposes only.
              properties:
                twilio_call_sid:
                  type: string
                  example: CA5d0d0d8047bf685c3f0ff980fe62c123
                  description: Twilio call sid.
        - $ref: '#/components/schemas/V3CallBase'
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
    ToolCallFilter:
      type: object
      minProperties: 1
      properties:
        name:
          type: string
          description: The tool call name to filter on.
        type:
          type: string
          description: The tool call type to filter on.
        latency_ms:
          oneOf:
            - $ref: '#/components/schemas/NumberFilter'
            - $ref: '#/components/schemas/RangeFilter'
          description: Filter by tool call latency in milliseconds.
        success:
          $ref: '#/components/schemas/BooleanFilter'
          description: Filter by tool call success status.
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
    V3CallBase:
      type: object
      required:
        - call_id
        - agent_id
        - agent_version
        - call_status
      properties:
        call_id:
          type: string
          example: Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6
          description: >-
            Unique id of the call. Used to identify the call in the LLM
            websocket and used to authenticate in the audio websocket.
        agent_id:
          type: string
          example: oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD
          description: Corresponding agent id of this call.
        agent_name:
          type: string
          example: My Agent
          description: Name of the agent.
        agent_version:
          type: integer
          example: 1
          description: The version of the agent.
        agent_tag:
          type: string
          example: prod
          nullable: true
          description: >-
            Tag pointing at the agent version used for this call, captured at
            call creation time and frozen thereafter (unaffected by later tag
            reassignments). Populated whether the caller dispatched by tag,
            numeric version, "latest", or "latest_published" — when the caller
            specified a tag, that tag wins; otherwise the most-recently-
            assigned tag on the resolved version is used. Absent when no tag
            points at the resolved version (or for calls created before this
            field was introduced).
        call_status:
          type: string
          enum:
            - registered
            - not_connected
            - ongoing
            - ended
            - error
          example: registered
          description: >
            Status of call.


            - `registered`: Call id issued, starting to make a call using this
            id.

            - `ongoing`: Call connected and ongoing.

            - `ended`: The underlying websocket has ended for the call. Either
            user or agent hung up, or call transferred.

            - `error`: Call encountered error.
        metadata:
          type: object
          description: >-
            An arbitrary object for storage purpose only. You can put anything
            here like your internal customer id associated with the call. Not
            used for processing. You can later get this field from the call
            object.
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
            Dynamic variables collected from the call. Only available after the
            call ends.
        custom_sip_headers:
          type: object
          additionalProperties:
            type: string
          example:
            X-Custom-Header: Custom Value
          description: Custom SIP headers to be added to the call.
        data_storage_setting:
          type: string
          enum:
            - everything
            - everything_except_pii
            - basic_attributes_only
          example: everything
          description: >-
            Data storage setting for this call's agent. "everything" stores all
            data, "everything_except_pii" excludes PII when possible,
            "basic_attributes_only" stores only metadata.
          nullable: true
        opt_in_signed_url:
          type: boolean
          example: true
          description: >-
            Whether this agent opts in for signed URLs for public logs and
            recordings. When enabled, the generated URLs will include security
            signatures that restrict access and automatically expire after 24
            hours.
        start_timestamp:
          type: integer
          example: 1703302407333
          description: >-
            Begin timestamp (milliseconds since epoch) of the call. Available
            after call starts.
        end_timestamp:
          type: integer
          example: 1703302428855
          description: >-
            End timestamp (milliseconds since epoch) of the call. Available
            after call ends.
        transfer_end_timestamp:
          type: integer
          example: 1703302628855
          description: >-
            Transfer end timestamp (milliseconds since epoch) of the call.
            Available after transfer call ends.
        duration_ms:
          type: integer
          example: 10000
          description: Duration of the call in milliseconds. Available after call ends.
        recording_url:
          type: string
          example: >-
            https://retellai.s3.us-west-2.amazonaws.com/Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6/recording.wav
          description: Recording of the call. Available after call ends.
        recording_multi_channel_url:
          type: string
          example: >-
            https://retellai.s3.us-west-2.amazonaws.com/Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6/recording_multichannel.wav
          description: >-
            Recording of the call, with each party's audio stored in a separate
            channel. Available after the call ends.
        scrubbed_recording_url:
          type: string
          example: >-
            https://retellai.s3.us-west-2.amazonaws.com/Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6/recording.wav
          description: Recording of the call without PII. Available after call ends.
        scrubbed_recording_multi_channel_url:
          type: string
          example: >-
            https://retellai.s3.us-west-2.amazonaws.com/Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6/recording_multichannel.wav
          description: >-
            Recording of the call without PII, with each party's audio stored in
            a separate channel. Available after the call ends.
        public_log_url:
          type: string
          example: >-
            https://retellai.s3.us-west-2.amazonaws.com/Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6/public_log.txt
          description: >-
            Public log of the call, containing details about all the requests
            and responses received in LLM WebSocket, latency tracking for each
            turntaking, helpful for debugging and tracing. Available after call
            ends.
        knowledge_base_retrieved_contents_url:
          type: string
          example: >-
            https://retellai.s3.us-west-2.amazonaws.com/Jabr9TXYYJHfvl6Syypi88rdAHYHmcq6/kb_retrieved_contents.txt
          description: >-
            URL to the knowledge base retrieved contents of the call. Available
            after call ends if the call utilizes knowledge base feature. It
            consists of the respond id and the retrieved contents related to
            that response. It's already rendered in call history tab of
            dashboard, and you can also manually download and check against the
            transcript to view the knowledge base retrieval results.
        latency:
          type: object
          description: >-
            Latency tracking of the call, available after call ends. Not all
            fields here will be available, as it depends on the type of call and
            feature used.
          properties:
            e2e:
              $ref: '#/components/schemas/CallLatency'
              description: >-
                End to end latency (from user stops talking to agent start
                talking) tracking of the call. This latency does not account for
                the network trip time from Retell server to user frontend. The
                latency is tracked every time turn change between user and
                agent.
            asr:
              $ref: '#/components/schemas/CallLatency'
              description: >-
                Transcription latency (diff between the duration of the chunks
                streamed and the durations of the transcribed part) tracking of
                the call.
            llm:
              $ref: '#/components/schemas/CallLatency'
              description: >-
                LLM latency (from issue of LLM call to first speakable chunk
                received) tracking of the call. When using custom LLM. this
                latency includes LLM websocket roundtrip time between user
                server and Retell server.
            llm_websocket_network_rtt:
              $ref: '#/components/schemas/CallLatency'
              description: >-
                LLM websocket roundtrip latency (between user server and Retell
                server) tracking of the call. Only populated for calls using
                custom LLM.
            tts:
              $ref: '#/components/schemas/CallLatency'
              description: >-
                Text-to-speech latency (from the triggering of TTS to first byte
                received) tracking of the call.
            knowledge_base:
              $ref: '#/components/schemas/CallLatency'
              description: >-
                Knowledge base latency (from the triggering of knowledge base
                retrival to all relevant context received) tracking of the call.
                Only populated when using knowledge base feature for the agent
                of the call.
            s2s:
              $ref: '#/components/schemas/CallLatency'
              description: >-
                Speech-to-speech latency (from requesting responses of a S2S
                model to first byte received) tracking of the call. Only
                populated for calls that uses S2S model like Realtime API.
        disconnection_reason:
          $ref: '#/components/schemas/DisconnectionReason'
          example: agent_hangup
          description: >-
            The reason for the disconnection of the call. Read detailed
            description about reasons listed here at [Disconnection Reason
            Doc](/reliability/debug-call-disconnect#understanding-disconnection-reasons).
        transfer_destination:
          type: string
          example: '+12137771234'
          description: >-
            The destination number or identifier where the call was transferred
            to. Only populated when the disconnection reason was
            `call_transfer`. Can be a phone number or a SIP URI. SIP URIs are
            prefixed with "sip:" and may include a ";transport=..." portion (if
            transport is known) where the transport type can be "tls", "tcp" or
            "udp".
          nullable: true
        call_analysis:
          $ref: '#/components/schemas/CallAnalysis'
          description: >-
            Post call analysis that includes information such as sentiment,
            status, summary, and custom defined data to extract. Available after
            call ends. Subscribe to `call_analyzed` webhook event type to
            receive it once ready.
        call_cost:
          description: >-
            Cost of the call, including all the products and their costs and
            discount.
          type: object
          required:
            - product_costs
            - total_duration_seconds
            - total_duration_unit_price
            - combined_cost
          properties:
            product_costs:
              type: array
              description: List of products with their unit prices and costs in cents
              items:
                $ref: '#/components/schemas/ProductCost'
            total_duration_seconds:
              type: number
              description: Total duration of the call in seconds
              example: 60
            total_duration_unit_price:
              type: number
              description: Total unit duration price of all products in cents per second
              example: 1
            combined_cost:
              type: number
              description: Combined cost of all individual costs in cents
              example: 70
        llm_token_usage:
          type: object
          description: >-
            LLM token usage of the call, available after call ends. Not
            populated if using custom LLM, realtime API, or no LLM call is made.
          required:
            - values
            - average
            - num_requests
          properties:
            values:
              type: array
              items:
                type: number
              description: All the token count values in the call.
            average:
              type: number
              description: Average token count of the call.
            num_requests:
              type: number
              description: Number of requests made to the LLM.
    ValueFilter:
      oneOf:
        - $ref: '#/components/schemas/StringFilter'
        - $ref: '#/components/schemas/NumberFilter'
        - $ref: '#/components/schemas/BooleanFilter'
        - $ref: '#/components/schemas/RangeFilter'
        - $ref: '#/components/schemas/EnumFilter'
        - $ref: '#/components/schemas/PresentFilter'
    CallLatency:
      type: object
      properties:
        p50:
          type: number
          description: 50 percentile of latency, measured in milliseconds.
          example: 800
        p90:
          type: number
          description: 90 percentile of latency, measured in milliseconds.
          example: 1200
        p95:
          type: number
          description: 95 percentile of latency, measured in milliseconds.
          example: 1500
        p99:
          type: number
          description: 99 percentile of latency, measured in milliseconds.
          example: 2500
        max:
          type: number
          description: Maximum latency in the call, measured in milliseconds.
          example: 2700
        min:
          type: number
          description: Minimum latency in the call, measured in milliseconds.
          example: 500
        num:
          type: number
          description: Number of data points (number of times latency is tracked).
          example: 10
        values:
          type: array
          items:
            type: number
          description: All the latency data points in the call, measured in milliseconds.
    CallAnalysis:
      type: object
      properties:
        call_summary:
          type: string
          example: >-
            The agent called the user to ask question about his purchase
            inquiry. The agent asked several questions regarding his preference
            and asked if user would like to book an appointment. The user
            happily agreed and scheduled an appointment next Monday 10am.
          description: A high level summary of the call.
        in_voicemail:
          type: boolean
          example: false
          description: Whether the call is entered voicemail.
        user_sentiment:
          type: string
          enum:
            - Negative
            - Positive
            - Neutral
            - Unknown
          example: Positive
          description: Sentiment of the user in the call.
        call_successful:
          type: boolean
          example: true
          description: >-
            Whether the agent seems to have a successful call with the user,
            where the agent finishes the task, and the call was complete without
            being cutoff.
        custom_analysis_data:
          type: object
          description: >-
            Custom analysis data that was extracted based on the schema defined
            in agent post call analysis data. Can be empty if nothing is
            specified.
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