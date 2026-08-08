> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Chat Agent

> Create a new chat agent



## OpenAPI

````yaml openapi-final post /create-chat-agent
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
  /create-chat-agent:
    post:
      description: Create a new chat agent
      operationId: createChatAgent
      requestBody:
        required: true
        content:
          application/json:
            schema:
              allOf:
                - $ref: '#/components/schemas/ChatAgentRequest'
                - required:
                    - response_engine
      responses:
        '201':
          description: Successfully created a new chat agent.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ChatAgentResponse'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '402':
          $ref: '#/components/responses/PaymentRequired'
        '500':
          $ref: '#/components/responses/InternalServerError'
      x-codeSamples:
        - lang: JavaScript
          source: |-
            import Retell from 'retell-sdk';

            const client = new Retell({
              apiKey: process.env['RETELL_API_KEY'], // This is the default and can be omitted
            });

            const chatAgentResponse = await client.chatAgent.create({
              response_engine: { llm_id: 'llm_234sdertfsdsfsdf', type: 'retell-llm' },
            });

            console.log(chatAgentResponse.agent_id);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            chat_agent_response = client.chat_agent.create(
                response_engine={
                    "llm_id": "llm_234sdertfsdsfsdf",
                    "type": "retell-llm",
                },
            )
            print(chat_agent_response.agent_id)
components:
  schemas:
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
    PostChatAnalysisData:
      oneOf:
        - $ref: '#/components/schemas/AnalysisData'
        - $ref: '#/components/schemas/ChatPresetAnalysisData'
      description: >-
        Post-chat analysis item (custom data or chat preset). Use for chat agent
        post_chat_analysis_data; validates only chat presets (chat_summary,
        chat_successful, user_sentiment).
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
    AnalysisData:
      oneOf:
        - $ref: '#/components/schemas/StringAnalysisData'
        - $ref: '#/components/schemas/EnumAnalysisData'
        - $ref: '#/components/schemas/BooleanAnalysisData'
        - $ref: '#/components/schemas/NumberAnalysisData'
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