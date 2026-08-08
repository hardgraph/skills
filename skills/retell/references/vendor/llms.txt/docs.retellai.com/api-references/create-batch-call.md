> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Batch Call

> Create a batch call



## OpenAPI

````yaml openapi-final post /create-batch-call
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
  /create-batch-call:
    post:
      description: Create a batch call
      operationId: createBatchCall
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - from_number
                - tasks
              properties:
                name:
                  type: string
                  example: First batch call
                  description: >-
                    The name of the batch call. Only used for your own
                    reference.
                trigger_timestamp:
                  type: number
                  example: 1735718400000
                  description: >-
                    The scheduled time for sending the batch call, represented
                    as a Unix timestamp in milliseconds. If omitted, the call
                    will be sent immediately.
                from_number:
                  type: string
                  minLength: 1
                  example: '+14157774444'
                  description: >-
                    The number you own in E.164 format. Must be a number
                    purchased from Retell or imported to Retell.
                reserved_concurrency:
                  type: integer
                  minimum: 0
                  description: >-
                    Number of concurrency reserved for all other calls that are
                    not triggered by batch calls, such as inbound calls.
                tasks:
                  type: array
                  description: >-
                    A list of individual call tasks to be executed as part of
                    the batch call. Each task represents a single outbound call
                    and includes details such as the recipient's phone number
                    and optional dynamic variables to personalize the call
                    content.
                  items:
                    $ref: '#/components/schemas/BatchCallTask'
                call_time_window:
                  $ref: '#/components/schemas/CallTimeWindow'
      responses:
        '201':
          description: Successfully created a batch call.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/BatchCallResponse'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '422':
          $ref: '#/components/responses/UnprocessableContent'
        '500':
          $ref: '#/components/responses/InternalServerError'
      x-codeSamples:
        - lang: JavaScript
          source: |-
            import Retell from 'retell-sdk';

            const client = new Retell({
              apiKey: process.env['RETELL_API_KEY'], // This is the default and can be omitted
            });

            const batchCallResponse = await client.batchCall.createBatchCall({
              from_number: '+14157774444',
              tasks: [{ to_number: '+12137774445' }],
            });

            console.log(batchCallResponse.batch_call_id);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            batch_call_response = client.batch_call.create_batch_call(
                from_number="+14157774444",
                tasks=[{
                    "to_number": "+12137774445"
                }],
            )
            print(batch_call_response.batch_call_id)
