> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Phone Number

> Buy a new phone number & Bind agents



## OpenAPI

````yaml openapi-final post /create-phone-number
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
  /create-phone-number:
    post:
      description: Buy a new phone number & Bind agents
      operationId: createPhoneNumber
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                inbound_agents:
                  type: array
                  items:
                    $ref: '#/components/schemas/AgentWeight'
                  description: >-
                    Inbound agents to bind to the number with weights. If set
                    and non-empty, one agent will be picked randomly for each
                    inbound call, with probability proportional to the weight.
                    Total weights must add up to 1.
                  nullable: true
                outbound_agents:
                  type: array
                  items:
                    $ref: '#/components/schemas/AgentWeight'
                  description: >-
                    Outbound agents to bind to the number with weights. If set
                    and non-empty, one agent will be picked randomly for each
                    outbound call, with probability proportional to the weight.
                    Total weights must add up to 1.
                  nullable: true
                area_code:
                  type: integer
                  example: 415
                  description: >-
                    Area code of the number to obtain. Format is a 3 digit
                    integer. Currently only supports US area code.
                nickname:
                  type: string
                  example: Frontdesk Number
                  description: Nickname of the number. This is for your reference only.
                inbound_webhook_url:
                  type: string
                  example: https://example.com/inbound-webhook
                  description: >-
                    If set, Retell will send a webhook for inbound calls, where
                    you can override the agent ID, set dynamic variables, reject
                    the call, and configure other fields specific to that call.
                  nullable: true
                allowed_inbound_country_list:
                  type: array
                  items:
                    type: string
                  example:
                    - US
                    - CA
                    - GB
                  description: >-
                    List of ISO 3166-1 alpha-2 country codes from which inbound
                    calls are allowed. If not set or empty, calls from all
                    countries are allowed.
                  nullable: true
                allowed_outbound_country_list:
                  type: array
                  items:
                    type: string
                  example:
                    - US
                    - CA
                  description: >-
                    List of ISO 3166-1 alpha-2 country codes to which outbound
                    calls are allowed. If not set or empty, calls to all
                    countries are allowed.
                  nullable: true
                number_provider:
                  type: string
                  enum:
                    - twilio
                    - telnyx
                  example: twilio
                  description: >-
                    The provider to purchase the phone number from. Default to
                    twilio.
                country_code:
                  type: string
                  enum:
                    - US
                    - CA
                  example: US
                  description: >-
                    The ISO 3166-1 alpha-2 country code of the number you are
                    trying to purchase. If left empty, will default to "US".
                toll_free:
                  type: boolean
                  description: >-
                    Whether to purchase a toll-free number. Toll-free numbers
                    incur higher costs.
                phone_number:
                  type: string
                  minLength: 1
                  example: '+14157774444'
                  description: >-
                    The number you are trying to purchase in E.164 format of the
                    number (+country code then number with no space and no
                    special characters).
                transport:
                  type: string
                  example: TCP
                  description: >-
                    Outbound transport protocol to use for the phone number.
                    Valid values are "TLS", "TCP" and "UDP". Default is "TCP".
                  nullable: true
                fallback_number:
                  type: string
                  example: '+14155551234'
                  description: >-
                    When inbound call concurrency is reached and a slot does not
                    free up after extended ringing, the call will fall back to
                    this number. Can be either a Retell phone number or an
                    external number. Cannot be the same as this phone number,
                    and cannot be a number that already has its own fallback
                    configured (prevents nested forwarding).
                  nullable: true
      responses:
        '201':
          description: Successfully created a new number.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/PhoneNumberResponse'
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

            const phoneNumberResponse = await client.phoneNumber.create();

            console.log(phoneNumberResponse.last_modification_timestamp);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            phone_number_response = client.phone_number.create()
            print(phone_number_response.last_modification_timestamp)
