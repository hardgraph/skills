> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Draft Chat Agent Version

> Create a new draft agent version from a base version.



## OpenAPI

````yaml openapi-final post /create-agent-version/{agent_id}
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
  /create-agent-version/{agent_id}:
    post:
      description: Create a new draft agent version from a base version.
      operationId: createAgentVersion
      parameters:
        - in: path
          name: agent_id
          schema:
            type: string
            minLength: 1
            example: agent_xxx
          required: true
          description: Unique id of the agent.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateAgentVersionRequest'
      responses:
        '201':
          description: Draft version created successfully.
          content:
            application/json:
              schema:
                oneOf:
                  - $ref: '#/components/schemas/AgentResponse'
                  - $ref: '#/components/schemas/ChatAgentResponse'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '404':
          $ref: '#/components/responses/NotFound'
        '422':
          $ref: '#/components/responses/UnprocessableContent'
        '500':
          $ref: '#/components/responses/InternalServerError'
      x-codeSamples:
        - lang: JavaScript
          source: >-
            import Retell from 'retell-sdk';


            const client = new Retell({
              apiKey: process.env['RETELL_API_KEY'], // This is the default and can be omitted
            });


            const response = await client.agent.createVersion('agent_xxx', {
            base_version: 12 });


            console.log(response);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            response = client.agent.create_version(
                agent_id="agent_xxx",
                base_version=12,
            )
            print(response)
components:
  schemas:
    CreateAgentVersionRequest:
      type: object
      additionalProperties: false
      required:
        - base_version
      properties:
        base_version:
          type: integer
          minimum: 0
          example: 12
          description: Existing version used as the base when creating a new draft.
    AgentResponse:
      allOf:
        - type: object
          required:
            - agent_id
            - version
          properties:
            agent_id:
              type: string
              example: oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD
              description: Unique id of agent.
            version:
              type: integer
              example: 0
              description: Version of the agent.
            base_version:
              type: integer
              nullable: true
              example: 12
              description: Version that this draft was based on. Null for initial versions.
            assigned_tags:
              type: array
              items:
                type: string
              description: >-
                Tags assigned to this agent version. Preferred tag is listed
                first.
            is_published:
              type: boolean
              example: false
              description: Whether the agent is published.
        - $ref: '#/components/schemas/AgentRequest'
          required:
            - response_engine
            - voice_id
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
    ChatAgentResponse:
      allOf:
        - type: object
          required:
            - agent_id
          properties:
            agent_id:
              type: string
              example: oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD
              description: Unique id of chat agent.
            version:
              type: integer
              example: 0
              description: The version of the chat agent.
            base_version:
              type: integer
              nullable: true
              example: 12
              description: Version that this draft was based on. Null for initial versions.
            assigned_tags:
              type: array
              items:
                type: string
              description: >-
                Tags assigned to this chat agent version. Preferred tag is
                listed first.
            is_published:
              type: boolean
              example: false
              description: Whether the chat agent is published.
        - $ref: '#/components/schemas/ChatAgentRequest'
          required:
            - response_engine
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
    ChatAgentRequest:
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
          description: The name of the chat agent. Only used for your own reference.
          nullable: true
        version_title:
          type: string
          example: Production hotfix
          description: >-
            Optional title of the chat agent version. Used for your own
            reference.
          nullable: true
        auto_close_message:
          type: string
          example: Thank you for chatting. The conversation has ended.
          description: Message to display when the chat is automatically closed.
          nullable: true
        end_chat_after_silence_ms:
          type: integer
          example: 3600000
          description: >-
            If users stay silent for a period after agent speech, end the chat.
            The minimum value allowed is 120,000 ms (2 minutes). The maximum
            value allowed is 259,200,000 ms (72 hours). By default, this is set
            to 3,600,000 (1 hour).
          nullable: true
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
            The webhook for agent to listen to chat events. See what events it
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
              - chat_started
              - chat_ended
              - chat_analyzed
              - transcript_updated
          description: >-
            Which webhook events this agent should receive. If not set, defaults
            to chat_started, chat_ended, chat_analyzed.
          nullable: true
        webhook_timeout_ms:
          type: integer
          example: 10000
          description: >-
            The timeout for the webhook in milliseconds. If not set, default
            value of 10000 will apply.
        data_storage_setting:
          type: string
          enum:
            - everything
            - everything_except_pii
            - basic_attributes_only
          example: everything
          description: >-
            Controls what data is stored for this agent. "everything" stores all
            data including transcripts and recordings. "everything_except_pii"
            stores data but excludes PII when possible based on PII
            configuration. "basic_attributes_only" stores only basic metadata.
            If not set, defaults to "everything".
          nullable: true
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
            Whether this agent opts in to signed url for public log. If not set,
            default value of false will apply.
        signed_url_expiration_ms:
          type: integer
          example: 86400000
          description: >-
            The expiration time for the signed url in milliseconds. Only
            applicable when opt_in_signed_url is true. If not set, default value
            of 86400000 (24 hours) will apply.
          nullable: true
        post_chat_analysis_data:
          type: array
          items:
            $ref: '#/components/schemas/PostChatAnalysisData'
          description: >-
            Post chat analysis data to extract from the chat. This data will
            augment the pre-defined variables extracted in the chat analysis.
            This will be available after the chat ends.
          nullable: true
        post_chat_analysis_model:
          $ref: '#/components/schemas/NullableLLMModel'
          example: gpt-4.1-mini
          description: The model to use for post chat analysis. Default to gpt-4.1.
        pii_config:
          $ref: '#/components/schemas/PIIConfig'
          description: Configuration for PII scrubbing from transcripts and recordings.
        guardrail_config:
          $ref: '#/components/schemas/GuardrailConfig'
          description: >-
            Configuration for guardrail checks to detect and prevent prohibited
            topics in agent output and user input.
        handbook_config:
          $ref: '#/components/schemas/ChatHandbookConfig'
          description: >-
            Toggle behavior presets on/off to influence agent response style and
            behaviors. Voice-only presets are not available for chat agents.
        timezone:
          type: string
          description: >-
            IANA timezone for the agent (e.g. America/New_York). Defaults to
            America/Los_Angeles if not set.
          example: America/New_York
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
    PostChatAnalysisData:
      oneOf:
        - $ref: '#/components/schemas/AnalysisData'
        - $ref: '#/components/schemas/ChatPresetAnalysisData'
      description: >-
        Post-chat analysis item (custom data or chat preset). Use for chat agent
        post_chat_analysis_data; validates only chat presets (chat_summary,
        chat_successful, user_sentiment).
    ChatHandbookConfig:
      type: object
      description: Behavior presets for chat agents. Voice-only presets are excluded.
      properties:
        default_personality:
          type: boolean
          description: Professional call center rep baseline.
        high_empathy:
          type: boolean
          description: Warm acknowledgment of caller concerns.
        ai_disclosure:
          type: boolean
          description: When asked, acknowledge being a virtual assistant.
        scope_boundaries:
          type: boolean
          description: Stay within prompt/context scope, don't invent details.
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
    ChatPresetAnalysisData:
      type: object
      required:
        - type
        - name
      description: >-
        System preset for post-chat analysis (chat agents). Use in
        post_chat_analysis_data to override prompts or mark fields optional.
      properties:
        type:
          type: string
          enum:
            - system-presets
          description: Identifies this item as a system preset.
        name:
          type: string
          enum:
            - chat_summary
            - chat_successful
            - user_sentiment
          description: Preset identifier for chat agent analysis.
          example: chat_summary
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
    NotFound:
      description: Not Found
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
                example: The requested resource was not found.
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