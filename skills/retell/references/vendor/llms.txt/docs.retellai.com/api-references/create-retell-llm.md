> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Retell LLM

> Create a new Retell LLM Response Engine that can be attached to an agent. This is used to generate response output for the agent.



## OpenAPI

````yaml openapi-final post /create-retell-llm
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
  /create-retell-llm:
    post:
      description: >-
        Create a new Retell LLM Response Engine that can be attached to an
        agent. This is used to generate response output for the agent.
      operationId: createRetellLLM
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/RetellLlmRequest'
      responses:
        '201':
          description: Successfully created a new Retell LLM Response Engine.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/RetellLLMResponse'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '500':
          $ref: '#/components/responses/InternalServerError'
      x-codeSamples:
        - lang: JavaScript
          source: |-
            import Retell from 'retell-sdk';

            const client = new Retell({
              apiKey: process.env['RETELL_API_KEY'], // This is the default and can be omitted
            });

            const llmResponse = await client.llm.create();

            console.log(llmResponse.llm_id);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            llm_response = client.llm.create()
            print(llm_response.llm_id)
components:
  schemas:
    RetellLlmRequest:
      allOf:
        - $ref: '#/components/schemas/RetellLlmOverride'
        - type: object
          properties:
            general_prompt:
              type: string
              example: You are ...
              description: >
                General prompt appended to system prompt no matter what state
                the agent is in.


                - System prompt (with state) = general prompt + state prompt.

                - System prompt (no state) = general prompt.
              nullable: true
            general_tools:
              type: array
              items:
                $ref: '#/components/schemas/Tool'
              description: >
                A list of tools the model may call (to get external knowledge,
                call API, etc). You can select from some common predefined tools
                like end call, transfer call, etc; or you can create your own
                custom tool for the LLM to use.


                - Tools of LLM (with state) = general tools + state tools +
                state transitions

                - Tools of LLM (no state) = general tools
              example:
                - type: end_call
                  name: end_call
                  description: End the call with user.
              nullable: true
            states:
              type: array
              items:
                $ref: '#/components/schemas/State'
              description: >-
                States of the LLM. This is to help reduce prompt length and tool
                choices when the call can be broken into distinct states. With
                shorter prompts and less tools, the LLM can better focus and
                follow the rules, minimizing hallucination. If this field is not
                set, the agent would only have general prompt and general tools
                (essentially one state).
              example:
                - name: information_collection
                  state_prompt: You will follow the steps below to collect information...
                  edges:
                    - destination_state_name: appointment_booking
                      description: Transition to book an appointment.
                  tools:
                    - type: transfer_call
                      name: transfer_to_support
                      description: Transfer to the support team.
                      transfer_destination:
                        type: predefined
                        number: '16175551212'
                        ignore_e164_validation: false
                      transfer_option:
                        type: cold_transfer
                        show_transferee_as_caller: false
                - name: appointment_booking
                  state_prompt: You will follow the steps below to book an appointment...
                  tools:
                    - type: book_appointment_cal
                      name: book_appointment
                      description: Book an annual check up.
                      cal_api_key: cal_live_xxxxxxxxxxxx
                      event_type_id: 60444
                      timezone: America/Los_Angeles
              nullable: true
            starting_state:
              type: string
              example: information_collection
              description: Name of the starting state. Required if states is not empty.
              nullable: true
            default_dynamic_variables:
              type: object
              additionalProperties:
                type: string
              example:
                customer_name: John Doe
              description: >-
                Default dynamic variables represented as key-value pairs of
                strings. These are injected into your Retell LLM prompt and tool
                description when specific values are not provided in a request.
                Only applicable for Retell LLM.
              nullable: true
            mcps:
              type: array
              items:
                $ref: '#/components/schemas/MCP'
              description: A list of MCPs to use for this LLM.
              nullable: true
    RetellLLMResponse:
      allOf:
        - type: object
          required:
            - llm_id
          properties:
            llm_id:
              type: string
              example: oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD
              description: Unique id of Retell LLM Response Engine.
            version:
              type: integer
              example: 0
              description: Version of the Retell LLM Response Engine.
            is_published:
              type: boolean
              example: false
              description: Whether the Retell LLM Response Engine is published.
        - $ref: '#/components/schemas/RetellLlmRequest'
        - type: object
          required:
            - last_modification_timestamp
          properties:
            last_modification_timestamp:
              type: integer
              example: 1703413636133
              description: >-
                Last modification timestamp (milliseconds since epoch). Either
                the time of last update or creation if no updates available.
    RetellLlmOverride:
      type: object
      description: >-
        Override properties for Retell LLM configuration in agent override
        requests.
      properties:
        model:
          $ref: '#/components/schemas/NullableLLMModel'
          example: gpt-4.1
          description: >-
            Select the underlying text LLM. If not set, would default to
            gpt-4.1.
        s2s_model:
          type: string
          enum:
            - gpt-realtime-2.1
            - gpt-realtime-2.1-mini
            - gpt-realtime-2
            - gpt-realtime-1.5
            - gpt-realtime
            - gpt-realtime-mini
            - null
          example: gpt-realtime-1.5
          description: >-
            Select the underlying speech to speech model. Can only set this or
            model, not both.
          nullable: true
        model_temperature:
          type: number
          example: 0
          description: >-
            If set, will control the randomness of the response. Value ranging
            from [0,1]. Lower value means more deterministic, while higher value
            means more random. If unset, default value 0 will apply. Note that
            for tool calling, a lower value is recommended.
        model_high_priority:
          type: boolean
          example: true
          description: >-
            If set to true, will use high priority pool with more dedicated
            resource to ensure lower and more consistent latency, default to
            false. This feature usually comes with a higher cost.
          nullable: true
        tool_call_strict_mode:
          type: boolean
          example: true
          description: >-
            Whether to use strict mode for tool calls. Only applicable when
            using certain supported models.
          nullable: true
        knowledge_base_ids:
          type: array
          items:
            type: string
          description: A list of knowledge base ids to use for this resource.
          nullable: true
        kb_config:
          $ref: '#/components/schemas/KBConfig'
          type: object
          description: Knowledge base configuration for RAG retrieval.
          nullable: true
        start_speaker:
          type: string
          enum:
            - user
            - agent
          description: >-
            The speaker who starts the conversation. Required. Must be either
            'user' or 'agent'.
        begin_after_user_silence_ms:
          type: integer
          example: 2000
          description: >-
            If set, the AI will begin the conversation after waiting for the
            user for the duration (in milliseconds) specified by this attribute.
            This only applies if the agent is configured to wait for the user to
            speak first. If not set, the agent will wait indefinitely for the
            user to speak.
          nullable: true
        begin_message:
          type: string
          example: Hey I am a virtual assistant calling from Retell Hospital.
          description: >-
            First utterance said by the agent in the call. If not set, LLM will
            dynamically generate a message. If set to "", agent will wait for
            user to speak first.
          nullable: true
    Tool:
      oneOf:
        - $ref: '#/components/schemas/EndCallTool'
        - $ref: '#/components/schemas/TransferCallTool'
        - $ref: '#/components/schemas/CheckAvailabilityCalTool'
        - $ref: '#/components/schemas/BookAppointmentCalTool'
        - $ref: '#/components/schemas/AgentSwapTool'
        - $ref: '#/components/schemas/PressDigitTool'
        - $ref: '#/components/schemas/SendSMSTool'
        - $ref: '#/components/schemas/CustomTool'
        - $ref: '#/components/schemas/CodeTool'
        - $ref: '#/components/schemas/ExtractDynamicVariableTool'
        - $ref: '#/components/schemas/BridgeTransferTool'
        - $ref: '#/components/schemas/CancelTransferTool'
        - $ref: '#/components/schemas/MCPTool'
    State:
      type: object
      required:
        - name
      properties:
        name:
          example: information_collection
          type: string
          description: >-
            Name of the state, must be unique for each state. Must be consisted
            of a-z, A-Z, 0-9, or contain underscores and dashes, with a maximum
            length of 64 (no space allowed).
        state_prompt:
          example: |-
            ## Task
            You will follow the steps below...
          type: string
          description: |
            Prompt of the state, will be appended to the system prompt of LLM.

            - System prompt = general prompt + state prompt.
        edges:
          type: array
          items:
            $ref: '#/components/schemas/StateEdge'
          description: >-
            Edges of the state define how and what state can be reached from
            this state.
        tools:
          type: array
          items:
            $ref: '#/components/schemas/Tool'
          description: >
            A list of tools specific to this state the model may call (to get
            external knowledge, call API, etc). You can select from some common
            predefined tools like end call, transfer call, etc; or you can
            create your own custom tool for the LLM to use.


            - Tools of LLM = general tools + state tools + state transitions
    MCP:
      type: object
      properties:
        name:
          type: string
        url:
          type: string
          description: The URL of the MCP server.
        headers:
          type: object
          additionalProperties:
            type: string
          example:
            Authorization: Bearer 1234567890
          description: Headers to add to the MCP connection request.
        query_params:
          type: object
          additionalProperties:
            type: string
          example:
            index: '1'
            key: value
          description: Query parameters to append to the  MCP connection request URL.
        timeout_ms:
          type: integer
          description: >-
            Maximum time to wait for a connection to be established (in
            milliseconds). Default to 120,000 ms (2 minutes).
      required:
        - name
        - url
    NullableLLMModel:
      type: string
      enum:
        - gpt-4.1
        - gpt-4.1-mini
        - gpt-4.1-nano
        - gpt-5
        - gpt-5-mini
        - gpt-5-nano
        - gpt-5.1
        - gpt-5.2
        - gpt-5.4
        - gpt-5.4-mini
        - gpt-5.4-nano
        - gpt-5.5
        - gpt-5.6-terra
        - gpt-5.6-luna
        - claude-4.5-sonnet
        - claude-4.6-sonnet
        - claude-5-sonnet
        - claude-4.5-haiku
        - gemini-3.0-flash
        - gemini-3.1-flash-lite
        - gemini-3.5-flash
        - null
      nullable: true
      description: Available LLM models for agents.
    KBConfig:
      type: object
      properties:
        top_k:
          type: integer
          minimum: 1
          maximum: 10
          example: 3
          description: Max number of knowledge base chunks to retrieve
        filter_score:
          type: number
          minimum: 0
          maximum: 1
          example: 0.6
          description: Similarity threshold for filtering search results
    EndCallTool:
      type: object
      properties:
        type:
          type: string
          enum:
            - end_call
        name:
          type: string
          description: >-
            Name of the tool. Must be unique within all tools available to LLM
            at any given time (general tools + state tools + state transitions).
            Must be consisted of a-z, A-Z, 0-9, or contain underscores and
            dashes, with a maximum length of 64 (no space allowed).
        description:
          type: string
          description: >-
            Describes what the tool does, sometimes can also include information
            about when to call the tool.
        speak_during_execution:
          type: boolean
          description: If true, will speak during execution.
        execution_message_description:
          type: string
          description: >-
            Describes what to say to user when ending the call. Only applicable
            when speak_during_execution is true.
        execution_message_type:
          type: string
          enum:
            - prompt
            - static_text
          description: >-
            Type of execution message. "prompt" means the agent will use
            execution_message_description as a prompt to generate the message.
            "static_text" means the agent will speak the
            execution_message_description directly. Defaults to "prompt".
      required:
        - type
        - name
    TransferCallTool:
      type: object
      properties:
        type:
          type: string
          enum:
            - transfer_call
        name:
          type: string
          example: transfer_to_support
          description: >-
            Name of the tool. Must be unique within all tools available to LLM
            at any given time (general tools + state tools + state edges).
        description:
          type: string
          description: >-
            Describes what the tool does, sometimes can also include information
            about when to call the tool.
        transfer_destination:
          $ref: '#/components/schemas/TransferDestination'
          type: object
        ignore_e164_validation:
          type: boolean
          description: >-
            If true, the e.164 validation will be ignored for the from_number.
            This can be useful when you want to dial to internal pseudo numbers.
            This only applies when you are using custom telephony and does not
            apply when you are using Retell Telephony. If omitted, the default
            value is false.
          example: false
        custom_sip_headers:
          type: object
          additionalProperties:
            type: string
          example:
            X-Custom-Header: Custom Value
          description: Custom SIP headers to be added to the call.
        transfer_option:
          $ref: '#/components/schemas/TransferOption'
          type: object
        speak_during_execution:
          type: boolean
          description: If true, will speak during execution.
        execution_message_description:
          type: string
          description: >-
            Describes what to say to user when transferring the call. Only
            applicable when speak_during_execution is true.
        execution_message_type:
          type: string
          enum:
            - prompt
            - static_text
          description: >-
            Type of execution message. "prompt" means the agent will use
            execution_message_description as a prompt to generate the message.
            "static_text" means the agent will speak the
            execution_message_description directly. Defaults to "prompt".
      required:
        - type
        - name
        - transfer_destination
        - transfer_option
    CheckAvailabilityCalTool:
      type: object
      properties:
        type:
          type: string
          enum:
            - check_availability_cal
        name:
          type: string
          description: >-
            Name of the tool. Must be unique within all tools available to LLM
            at any given time (general tools + state tools + state transitions).
            Must be consisted of a-z, A-Z, 0-9, or contain underscores and
            dashes, with a maximum length of 64 (no space allowed).
        description:
          type: string
          description: >-
            Describes what the tool does, sometimes can also include information
            about when to call the tool.
        cal_api_key:
          type: string
          description: >-
            Cal.com Api key that have access to the cal.com event you want to
            check availability for.
        event_type_id:
          oneOf:
            - type: number
            - type: string
          description: >-
            Cal.com event type id number for the cal.com event you want to check
            availability for. Can be a number or a dynamic variable in the
            format `{{variable_name}}` that will be resolved at runtime.
        timezone:
          type: string
          description: >-
            Timezone to be used when checking availability, must be in [IANA
            timezone
            database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).
            Can also be a dynamic variable in the format `{{variable_name}}`
            that will be resolved at runtime. If not specified, will check if
            user specified timezone in call, and if not, will use the timezone
            of the Retell servers.
      required:
        - type
        - name
        - cal_api_key
        - event_type_id
    BookAppointmentCalTool:
      type: object
      properties:
        type:
          type: string
          enum:
            - book_appointment_cal
        name:
          type: string
          description: >-
            Name of the tool. Must be unique within all tools available to LLM
            at any given time (general tools + state tools + state transitions).
            Must be consisted of a-z, A-Z, 0-9, or contain underscores and
            dashes, with a maximum length of 64 (no space allowed).
        description:
          type: string
          description: >-
            Describes what the tool does, sometimes can also include information
            about when to call the tool.
        cal_api_key:
          type: string
          description: >-
            Cal.com Api key that have access to the cal.com event you want to
            book appointment.
        event_type_id:
          oneOf:
            - type: number
            - type: string
          description: >-
            Cal.com event type id number for the cal.com event you want to book
            appointment. Can be a number or a dynamic variable in the format
            `{{variable_name}}` that will be resolved at runtime.
        timezone:
          type: string
          description: >-
            Timezone to be used when booking appointment, must be in [IANA
            timezone
            database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).
            Can also be a dynamic variable in the format `{{variable_name}}`
            that will be resolved at runtime. If not specified, will check if
            user specified timezone in call, and if not, will use the timezone
            of the Retell servers.
      required:
        - type
        - name
        - cal_api_key
        - event_type_id
    AgentSwapTool:
      type: object
      properties:
        name:
          type: string
          description: >-
            Name of the tool. Must be unique within all tools available to LLM
            at any given time (general tools + state tools + state edges).
        type:
          type: string
          enum:
            - agent_swap
        description:
          type: string
          description: >-
            Describes what the tool does, sometimes can also include information
            about when to call the tool.
        agent_id:
          type: string
          minLength: 1
          description: The id of the agent to swap to.
        agent_version:
          $ref: '#/components/schemas/AgentVersionReference'
          description: >-
            The version of the agent to swap to. If not specified, will use the
            latest version.
        speak_during_execution:
          type: boolean
        execution_message_description:
          type: string
          description: The message for the agent to speak when executing agent swap.
        execution_message_type:
          type: string
          enum:
            - prompt
            - static_text
          description: >-
            Type of execution message. "prompt" means the agent will use
            execution_message_description as a prompt to generate the message.
            "static_text" means the agent will speak the
            execution_message_description directly. Defaults to "prompt".
        post_call_analysis_setting:
          $ref: '#/components/schemas/PostCallAnalysisSetting'
          description: Post call analysis setting for the agent swap.
        webhook_setting:
          $ref: '#/components/schemas/AgentSwapWebhookSetting'
          description: Webhook setting for the agent swap, defaults to only source.
        keep_current_voice:
          type: boolean
          description: >-
            If true, keep the current voice when swapping agents. Defaults to
            false.
        keep_current_language:
          type: boolean
          description: >-
            If true, keep the current language when swapping agents. Defaults to
            false.
      required:
        - type
        - name
        - agent_id
        - post_call_analysis_setting
    PressDigitTool:
      type: object
      properties:
        type:
          type: string
          enum:
            - press_digit
        name:
          type: string
          description: >-
            Name of the tool. Must be unique within all tools available to LLM
            at any given time (general tools + state tools + state transitions).
            Must be consisted of a-z, A-Z, 0-9, or contain underscores and
            dashes, with a maximum length of 64 (no space allowed).
        description:
          type: string
          description: >-
            Describes what the tool does, sometimes can also include information
            about when to call the tool.
        delay_ms:
          type: integer
          description: >-
            Delay in milliseconds before pressing the digit, because a lot of
            IVR systems speak very slowly, and a delay can make sure the agent
            hears the full menu. Default to 1000 ms (1s). Valid range is 0 to
            5000 ms (inclusive).
      required:
        - type
        - name
    SendSMSTool:
      type: object
      properties:
        name:
          type: string
          description: >-
            Name of the tool. Must be unique within all tools available to LLM
            at any given time (general tools + state tools + state edges).
        type:
          type: string
          enum:
            - send_sms
        description:
          type: string
          description: >-
            Describes what the tool does, sometimes can also include information
            about when to call the tool.
        speak_during_execution:
          type: boolean
          description: >-
            If true, the agent will speak a short line before sending the SMS.
            If omitted, defaults to true (same as end_call / transfer_call
            tools).
        execution_message_description:
          type: string
          description: >-
            Describes what to say before sending the SMS. Only applicable when
            speak_during_execution is true.
        execution_message_type:
          type: string
          enum:
            - prompt
            - static_text
          description: >-
            Type of execution message. "prompt" means the agent will use
            execution_message_description as a prompt to generate the message.
            "static_text" means the agent will speak the
            execution_message_description directly. Defaults to "prompt".
        sms_content:
          $ref: '#/components/schemas/SmsContent'
      required:
        - type
        - name
        - sms_content
    CustomTool:
      type: object
      properties:
        type:
          type: string
          enum:
            - custom
        name:
          type: string
          description: >-
            Name of the tool. Must be unique within all tools available to LLM
            at any given time (general tools + state tools + state edges). Must
            be consisted of a-z, A-Z, 0-9, or contain underscores and dashes,
            with a maximum length of 64 (no space allowed).
        url:
          type: string
          description: >-
            Describes what the tool does, sometimes can also include information
            about when to call the tool.
        description:
          type: string
          description: Describes what this tool does and when to call this tool.
        method:
          type: string
          enum:
            - GET
            - POST
            - PUT
            - PATCH
            - DELETE
          description: Method to use for the request, default to POST.
        headers:
          type: object
          additionalProperties:
            type: string
          example:
            Authorization: Bearer 1234567890
          description: Headers to add to the request.
        query_params:
          type: object
          additionalProperties:
            type: string
          example:
            page: '1'
            sort: asc
          description: Query parameters to append to the request URL.
        parameters:
          $ref: '#/components/schemas/ToolParameter'
        response_variables:
          type: object
          additionalProperties:
            type: string
          example:
            user_name: data.user.name
          description: >-
            A mapping of variable names to JSON paths in the response body.
            These values will be extracted from the response and made available
            as dynamic variables for use.
        speak_during_execution:
          type: boolean
          description: >-
            Determines whether the agent would say sentence like "One moment,
            let me check that." when executing the function. Recommend to turn
            on if your function call takes over 1s (including network) to
            complete, so that your agent remains responsive.
        speak_after_execution:
          type: boolean
          description: >-
            Determines whether the agent would call LLM another time and speak
            when the result of function is obtained. Usually this needs to get
            turned on so user can get update for the function call.
        execution_message_description:
          type: string
          description: >-
            The description for the sentence agent say during execution. Only
            applicable when speak_during_execution is true. Can write what to
            say or even provide examples. The default is "The message you will
            say to callee when calling this tool. Make sure it fits into the
            conversation smoothly.".
        execution_message_type:
          type: string
          enum:
            - prompt
            - static_text
          description: >-
            Type of execution message. "prompt" means the agent will use
            execution_message_description as a prompt to generate the message.
            "static_text" means the agent will speak the
            execution_message_description directly. Defaults to "prompt".
        timeout_ms:
          type: integer
          description: >-
            The maximum time in milliseconds the tool can run before it's
            considered timeout. If the tool times out, the agent would have that
            info. The minimum value allowed is 1000 ms (1 s), and maximum value
            allowed is 600,000 ms (10 min). By default, this is set to 120,000
            ms (2 min).
        args_at_root:
          type: boolean
          description: >-
            If set to true, the parameters will be passed as root level JSON
            object instead of nested under "args".
        parameter_type:
          type: string
          enum:
            - json
            - form
          description: >-
            How the tool's `parameters` are authored and shown in the dashboard
            editor — "form" for the visual parameter builder, "json" for a raw
            JSON Schema. Both produce the same `parameters` schema; this does
            not change how the request body is encoded (see `args_at_root`).
        enable_typing_sound:
          type: boolean
          description: >-
            If true, play a typing sound on the agent audio track while this
            tool is executing. Useful when the tool takes a noticeable amount of
            time to prevent silence on the call.
      required:
        - type
        - name
        - url
    CodeTool:
      type: object
      properties:
        type:
          type: string
          enum:
            - code
        name:
          type: string
          description: >-
            Name of the tool. Must be unique within all tools available to LLM
            at any given time (general tools + state tools + state edges). Must
            be consisted of a-z, A-Z, 0-9, or contain underscores and dashes,
            with a maximum length of 64 (no space allowed).
        description:
          type: string
          description: Describes what this tool does and when to call this tool.
        code:
          type: string
          maxLength: 20000
          description: JavaScript code to execute in the sandbox.
        timeout_ms:
          type: integer
          minimum: 5000
          maximum: 60000
          description: >-
            The maximum time in milliseconds the code can run before it's
            considered timeout. Defaults to 30,000 ms (30 s).
        response_variables:
          type: object
          additionalProperties:
            type: string
          example:
            order_id: data.order.id
          description: >-
            A mapping of variable names to JSON paths in the code execution
            result. These mapped values will be extracted and added as dynamic
            variables.
        speak_during_execution:
          type: boolean
          description: >-
            Determines whether the agent would say sentence like "One moment,
            let me check that." when executing the tool.
        speak_after_execution:
          type: boolean
          default: true
          description: >-
            Determines whether the agent would call LLM another time and speak
            when the result of function is obtained.
        execution_message_description:
          type: string
          description: >-
            The description for the sentence agent say during execution. Only
            applicable when speak_during_execution is true.
        execution_message_type:
          type: string
          enum:
            - prompt
            - static_text
          description: >-
            Type of execution message. "prompt" means the agent will use
            execution_message_description as a prompt to generate the message.
            "static_text" means the agent will speak the
            execution_message_description directly. Defaults to "prompt".
        enable_typing_sound:
          type: boolean
          description: >-
            If true, play a typing sound on the agent audio track while this
            tool is executing.
      required:
        - type
        - name
        - code
    ExtractDynamicVariableTool:
      type: object
      properties:
        type:
          type: string
          enum:
            - extract_dynamic_variable
        name:
          type: string
          description: >-
            Name of the tool. Must be unique within all tools available to LLM
            at any given time (general tools + state tools + state edges). Must
            be consisted of a-z, A-Z, 0-9, or contain underscores and dashes,
            with a maximum length of 64 (no space allowed).
        description:
          type: string
          description: >-
            Describes what the tool does, sometimes can also include information
            about when to call the tool.
        variables:
          type: array
          items:
            $ref: '#/components/schemas/AnalysisData'
          description: The variables to be extracted.
        enable_typing_sound:
          type: boolean
          description: >-
            If true, play a typing sound on the agent audio track while this
            tool is executing.
      required:
        - type
        - name
        - variables
        - description
    BridgeTransferTool:
      type: object
      properties:
        type:
          type: string
          enum:
            - bridge_transfer
        name:
          type: string
          description: >-
            Name of the tool. Must be unique within all tools available to LLM
            at any given time (general tools + state tools + state transitions).
            Must be consisted of a-z, A-Z, 0-9, or contain underscores and
            dashes, with a maximum length of 64 (no space allowed).
        description:
          type: string
          description: >-
            Describes what the tool does. This tool is only available to
            transfer agents (agents with isTransferAgent set to true) in agentic
            warm transfer mode. When invoked, it bridges the original caller to
            the transfer target and ends the transfer agent call.
        speak_during_execution:
          type: boolean
          description: If true, will speak during execution.
        execution_message_description:
          type: string
          description: >-
            Describes what to say to user when bridging the transfer. Only
            applicable when speak_during_execution is true.
        execution_message_type:
          type: string
          enum:
            - prompt
            - static_text
          description: >-
            Type of execution message. "prompt" means the agent will use
            execution_message_description as a prompt to generate the message.
            "static_text" means the agent will speak the
            execution_message_description directly. Defaults to "prompt".
      required:
        - type
        - name
    CancelTransferTool:
      type: object
      properties:
        type:
          type: string
          enum:
            - cancel_transfer
        name:
          type: string
          description: >-
            Name of the tool. Must be unique within all tools available to LLM
            at any given time (general tools + state tools + state transitions).
            Must be consisted of a-z, A-Z, 0-9, or contain underscores and
            dashes, with a maximum length of 64 (no space allowed).
        description:
          type: string
          description: >-
            Describes what the tool does. This tool is only available to
            transfer agents (agents with isTransferAgent set to true) in agentic
            warm transfer mode. When invoked, it cancels the transfer, returns
            the original caller to the main agent, and ends the transfer agent
            call.
        speak_during_execution:
          type: boolean
          description: If true, will speak during execution.
        execution_message_description:
          type: string
          description: >-
            Describes what to say to user when cancelling the transfer. Only
            applicable when speak_during_execution is true.
        execution_message_type:
          type: string
          enum:
            - prompt
            - static_text
          description: >-
            Type of execution message. "prompt" means the agent will use
            execution_message_description as a prompt to generate the message.
            "static_text" means the agent will speak the
            execution_message_description directly. Defaults to "prompt".
      required:
        - type
        - name
    MCPTool:
      type: object
      properties:
        type:
          type: string
          enum:
            - mcp
        mcp_id:
          type: string
          description: Unique id of the MCP.
        name:
          type: string
          description: Name of the MCP tool.
        description:
          type: string
          description: Description of the MCP tool.
        input_schema:
          type: object
          additionalProperties:
            type: string
          description: The input schema of the MCP tool.
        response_variables:
          type: object
          additionalProperties:
            type: string
          description: >-
            Response variables to add to dynamic variables, key is the variable
            name, value is the path to the variable in the response
        speak_during_execution:
          type: boolean
          description: >-
            Determines whether the agent would say sentence like "One moment,
            let me check that." when executing the function. Recommend to turn
            on if your function call takes over 1s (including network) to
            complete, so that your agent remains responsive.
        speak_after_execution:
          type: boolean
          description: >-
            Determines whether the agent would call LLM another time and speak
            when the result of function is obtained. Usually this needs to get
            turned on so user can get update for the function call.
        execution_message_description:
          type: string
          description: >-
            The description for the sentence agent say during execution. Only
            applicable when speak_during_execution is true. Can write what to
            say or even provide examples. The default is "The message you will
            say to callee when calling this tool. Make sure it fits into the
            conversation smoothly.".
        execution_message_type:
          type: string
          enum:
            - prompt
            - static_text
          description: >-
            Type of execution message. "prompt" means the agent will use
            execution_message_description as a prompt to generate the message.
            "static_text" means the agent will speak the
            execution_message_description directly. Defaults to "prompt".
        enable_typing_sound:
          type: boolean
          description: >-
            If true, play a typing sound on the agent audio track while this MCP
            tool is executing.
      required:
        - type
        - name
        - description
    StateEdge:
      type: object
      required:
        - destination_state_name
        - description
      properties:
        destination_state_name:
          type: string
          description: >-
            The destination state name when going through transition of state
            via this edge. State transition internally is implemented as a tool
            call of LLM, and a tool call with name
            "transition_to_{destination_state_name}" will get created. Feel free
            to reference it inside the prompt.
        description:
          type: string
          description: >-
            Describes what's the transition and at what time / criteria should
            this transition happen.
        parameters:
          $ref: '#/components/schemas/ToolParameter'
          description: >-
            Describes what parameters you want to extract out when the
            transition changes. The parameters extracted here can be referenced
            in prompts & function descriptions of later states via dynamic
            variables. The parameters the functions accepts, described as a JSON
            Schema object. See [JSON Schema
            reference](https://json-schema.org/understanding-json-schema/) for
            documentation about the format.
    TransferDestination:
      oneOf:
        - $ref: '#/components/schemas/TransferDestinationPredefined'
        - $ref: '#/components/schemas/TransferDestinationInferred'
    TransferOption:
      oneOf:
        - $ref: '#/components/schemas/TransferOptionColdTransfer'
        - $ref: '#/components/schemas/TransferOptionWarmTransfer'
        - $ref: '#/components/schemas/TransferOptionAgenticWarmTransfer'
      x-mintlify-name: Transfer Options
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
    PostCallAnalysisSetting:
      type: string
      enum:
        - both_agents
        - only_destination_agent
    AgentSwapWebhookSetting:
      type: string
      enum:
        - both_agents
        - only_destination_agent
        - only_source_agent
    SmsContent:
      oneOf:
        - $ref: '#/components/schemas/SmsContentPredefined'
        - $ref: '#/components/schemas/SmsContentInferred'
        - $ref: '#/components/schemas/SmsContentTemplate'
    ToolParameter:
      type: object
      description: >-
        The parameters the functions accepts, described as a JSON Schema object.
        See [JSON Schema
        reference](https://json-schema.org/understanding-json-schema/) for
        documentation about the format. Omitting parameters defines a function
        with an empty parameter list.
      required:
        - type
        - properties
      properties:
        type:
          type: string
          enum:
            - object
          description: Type must be "object" for a JSON Schema object.
        properties:
          type: object
          description: >-
            The value of properties is an object, where each key is the name of
            a property and each value is a schema used to validate that
            property.
        required:
          type: array
          items:
            type: string
          description: >-
            List of names of required property when generating this parameter.
            LLM will do its best to generate the required properties in its
            function arguments. Property must exist in properties.
    AnalysisData:
      oneOf:
        - $ref: '#/components/schemas/StringAnalysisData'
        - $ref: '#/components/schemas/EnumAnalysisData'
        - $ref: '#/components/schemas/BooleanAnalysisData'
        - $ref: '#/components/schemas/NumberAnalysisData'
    TransferDestinationPredefined:
      type: object
      properties:
        type:
          type: string
          enum:
            - predefined
          description: The type of transfer destination.
        number:
          type: string
          description: >-
            The number to transfer to in E.164 format or a dynamic variable like
            {{transfer_number}}.
        extension:
          type: string
          description: >-
            Extension digits to dial after the main number connects. Sent via
            DTMF. Allow digits, '*', '#', or a dynamic variable like
            {{extension}}.
          example: 123*456#
      required:
        - type
        - number
    TransferDestinationInferred:
      type: object
      properties:
        type:
          type: string
          enum:
            - inferred
          description: The type of transfer destination.
        prompt:
          type: string
          description: >-
            The prompt to be used to help infer the transfer destination. The
            model will take the global prompt, the call transcript, and this
            prompt together to deduce the right number to transfer to. Can
            contain dynamic variables.
      required:
        - type
        - prompt
    TransferOptionColdTransfer:
      type: object
      title: Cold Transfer
      properties:
        type:
          type: string
          enum:
            - cold_transfer
          description: The type of the transfer.
        show_transferee_as_caller:
          type: boolean
          description: >-
            If set to true, will show transferee (the user, not the AI agent) as
            caller when transferring. Requires the telephony side to support
            caller id override. Retell Twilio numbers support this option. This
            parameter takes effect only when `cold_transfer_mode` is set to
            `sip_invite`. When using `sip_refer`, this option is not available.
            Retell Twilio numbers always use user's number as the caller id when
            using `sip refer` cold transfer mode.
        cold_transfer_mode:
          type: string
          enum:
            - sip_refer
            - sip_invite
          description: >-
            The mode of the cold transfer. If set to `sip_refer`, will use SIP
            REFER to transfer the call. If set to `sip_invite`, will use SIP
            INVITE to transfer the call.
          default: sip_invite
        transfer_ring_duration_ms:
          type: integer
          minimum: 5000
          maximum: 90000
          description: >-
            Override the ring duration for this specific transfer, in
            milliseconds. If not set, falls back to the agent-level
            `ring_duration_ms`.
      required:
        - type
    TransferOptionWarmTransfer:
      type: object
      title: Warm Transfer
      properties:
        type:
          type: string
          enum:
            - warm_transfer
          description: The type of the transfer.
        show_transferee_as_caller:
          type: boolean
          description: >-
            If set to true, will show transferee (the user, not the AI agent) as
            caller when transferring, requires the telephony side to support
            caller id override. Retell Twilio numbers support this option.
        agent_detection_timeout_ms:
          type: number
          description: The time to wait before considering transfer fails.
        transfer_ring_duration_ms:
          type: integer
          minimum: 5000
          maximum: 90000
          description: >-
            Override the ring duration for this specific transfer, in
            milliseconds. If not set, falls back to the agent-level
            `ring_duration_ms`.
        on_hold_music:
          type: string
          enum:
            - none
            - relaxing_sound
            - uplifting_beats
            - ringtone
            - custom
          description: >-
            The music to play while the caller is being transferred. Use
            `custom` together with `custom_on_hold_music_asset_id` to play an
            uploaded audio asset.
        custom_on_hold_music_asset_id:
          type: string
          description: >-
            Asset ID of the uploaded hold music to play. Required when
            `on_hold_music` is `custom`. Must reference an audio asset owned by
            the organization (see create-asset).
          example: asset_abc123def456
        public_handoff_option:
          type: object
          oneOf:
            - $ref: '#/components/schemas/WarmTransferPrompt'
            - $ref: '#/components/schemas/WarmTransferStaticMessage'
          description: >-
            If set, when transfer is successful, will say the handoff message to
            both the transferee and the agent receiving the transfer. Can leave
            either a static message or a dynamic one based on prompt. Set to
            null to disable warm handoff.
        private_handoff_option:
          type: object
          oneOf:
            - $ref: '#/components/schemas/WarmTransferPrompt'
            - $ref: '#/components/schemas/WarmTransferStaticMessage'
          description: >-
            If set, when transfer is connected, will say the handoff message
            only to the agent receiving the transfer. Can leave either a static
            message or a dynamic one based on prompt. Set to null to disable
            warm handoff.
        ivr_option:
          $ref: '#/components/schemas/WarmTransferPrompt'
          type: object
          description: >-
            IVR navigation option to run when doing human detection. This prompt
            will guide the AI on how to navigate the IVR system.
        opt_out_human_detection:
          type: boolean
          description: >-
            If set to true, will not perform human detection for the transfer.
            Default to false.
        enable_bridge_audio_cue:
          type: boolean
          description: >-
            Whether to play an audio cue when bridging the call. Defaults to
            true.
          default: true
      required:
        - type
    TransferOptionAgenticWarmTransfer:
      type: object
      title: Agentic Warm Transfer
      properties:
        type:
          type: string
          enum:
            - agentic_warm_transfer
          description: The type of the transfer.
        show_transferee_as_caller:
          type: boolean
          description: >-
            If set to true, will show transferee (the user, not the AI agent) as
            caller when transferring, requires the telephony side to support
            caller id override. Retell Twilio numbers support this option.
        on_hold_music:
          type: string
          enum:
            - none
            - relaxing_sound
            - uplifting_beats
            - ringtone
            - custom
          description: >-
            The music to play while the caller is being transferred. Use
            `custom` together with `custom_on_hold_music_asset_id` to play an
            uploaded audio asset.
        custom_on_hold_music_asset_id:
          type: string
          description: >-
            Asset ID of the uploaded hold music to play. Required when
            `on_hold_music` is `custom`. Must reference an audio asset owned by
            the organization (see create-asset).
          example: asset_abc123def456
        transfer_ring_duration_ms:
          type: integer
          minimum: 5000
          maximum: 90000
          description: >-
            Override the ring duration for this specific transfer, in
            milliseconds. If not set, falls back to the agent-level
            `ring_duration_ms`.
        public_handoff_option:
          type: object
          oneOf:
            - $ref: '#/components/schemas/WarmTransferPrompt'
            - $ref: '#/components/schemas/WarmTransferStaticMessage'
          description: >-
            If set, when transfer is successful, will say the handoff message to
            both the transferee and the agent receiving the transfer. Can leave
            either a static message or a dynamic one based on prompt. Set to
            null to disable warm handoff.
        agentic_transfer_config:
          type: object
          description: >-
            Configuration for agentic warm transfer. Required for agentic warm
            transfer.
          properties:
            transfer_agent:
              type: object
              description: The agent that will mediate the transfer decision.
              properties:
                agent_id:
                  type: string
                  minLength: 1
                  description: >-
                    The agent ID of the transfer agent. This agent must have
                    isTransferAgent set to true and should use bridge_transfer
                    and cancel_transfer tools (for Retell LLM) or
                    BridgeTransferNode and CancelTransferNode (for Conversation
                    Flow).
                agent_version:
                  $ref: '#/components/schemas/AgentVersionReference'
                  description: The version of the transfer agent to use.
              required:
                - agent_id
                - agent_version
            transfer_timeout_ms:
              type: number
              description: >-
                The maximum time to wait for the transfer agent to make a
                decision, in milliseconds. Defaults to 30000 (30 seconds).
              default: 30000
            action_on_timeout:
              type: string
              enum:
                - bridge_transfer
                - cancel_transfer
              description: >-
                The action to take when the transfer agent times out without
                making a decision. Defaults to cancel_transfer.
              default: cancel_transfer
        enable_bridge_audio_cue:
          type: boolean
          description: >-
            Whether to play an audio cue when bridging the call. Defaults to
            true.
          default: true
      required:
        - type
        - agentic_transfer_config
    SmsContentPredefined:
      type: object
      properties:
        type:
          type: string
          enum:
            - predefined
        text:
          type: string
          description: >-
            The static message to be sent in the SMS. Can contain dynamic
            variables.
    SmsContentInferred:
      type: object
      properties:
        type:
          type: string
          enum:
            - inferred
        prompt:
          type: string
          description: >-
            The prompt to be used to help infer the SMS content. The model will
            take the global prompt, the call transcript, and this prompt
            together to deduce the right message to send. Can contain dynamic
            variables.
    SmsContentTemplate:
      type: object
      required:
        - type
        - template
      properties:
        type:
          type: string
          enum:
            - template
        template:
          type: string
          enum:
            - info_collection
          description: >-
            The template to use for the SMS content. "info_collection" sends a
            predefined message requesting information from the user.
    StringAnalysisData:
      type: object
      required:
        - type
        - name
        - description
      properties:
        type:
          type: string
          enum:
            - string
          description: Type of the variable to extract.
          example: string
        name:
          type: string
          description: Name of the variable.
          example: customer_name
          minLength: 1
        description:
          type: string
          description: Description of the variable.
          example: The name of the customer.
        examples:
          type: array
          items:
            type: string
          description: Examples of the variable value to teach model the style and syntax.
          example:
            - John Doe
            - Jane Smith
        required:
          type: boolean
          description: >-
            Whether this data is required. If true and the data is not
            extracted, the call will be marked as unsuccessful.
        conditional_prompt:
          type: string
          description: >-
            Optional instruction to help decide whether this field needs to be
            populated in the analysis. If not set, the field is always included.
            If required is true, this is ignored.
    EnumAnalysisData:
      type: object
      required:
        - type
        - name
        - description
        - choices
      properties:
        type:
          type: string
          enum:
            - enum
          description: Type of the variable to extract.
          example: enum
        name:
          type: string
          description: Name of the variable.
          example: product_rating
          minLength: 1
        description:
          type: string
          description: Description of the variable.
          example: Rating of the product.
        choices:
          type: array
          items:
            type: string
          description: The possible values of the variable, must be non empty array.
          example:
            - good
        required:
          type: boolean
          description: >-
            Whether this data is required. If true and the data is not
            extracted, the call will be marked as unsuccessful.
        conditional_prompt:
          type: string
          description: >-
            Optional instruction to help decide whether this field needs to be
            populated in the analysis. If not set, the field is always included.
            If required is true, this is ignored.
    BooleanAnalysisData:
      type: object
      required:
        - type
        - name
        - description
      properties:
        type:
          type: string
          enum:
            - boolean
          description: Type of the variable to extract.
          example: boolean
        name:
          type: string
          description: Name of the variable.
          example: is_converted
          minLength: 1
        description:
          type: string
          description: Description of the variable.
          example: Whether the customer converted.
        required:
          type: boolean
          description: >-
            Whether this data is required. If true and the data is not
            extracted, the call will be marked as unsuccessful.
        conditional_prompt:
          type: string
          description: >-
            Optional instruction to help decide whether this field needs to be
            populated in the analysis. If not set, the field is always included.
            If required is true, this is ignored.
    NumberAnalysisData:
      type: object
      required:
        - type
        - name
        - description
      properties:
        type:
          type: string
          enum:
            - number
          description: Type of the variable to extract.
          example: number
        name:
          type: string
          description: Name of the variable.
          example: order_count
          minLength: 1
        description:
          type: string
          description: Description of the variable.
          example: How many the customer intend to order.
        required:
          type: boolean
          description: >-
            Whether this data is required. If true and the data is not
            extracted, the call will be marked as unsuccessful.
        conditional_prompt:
          type: string
          description: >-
            Optional instruction to help decide whether this field needs to be
            populated in the analysis. If not set, the field is always included.
            If required is true, this is ignored.
    WarmTransferPrompt:
      type: object
      properties:
        type:
          type: string
          enum:
            - prompt
        prompt:
          type: string
          example: Summarize the call in one sentence for the warn handoff.
          description: >-
            The prompt to be used for warm handoff. Can contain dynamic
            variables.
    WarmTransferStaticMessage:
      type: object
      properties:
        type:
          type: string
          enum:
            - static_message
        message:
          type: string
          example: You can take it from here.
          description: >-
            The static message to be used for warm handoff. Can contain dynamic
            variables.
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