components:
  schemas:
    AgentWeight:
      type: object
      required:
        - agent_id
        - weight
      properties:
        agent_id:
          type: string
          minLength: 1
          example: oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD
        agent_version:
          $ref: '#/components/schemas/AgentVersionReference'
        weight:
          type: number
          example: 0.5
          minimum: 0
          exclusiveMinimum: true
          maximum: 1
          description: >-
            The weight of the agent. When used in a list of agents, the total
            weights must add up to 1.
    PhoneNumberResponse:
      type: object
      required:
        - phone_number
        - phone_number_type
        - last_modification_timestamp
      properties:
        phone_number:
          type: string
          example: '+14157774444'
          description: >-
            E.164 format of the number (+country code, then number with no
            space, no special characters), used as the unique identifier for
            phone number APIs.
        phone_number_type:
          type: string
          enum:
            - retell-twilio
            - retell-telnyx
            - custom
          example: retell-twilio
          description: Type of the phone number.
        phone_number_pretty:
          type: string
          example: +1 (415) 777-4444
          description: Pretty printed phone number, provided for your reference.
        allowed_inbound_country_list:
          type: array
          items:
            type: string
          example:
            - US
            - CA
            - GB
          description: >-
            List of ISO 3166-1 alpha-2 country codes from which inbound calls
            are allowed. If not set or empty, calls from all countries are
            allowed.
          nullable: true
        allowed_outbound_country_list:
          type: array
          items:
            type: string
          example:
            - US
            - CA
          description: >-
            List of ISO 3166-1 alpha-2 country codes to which outbound calls are
            allowed. If not set or empty, calls to all countries are allowed.
          nullable: true
        area_code:
          type: integer
          example: 415
          description: >-
            Area code of the number to obtain. Format is a 3 digit integer.
            Currently only supports US area code.
        inbound_agents:
          type: array
          items:
            $ref: '#/components/schemas/AgentWeight'
          description: >-
            Inbound agents to bind to the number with weights. If set and
            non-empty, one agent will be picked randomly for each inbound call,
            with probability proportional to the weight. Total weights must add
            up to 1.
          nullable: true
        outbound_agents:
          type: array
          items:
            $ref: '#/components/schemas/AgentWeight'
          description: >-
            Outbound agents to bind to the number with weights. If set and
            non-empty, one agent will be picked randomly for each outbound call,
            with probability proportional to the weight. Total weights must add
            up to 1.
          nullable: true
        inbound_sms_agents:
          type: array
          items:
            $ref: '#/components/schemas/AgentWeight'
          description: >-
            Inbound SMS agents to bind to the number with weights. If set and
            non-empty, one agent will be picked randomly for each inbound SMS,
            with probability proportional to the weight. Total weights must add
            up to 1.
          nullable: true
        outbound_sms_agents:
          type: array
          items:
            $ref: '#/components/schemas/AgentWeight'
          description: >-
            Outbound SMS agents to bind to the number with weights. If set and
            non-empty, one agent will be picked randomly for each outbound SMS,
            with probability proportional to the weight. Total weights must add
            up to 1.
          nullable: true
        nickname:
          type: string
          example: Frontdesk Number
          description: Nickname of the number. This is for your reference only.
          nullable: true
        inbound_webhook_url:
          type: string
          example: https://example.com/inbound-webhook
          description: >-
            If set, Retell will send a webhook for inbound calls, where you can
            override the agent ID, set dynamic variables, reject the call, and
            configure other fields specific to that call.
          nullable: true
        inbound_sms_webhook_url:
          type: string
          example: https://example.com/inbound-sms-webhook
          description: >-
            If set, Retell will send a webhook for inbound SMS, where you can
            override the agent ID, set dynamic variables, reject the SMS, and
            configure other fields specific to that chat.
          nullable: true
        last_modification_timestamp:
          type: integer
          example: 1703413636133
          description: >-
            Last modification timestamp (milliseconds since epoch). Either the
            time of last update or creation if no updates available.
        sip_outbound_trunk_config:
          type: object
          nullable: true
          properties:
            termination_uri:
              type: string
              example: someuri.pstn.twilio.com
              nullable: true
              description: The termination URI for the SIP trunk for the phone number.
            auth_username:
              type: string
              example: username
              nullable: true
              description: >-
                The username used for authenticating the SIP trunk for the phone
                number.
            transport:
              type: string
              example: TCP
              nullable: true
              description: >-
                Outbound transport protocol for the SIP trunk for the phone
                number. Valid values are "TLS", "TCP" and "UDP". Default is
                "TCP".
        fallback_number:
          type: string
          example: '+14155551234'
          description: >-
            When inbound call concurrency is reached and a slot does not free up
            after extended ringing, the call will fall back to this number. Can
            be either a Retell phone number or an external number. Cannot be the
            same as this phone number, and cannot be a number that already has
            its own fallback configured (prevents nested forwarding).
          nullable: true
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