components:
  schemas:
    BatchCallTask:
      type: object
      required:
        - to_number
      properties:
        to_number:
          type: string
          minLength: 1
          example: '+12137774445'
          description: >-
            The number you want to call, in E.164 format. If using a number
            purchased from Retell, only US numbers are supported as destination.
        ignore_e164_validation:
          type: boolean
          description: >-
            If true, the e.164 validation will be ignored for the from_number.
            This can be useful when you want to dial to internal pseudo numbers.
            This only applies when you are using custom telephony and does not
            apply when you are using Retell Telephony. If omitted, the default
            value is false.
          example: false
        override_agent_id:
          type: string
          minLength: 1
          example: oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD
          description: >-
            For this particular call, override the agent used with this agent
            id. This does not bind the agent to this number, this is for one
            time override.
        override_agent_version:
          $ref: '#/components/schemas/AgentVersionReference'
          description: >-
            For this particular call, override the agent version used with this
            version. This does not bind the agent to this number, this is for
            one time override.
        agent_override:
          $ref: '#/components/schemas/AgentOverrideRequest'
          description: >-
            For this particular call, override agent configuration with these
            settings. This allows you to customize agent behavior for individual
            calls without modifying the base agent.
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
        metadata:
          type: object
          description: >-
            An arbitrary object for storage purpose only. You can put anything
            here like your internal customer id associated with the call. Not
            used for processing. You can later get this field from the call
            object.
        custom_sip_headers:
          type: object
          additionalProperties:
            type: string
          example:
            X-Custom-Header: Custom Value
          description: Add optional custom SIP headers to the call.
    CallTimeWindow:
      type: object
      description: >-
        Allowed calling windows in a specific timezone. Each window is a
        half-open interval [startMin, endMin) in minutes since 00:00 local time.
        Cross-midnight windows are NOT allowed (must satisfy startMin < endMin).
        `endMin = 1440` (24:00) is valid.
      properties:
        windows:
          type: array
          minItems: 1
          items:
            $ref: '#/components/schemas/TimeWindow'
          description: List of TimeWindow (start/end in minutes since local midnight).
        timezone:
          type: string
          description: >-
            IANA timezone (e.g. America/Los_Angeles). Defaults to
            America/Los_Angeles if omitted.
        day:
          type: array
          items:
            $ref: '#/components/schemas/DayOfWeek'
          description: >-
            Optional list of days to which the windows apply. If omitted or
            empty, windows apply to every day.
      required:
        - windows
    BatchCallResponse:
      type: object
      required:
        - batch_call_id
        - name
        - from_number
        - scheduled_timestamp
        - total_task_count
      properties:
        batch_call_id:
          type: string
          example: batch_call_dbcc4412483ebfc348abb
          description: Unique id of the batch call.
        name:
          type: string
          example: First batch call
        from_number:
          type: string
          example: '+14157774444'
        scheduled_timestamp:
          type: number
          example: 1735718400
        total_task_count:
          type: number
          description: Number of tasks within the batch call
        call_time_window:
          $ref: '#/components/schemas/CallTimeWindow'
          description: >-
            Canonicalized minutes-based time windows. Present only if specified
            when the batch call was created or updated. See CallTimeWindow for
            format details ([startMin, endMin) in local minutes; no
            cross-midnight).
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
    AgentOverrideRequest:
      type: object
      description: >-
        Override configuration for agent, retell LLM, or conversation flow
        settings for a specific call.
      properties:
        agent:
          $ref: '#/components/schemas/AgentRequest'
          description: >-
            Override agent configuration settings. Any properties specified here
            will override the base agent configuration for this call.
        retell_llm:
          $ref: '#/components/schemas/RetellLlmOverride'
          description: >-
            Override Retell LLM configuration settings. Only applicable when
            using Retell LLM as the response engine. Supported attributes -
            model, s2s_model, model_temperature, model_high_priority,
            tool_call_strict_mode, knowledge_base_ids, kb_config, start_speaker,
            begin_after_user_silence_ms, begin_message.
        conversation_flow:
          $ref: '#/components/schemas/ConversationFlowOverride'
          description: >-
            Override conversation flow configuration settings. Only applicable
            when using conversation flow as the response engine. Supported
            attributes - model_choice, model_temperature, tool_call_strict_mode,
            knowledge_base_ids, kb_config, start_speaker,
            begin_after_user_silence_ms.
    TimeWindow:
      type: object
      required:
        - start
        - end
      properties:
        start:
          type: number
          example: 540
          description: Start time in minutes since local midnight.
        end:
          type: number
          example: 1020
          description: End time in minutes since local midnight.
    DayOfWeek:
      type: string
      enum:
        - Monday
        - Tuesday
        - Wednesday
        - Thursday
        - Friday
        - Saturday
        - Sunday
      description: Day of week. Matches server-side DayOfWeek enum.
    AgentRequest:
      type: object
      properties:
        response_engine:
          $ref: '#/components/schemas/ResponseEngine'
          description: >-
            The Response Engine to attach to the agent. It is used to generate
            responses for the agent. You need to create a Response Engine first
            before attaching it to an agent.
          example:
            type: retell-llm
            llm_id: llm_234sdertfsdsfsdf
            version: 0
        agent_name:
          type: string
          example: Jarvis
          description: The name of the agent. Only used for your own reference.
          nullable: true
        version_description:
          type: string
          example: Customer support agent for handling product inquiries
          description: >-
            Optional description of the agent version. Used for your own
            reference and documentation.
          nullable: true
        version_title:
          type: string
          example: Production hotfix
          description: Optional title of the agent version. Used for your own reference.
          nullable: true
        voice_id:
          type: string
          example: retell-Cimo
          description: >-
            Unique voice id used for the agent. Find list of available voices
            and their preview in Dashboard.
        voice_model:
          type: string
          enum:
            - eleven_flash_v2
            - eleven_flash_v2_5
            - eleven_multilingual_v2
            - eleven_v3
            - sonic-3
            - sonic-3-latest
            - sonic-3.5
            - tts-1
            - gpt-4o-mini-tts
            - speech-02-turbo
            - speech-2.8-turbo
            - s1
            - s2-pro
            - s2.1-pro
            - null
          description: >-
            Select the voice model used for the selected voice. Each provider
            has a set of available voice models. Set to null to remove voice
            model selection, and default ones will apply. Check out dashboard
            for more details of each voice model.
          nullable: true
        fallback_voice_ids:
          type: array
          items:
            type: string
          example:
            - cartesia-Cimo
            - minimax-Cimo
          description: >-
            When TTS provider for the selected voice is experiencing outages, we
            would use fallback voices listed here for the agent. Voice id and
            the fallback voice ids must be from different TTS providers. The
            system would go through the list in order, if the first one in the
            list is also having outage, it would use the next one. Set to null
            to remove voice fallback for the agent.
          nullable: true
        voice_temperature:
          type: number
          example: 1
          description: >-
            Controls how stable the voice is. Value ranging from [0,2]. Lower
            value means more stable, and higher value means more variant speech
            generation. Check the dashboard to see what provider supports this
            feature. If unset, default value 1 will apply.
        voice_speed:
          type: number
          minimum: 0.5
          maximum: 2
          example: 1
          description: >-
            Controls speed of voice. Value ranging from [0.5,2]. Lower value
            means slower speech, while higher value means faster speech rate. If
            unset, default value 1 will apply.
        enable_dynamic_voice_speed:
          type: boolean
          example: true
          description: >-
            If set to true, will enable dynamic voice speed adjustment based on
            the user's speech rate and conversation context. If unset, default
            value false will apply.
        enable_dynamic_responsiveness:
          type: boolean
          example: true
          description: >-
            If set to true, the agent will dynamically adjust how quickly it
            responds based on the user's speech rate and past turn-taking
            behavior in the call. If unset, default value false will apply.
        volume:
          type: number
          example: 1
          description: >-
            If set, will control the volume of the agent. Value ranging from
            [0,2]. Lower value means quieter agent speech, while higher value
            means louder agent speech. If unset, default value 1 will apply.
        voice_emotion:
          type: string
          nullable: true
          enum:
            - calm
            - sympathetic
            - happy
            - sad
            - angry
            - fearful
            - surprised
            - null
          example: calm
          description: >
            Controls the emotional tone of the agent's voice. Currently
            supported for Cartesia and Minimax TTS providers. If unset, no
            emotion will be used.
        enable_expressive_mode:
          type: boolean
          example: true
          description: >-
            Master toggle for expressive mode. When true, the agent may add
            expressive voice tags to the audio it generates. Only applicable for
            platform voices. If unset, defaults to false.
        expressive_emotion_tags:
          type: array
          uniqueItems: true
          items:
            type: string
            enum:
              - empathetic
              - excited
              - happy
              - curious
              - surprised
              - sigh
              - clear throat
              - pause
              - long pause
              - emphasis
          example:
            - empathetic
            - excited
            - sigh
            - clear throat
            - emphasis
          description: >-
            The expressive voice tags Retell pre-teaches the model to use when
            enable_expressive_mode is true. Custom tags defined in the system
            prompt are still allowed. If empty, the agent follows general
            expressive guidance without a fixed tag set.
        expressive_mode_prompt:
          type: string
          nullable: true
          example: Use [sigh] for thoughtful pauses and [excited] for good news.
          description: >-
            Custom expressive voice guidance to use instead of the default
            Retell expressive prompt when enable_expressive_mode is true. If
            omitted or blank, the default expressive prompt will be used.
        responsiveness:
          type: number
          minimum: 0
          maximum: 1
          example: 1
          description: >-
            Controls how responsive is the agent. Value ranging from [0,1].
            Lower value means less responsive agent (wait more, respond slower),
            while higher value means faster exchanges (respond when it can). If
            unset, default value 1 will apply.
        interruption_sensitivity:
          type: number
          minimum: 0
          maximum: 1
          example: 1
          description: >-
            Controls how sensitive the agent is to user interruptions. Value
            ranging from [0,1]. Lower value means it will take longer / more
            words for user to interrupt agent, while higher value means it's
            easier for user to interrupt agent. If unset, default value 1 will
            apply. When this is set to 0, agent would never be interrupted.
        enable_backchannel:
          type: boolean
          example: true
          description: >-
            Controls whether the agent would backchannel (agent interjects the
            speaker with phrases like "yeah", "uh-huh" to signify interest and
            engagement). Backchannel when enabled tends to show up more in
            longer user utterances. If not set, agent will not backchannel.
        backchannel_frequency:
          type: number
          example: 0.9
          description: >-
            Only applicable when enable_backchannel is true. Controls how often
            the agent would backchannel when a backchannel is possible. Value
            ranging from [0,1]. Lower value means less frequent backchannel,
            while higher value means more frequent backchannel. If unset,
            default value 0.8 will apply.
        backchannel_words:
          type: array
          items:
            type: string
          example:
            - yeah
            - uh-huh
          description: >-
            Only applicable when enable_backchannel is true. A list of words
            that the agent would use as backchannel. If not set, default
            backchannel words will apply. Check out [backchannel default
            words](/agent/interaction-configuration#backchannel) for more
            details. Note that certain voices do not work too well with certain
            words, so it's recommended to experiment before adding any words.
          nullable: true
        reminder_trigger_ms:
          type: number
          example: 10000
          description: >-
            If set (in milliseconds), will trigger a reminder to the agent to
            speak if the user has been silent for the specified duration after
            some agent speech. Must be a positive number. If unset, default
            value of 10000 ms (10 s) will apply.
        reminder_max_count:
          type: integer
          example: 2
          description: >-
            If set, controls how many times agent would remind user when user is
            unresponsive. Must be a non negative integer. If unset, default
            value of 1 will apply (remind once). Set to 0 to disable agent from
            reminding.
        ambient_sound:
          type: string
          enum:
            - coffee-shop
            - convention-hall
            - summer-outdoor
            - mountain-outdoor
            - static-noise
            - call-center
            - null
          description: >
            If set, will add ambient environment sound to the call to make
            experience more realistic. Currently supports the following options:


            - `coffee-shop`: Coffee shop ambience with people chatting in
            background. [Listen to
            Ambience](https://retell-utils-public.s3.us-west-2.amazonaws.com/coffee-shop.wav)

            - `convention-hall`: Convention hall ambience, with some echo and
            people chatting in background. [Listen to
            Ambience](https://retell-utils-public.s3.us-west-2.amazonaws.com/convention-hall.wav)

            - `summer-outdoor`: Summer outdoor ambience with cicada chirping.
            [Listen to
            Ambience](https://retell-utils-public.s3.us-west-2.amazonaws.com/summer-outdoor.wav)

            - `mountain-outdoor`: Mountain outdoor ambience with birds singing.
            [Listen to
            Ambience](https://retell-utils-public.s3.us-west-2.amazonaws.com/mountain-outdoor.wav)

            - `static-noise`: Constant static noise. [Listen to
            Ambience](https://retell-utils-public.s3.us-west-2.amazonaws.com/static-noise.wav)

            - `call-center`: Call center work noise. [Listen to
            Ambience](https://retell-utils-public.s3.us-west-2.amazonaws.com/call-center.wav)

            Set to `null` to remove ambient sound from this agent.
          nullable: true
        ambient_sound_volume:
          type: number
          example: 1
          description: >-
            If set, will control the volume of the ambient sound. Value ranging
            from [0,2]. Lower value means quieter ambient sound, while higher
            value means louder ambient sound. If unset, default value 1 will
            apply.
        language:
          oneOf:
            - $ref: '#/components/schemas/LanguageLegacy'
            - type: array
              items:
                $ref: '#/components/schemas/Language'
          description: >-
            Specifies what language(s) the agent will operate in. Accepts either
            a single scalar locale (e.g. `en-US`), the legacy scalar value
            `multi` for multilingual support, or an array of concrete locale
            codes for explicit multi-locale selection (e.g.
            `["en-US","es-ES"]`). The array form must contain concrete locale
            codes only — the `multi` value is valid only as the scalar legacy
            form and must not appear inside an array. Single-element arrays are
            normalized to the equivalent scalar on output. If unset, defaults to
            `en-US`.
        webhook_url:
          type: string
          example: https://webhook-url-here
          description: >-
            The webhook for agent to listen to call events. See what events it
            would get at [webhook doc](/features/webhook). If set, will binds
            webhook events for this agent to the specified url, and will ignore
            the account level webhook for this agent. Set to `null` to remove
            webhook url from this agent.
          nullable: true
        webhook_events:
          type: array
          items:
            type: string
            enum:
              - call_started
              - call_ended
              - call_analyzed
              - transcript_updated
              - transfer_started
              - transfer_bridged
              - transfer_cancelled
              - transfer_ended
          description: >-
            Which webhook events this agent should receive. If not set, defaults
            to call_started, call_ended, call_analyzed.
          nullable: true
        webhook_timeout_ms:
          type: integer
          example: 10000
          description: >-
            The timeout for the webhook in milliseconds. If not set, default
            value of 10000 will apply.
        boosted_keywords:
          type: array
          items:
            type: string
          example:
            - retell
            - kroger
          description: >-
            Provide a customized list of keywords to bias the transcriber model,
            so that these words are more likely to get transcribed. Commonly
            used for names, brands, street, etc. Entries may reference dynamic
            variables with `{{variable}}` syntax.
          nullable: true
        data_storage_setting:
          type: string
          enum:
            - everything
            - everything_except_pii
            - basic_attributes_only
          example: everything
          description: >
            Granular setting to manage how Retell stores sensitive data
            (transcripts, recordings, logs, etc.).

            This replaces the deprecated `opt_out_sensitive_data_storage` field.

            - `everything`: Store all data including transcripts, recordings,
            and logs.

            - `everything_except_pii`: Store data without PII when PII is
            detected.

            - `basic_attributes_only`: Store only basic attributes; no
            transcripts/recordings/logs.

            If not set, default value of "everything" will apply.
        data_storage_retention_days:
          type: integer
          minimum: 1
          maximum: 730
          example: 30
          nullable: true
          description: >-
            Number of days to retain call/chat data before automatic deletion.
            Must be between 1 and 730 days. If not set, data is retained forever
            (no automatic deletion).
        opt_in_signed_url:
          type: boolean
          example: true
          description: >-
            Whether this agent opts in for signed URLs for public logs and
            recordings. When enabled, the generated URLs will include security
            signatures that restrict access and automatically expire after 24
            hours.
        signed_url_expiration_ms:
          type: integer
          example: 86400000
          description: >-
            The expiration time for the signed url in milliseconds. Only
            applicable when opt_in_signed_url is true. If not set, default value
            of 86400000 (24 hours) will apply.
          nullable: true
        pronunciation_dictionary:
          type: array
          items:
            type: object
            required:
              - word
              - alphabet
              - phoneme
            properties:
              word:
                type: string
                example: actually
                description: >-
                  The string of word / phrase to be annotated with
                  pronunciation.
              alphabet:
                type: string
                enum:
                  - ipa
                  - cmu
                example: ipa
                description: The phonetic alphabet to be used for pronunciation.
              phoneme:
                type: string
                example: ˈæktʃuəli
                description: >-
                  Pronunciation of the word in the format of a IPA / CMU
                  pronunciation.
          description: >-
            A list of words / phrases and their pronunciation to be used to
            guide the audio synthesize for consistent pronunciation. Check the
            dashboard to see what provider supports this feature. Set to null to
            remove pronunciation dictionary from this agent.
          nullable: true
        end_call_after_silence_ms:
          type: integer
          example: 600000
          description: >-
            If users stay silent for a period after agent speech, end the call.
            The minimum value allowed is 10,000 ms (10 s). By default, this is
            set to 600000 (10 min).
        max_call_duration_ms:
          type: integer
          example: 3600000
          description: >-
            Maximum allowed length for the call, will force end the call if
            reached. The minimum value allowed is 60,000 ms (1 min), and maximum
            value allowed is 7,200,000 (2 hours). By default, this is set to
            3,600,000 (1 hour).
        voicemail_option:
          type: object
          properties:
            action:
              $ref: '#/components/schemas/VoicemailAction'
            detection_prompt:
              type: string
              maxLength: 2000
              nullable: true
              description: >-
                Optionally describe what should be treated as voicemail. Leave
                as null to use the default definition.
          required:
            - action
          description: >-
            If this option is set, the call will try to detect voicemail in the
            first 3 minutes of the call. Actions defined (hangup, or leave a
            message) will be applied when the voicemail is detected. Set this to
            null to disable voicemail detection.
          example:
            action:
              type: static_text
              text: Please give us a callback tomorrow at 10am.
          nullable: true
        ivr_option:
          type: object
          properties:
            action:
              $ref: '#/components/schemas/IvrAction'
            detection_prompt:
              type: string
              maxLength: 2000
              nullable: true
              description: >-
                Optionally describe what should be treated as an IVR. Leave as
                null to use the default definition.
          required:
            - action
          description: >-
            If this option is set, the call will try to detect IVR in the first
            3 minutes of the call. Actions defined will be applied when the IVR
            is detected. Set this to null to disable IVR detection.
          example:
            action:
              type: hangup
          nullable: true
        call_screening_option:
          $ref: '#/components/schemas/CallScreeningOption'
        post_call_analysis_data:
          type: array
          items:
            $ref: '#/components/schemas/PostCallAnalysisData'
          description: >-
            Post call analysis data to extract from the call. This data will
            augment the pre-defined variables extracted in the call analysis.
            This will be available after the call ends.
          nullable: true
        post_call_analysis_model:
          $ref: '#/components/schemas/NullableLLMModel'
          example: gpt-4.1-mini
          description: The model to use for post call analysis. Default to gpt-4.1.
        begin_message_delay_ms:
          type: integer
          example: 1000
          description: >-
            If set, will delay the first message by the specified amount of
            milliseconds, so that it gives user more time to prepare to take the
            call. Valid range is [0, 5000]. If not set or set to 0, agent will
            speak immediately. Only applicable when agent speaks first.
        ring_duration_ms:
          type: integer
          minimum: 5000
          maximum: 300000
          example: 30000
          description: >-
            If set, the phone ringing will last for the specified amount of
            milliseconds. This applies for both outbound call ringtime, and call
            transfer ringtime. Default to 30000 (30 s). Valid range is [5000,
            300000].
        stt_mode:
          type: string
          enum:
            - fast
            - accurate
            - custom
          example: fast
          description: >-
            If set, determines whether speech to text should focus on latency or
            accuracy. Default to fast mode. When set to custom,
            custom_stt_config must be provided.
        custom_stt_config:
          type: object
          description: Custom STT configuration. Only used when stt_mode is set to custom.
          properties:
            provider:
              allOf:
                - $ref: '#/components/schemas/AsrProvider'
              description: The STT provider to use.
            endpointing_ms:
              type: integer
              description: >-
                Endpointing timeout in milliseconds. Minimum is 100 for Azure,
                10 for Deepgram, 500 for Soniox, 100 for AssemblyAI.
          required:
            - provider
            - endpointing_ms
          nullable: true
        vocab_specialization:
          type: string
          enum:
            - general
            - medical
          example: general
          description: >-
            If set, determines the vocabulary set to use for transcription. This
            setting only applies for English agents, for non English agent, this
            setting is a no-op. Default to general.
        allow_user_dtmf:
          type: boolean
          example: true
          description: >-
            If set to true, DTMF input will be accepted and processed. If false,
            any DTMF input will be ignored. Default to true.
        allow_dtmf_interruption:
          type: boolean
          example: false
          description: >-
            If set to true, DTMF input will interrupt the agent even when
            interruption_sensitivity is 0. Can be overridden per conversation or
            subagent node. Default to false.
        user_dtmf_options:
          type: object
          properties:
            digit_limit:
              type: number
              description: >-
                The maximum number of digits allowed in the user's DTMF
                (Dual-Tone Multi-Frequency) input per turn. Once this limit is
                reached, the input is considered complete and a response will be
                generated immediately.
              nullable: true
              minimum: 1
              maximum: 50
            termination_key:
              type: string
              nullable: true
              description: >-
                A single key that signals the end of DTMF input. Acceptable
                values include any digit (0-9), the pound/hash symbol (#), or
                the asterisk (*).
              example: '#'
            timeout_ms:
              type: integer
              description: >-
                The time (in milliseconds) to wait for user DTMF input before
                timing out. The timer resets with each digit received.
              minimum: 1000
              maximum: 15000
          nullable: true
        denoising_mode:
          type: string
          enum:
            - no-denoise
            - noise-cancellation
            - noise-and-background-speech-cancellation
          example: noise-cancellation
          description: >-
            If set, determines what denoising mode to use. Use "no-denoise" to
            bypass all audio denoising. Default to noise-cancellation.
        pii_config:
          $ref: '#/components/schemas/PIIConfig'
          description: Configuration for PII scrubbing from transcripts and recordings.
        guardrail_config:
          $ref: '#/components/schemas/GuardrailConfig'
          description: >-
            Configuration for guardrail checks to detect and prevent prohibited
            topics in agent output and user input.
        handbook_config:
          $ref: '#/components/schemas/VoiceHandbookConfig'
          description: >-
            Toggle behavior presets on/off to influence agent response style and
            behaviors.
        timezone:
          type: string
          description: >-
            IANA timezone for the agent (e.g. America/New_York). Defaults to
            America/Los_Angeles if not set.
          example: America/New_York
          nullable: true
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
    ConversationFlowOverride:
      type: object
      description: >-
        Override properties for conversation flow configuration in agent
        override requests.
      properties:
        model_choice:
          $ref: '#/components/schemas/ModelChoice'
          description: The model choice for the conversation flow.
        model_temperature:
          type: number
          minimum: 0
          maximum: 1
          example: 0.7
          description: >-
            Controls the randomness of the model's responses. Lower values make
            responses more deterministic.
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
          example:
            - kb_001
            - kb_002
          description: Knowledge base IDs for RAG (Retrieval-Augmented Generation).
          nullable: true
        kb_config:
          $ref: '#/components/schemas/KBConfig'
          type: object
          description: Knowledge base configuration for RAG retrieval.
        start_speaker:
          type: string
          enum:
            - user
            - agent
          example: agent
          description: Who starts the conversation - user or agent.
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
    ResponseEngine:
      oneOf:
        - $ref: '#/components/schemas/ResponseEngineRetellLm'
        - $ref: '#/components/schemas/ResponseEngineCustomLm'
        - $ref: '#/components/schemas/ResponseEngineConversationFlow'
    LanguageLegacy:
      oneOf:
        - $ref: '#/components/schemas/Language'
        - type: string
          enum:
            - multi
      example: en-US
      description: >-
        Legacy single-string language format. Accepts any concrete locale from
        `Language`, plus the special scalar value `multi` for multilingual
        support. If unset, will use default value `en-US`.
    Language:
      type: string
      example: en-US
      enum:
        - en-US
        - en-IN
        - en-GB
        - en-AU
        - en-NZ
        - de-DE
        - es-ES
        - es-419
        - hi-IN
        - fr-FR
        - fr-CA
        - ja-JP
        - pt-PT
        - pt-BR
        - zh-CN
        - ru-RU
        - it-IT
        - ko-KR
        - nl-NL
        - nl-BE
        - pl-PL
        - tr-TR
        - vi-VN
        - ro-RO
        - bg-BG
        - ca-ES
        - th-TH
        - da-DK
        - fi-FI
        - el-GR
        - hu-HU
        - id-ID
        - no-NO
        - sk-SK
        - sv-SE
        - lt-LT
        - lv-LV
        - cs-CZ
        - ms-MY
        - af-ZA
        - ar-SA
        - az-AZ
        - bs-BA
        - cy-GB
        - fa-IR
        - fil-PH
        - gl-ES
        - he-IL
        - hr-HR
        - hy-AM
        - is-IS
        - kk-KZ
        - kn-IN
        - mk-MK
        - mr-IN
        - ne-NP
        - sl-SI
        - sr-RS
        - sw-KE
        - ta-IN
        - ur-IN
        - yue-CN
        - uk-UA
      description: >-
        Specifies what language (and dialect) the agent will operate in. For
        instance, selecting `en-GB` optimizes speech recognition for British
        English and indexes knowledge bases with English. If unset, will use
        default value `en-US`. This enum does not include the legacy scalar
        value `multi`.
    VoicemailAction:
      oneOf:
        - $ref: '#/components/schemas/VoicemailActionPrompt'
        - $ref: '#/components/schemas/VoicemailActionStaticText'
        - $ref: '#/components/schemas/VoicemailActionHangup'
        - $ref: '#/components/schemas/VoicemailActionBridgeTransfer'
    IvrAction:
      oneOf:
        - $ref: '#/components/schemas/IvrActionHangup'
    CallScreeningOption:
      type: object
      properties:
        agent_identity:
          type: string
          minLength: 1
          maxLength: 100
          example: Acme Health scheduling team
          description: >-
            Identity the agent should provide when a call screen asks who is
            calling. Dynamic variables are supported.
        call_purpose:
          type: string
          minLength: 1
          maxLength: 300
          example: confirming your appointment for tomorrow
          description: >-
            Purpose the agent should provide when a call screen asks why it is
            calling. Dynamic variables are supported.
      required:
        - agent_identity
        - call_purpose
      additionalProperties: false
      description: >-
        If this option is set, the agent prompt will include call screen
        handling instructions for identity and call purpose questions. Set this
        to null to disable call screen prompt instructions.
      nullable: true
    PostCallAnalysisData:
      oneOf:
        - $ref: '#/components/schemas/AnalysisData'
        - $ref: '#/components/schemas/CallPresetAnalysisData'
      description: >-
        Post-call analysis item (custom data or voice preset). Use for voice
        agent post_call_analysis_data; validates only call presets
        (call_summary, call_successful, user_sentiment).
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
    AsrProvider:
      type: string
      enum:
        - azure
        - deepgram
        - soniox
        - assemblyai
      description: ASR provider name.
    PIIConfig:
      type: object
      required:
        - mode
        - categories
      properties:
        mode:
          type: string
          enum:
            - post_call
          default: post_call
          description: >-
            The processing mode for PII scrubbing. Currently only post-call is
            supported.
        categories:
          type: array
          items:
            type: string
            enum:
              - person_name
              - address
              - email
              - phone_number
              - ssn
              - passport
              - driver_license
              - credit_card
              - bank_account
              - password
              - pin
              - medical_id
              - date_of_birth
              - customer_account_number
          uniqueItems: true
          default: []
          description: >-
            List of PII categories to scrub from transcripts and recordings. PII
            redaction is only active when this list is non-empty; an empty array
            means no PII scrubbing is performed.
    GuardrailConfig:
      type: object
      properties:
        output_topics:
          type: array
          items:
            type: string
            enum:
              - harassment
              - self_harm
              - sexual_exploitation
              - violence
              - defense_and_national_security
              - illicit_and_harmful_activity
              - gambling
              - regulated_professional_advice
              - child_safety_and_exploitation
          uniqueItems: true
          description: >-
            Selected prohibited agent topic categories to check. When agent
            messages contain these topics, they will be replaced with a
            placeholder message.
          nullable: true
        input_topics:
          type: array
          items:
            type: string
            enum:
              - platform_integrity_jailbreaking
          uniqueItems: true
          description: >-
            Selected prohibited user topic categories to check. When user
            messages contain these topics, the agent will respond with a
            placeholder message instead of processing the request.
          nullable: true
    VoiceHandbookConfig:
      type: object
      description: Behavior presets for voice agents. All presets are available.
      properties:
        default_personality:
          type: boolean
          description: Professional call center rep baseline.
        conversational_personality:
          type: boolean
          description: >-
            Enables Conversational Personality. When true, the agent uses the
            Conversational Personality handbook preset, skips Professional Rep
            Personality during prompt assembly, and enables internal colloquial
            rewrite behavior.
        natural_filler_words:
          type: boolean
          description: >-
            Sprinkle natural speech fillers like "um", "you know" for a more
            human, conversational tone.
        high_empathy:
          type: boolean
          description: Warm acknowledgment of caller concerns.
        echo_verification:
          type: boolean
          description: Repeat back and confirm important details (voice only).
        nato_phonetic_alphabet:
          type: boolean
          description: Spell using NATO phonetic alphabet style (voice only).
        speech_normalization:
          type: boolean
          description: Convert numbers/dates/currency to spoken forms (voice only).
        smart_matching:
          type: boolean
          description: >-
            Treat near-match similar words as same entity to reduce impact of
            transcription error (voice only).
        ai_disclosure:
          type: boolean
          description: When asked, acknowledge being a virtual assistant.
        scope_boundaries:
          type: boolean
          description: Stay within prompt/context scope, don't invent details.
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
    ModelChoice:
      oneOf:
        - $ref: '#/components/schemas/ModelChoiceCascading'
    ResponseEngineRetellLm:
      type: object
      required:
        - type
        - llm_id
      properties:
        type:
          type: string
          enum:
            - retell-llm
          description: type of the Response Engine.
        llm_id:
          type: string
          description: id of the Retell LLM Response Engine.
        version:
          type: number
          example: 0
          description: Version of the Retell LLM Response Engine.
          nullable: true
    ResponseEngineCustomLm:
      type: object
      required:
        - type
        - llm_websocket_url
      properties:
        type:
          type: string
          enum:
            - custom-llm
          description: type of the Response Engine.
        llm_websocket_url:
          type: string
          description: LLM websocket url of the custom LLM.
    ResponseEngineConversationFlow:
      type: object
      required:
        - type
        - conversation_flow_id
      properties:
        type:
          type: string
          enum:
            - conversation-flow
          description: type of the Response Engine.
        conversation_flow_id:
          type: string
          description: ID of the Conversation Flow Response Engine.
        version:
          type: number
          example: 0
          description: Version of the Conversation Flow Response Engine.
          nullable: true
    VoicemailActionPrompt:
      type: object
      properties:
        type:
          type: string
          enum:
            - prompt
          example: prompt
        text:
          type: string
          example: Summarize the call in 2 sentences.
          description: >-
            The prompt used to generate the text to be spoken when the call is
            detected to be in voicemail.
      required:
        - type
        - text
    VoicemailActionStaticText:
      type: object
      properties:
        type:
          type: string
          enum:
            - static_text
          example: static_text
        text:
          type: string
          example: Please give us a callback tomorrow at 10am.
          description: The text to be spoken when the call is detected to be in voicemail.
      required:
        - type
        - text
    VoicemailActionHangup:
      type: object
      properties:
        type:
          type: string
          enum:
            - hangup
          example: hangup
      required:
        - type
    VoicemailActionBridgeTransfer:
      type: object
      properties:
        type:
          type: string
          enum:
            - bridge_transfer
          example: bridge_transfer
      required:
        - type
    IvrActionHangup:
      type: object
      properties:
        type:
          type: string
          enum:
            - hangup
          example: hangup
      required:
        - type
    AnalysisData:
      oneOf:
        - $ref: '#/components/schemas/StringAnalysisData'
        - $ref: '#/components/schemas/EnumAnalysisData'
        - $ref: '#/components/schemas/BooleanAnalysisData'
        - $ref: '#/components/schemas/NumberAnalysisData'
    CallPresetAnalysisData:
      type: object
      required:
        - type
        - name
      description: >-
        System preset for post-call analysis (voice agents). Use in
        post_call_analysis_data to override prompts or mark fields optional.
      properties:
        type:
          type: string
          enum:
            - system-presets
          description: Identifies this item as a system preset.
        name:
          type: string
          enum:
            - call_summary
            - call_successful
            - user_sentiment
          description: Preset identifier for voice agent analysis.
          example: call_summary
        description:
          type: string
          minLength: 1
          description: Prompt or description for this preset.
        required:
          type: boolean
          description: >-
            If false, this field is optional in the analysis. If true or unset,
            the field is required.
        conditional_prompt:
          type: string
          description: >-
            Optional instruction to help decide whether this field needs to be
            populated. If not set, the field is always included.
    ModelChoiceCascading:
      type: object
      required:
        - type
        - model
      properties:
        type:
          type: string
          enum:
            - cascading
          description: Type of model choice
        model:
          $ref: '#/components/schemas/LLMModel'
          description: The LLM model to use
        high_priority:
          type: boolean
          description: >-
            Whether to use high priority pool with more dedicated resource,
            default false
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
    LLMModel:
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
      description: Available LLM models for agents.
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