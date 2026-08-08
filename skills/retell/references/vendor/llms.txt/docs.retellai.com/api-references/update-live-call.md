> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Update Live Call

> Update an ongoing call at runtime. Supports overriding dynamic variables, metadata, and the data storage setting on the running call, and controlling the live agent (inject context, trigger a response). These overrides take effect immediately on the live call; metadata and data storage setting changes are also persisted to the call record. To update a call that is no longer ongoing, use /v2/update-call/{call_id}.



## OpenAPI

````yaml openapi-final patch /v2/update-live-call/{call_id}
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
  /v2/update-live-call/{call_id}:
    patch:
      description: >-
        Update an ongoing call at runtime. Supports overriding dynamic
        variables, metadata, and the data storage setting on the running call,
        and controlling the live agent (inject context, trigger a response).
        These overrides take effect immediately on the live call; metadata and
        data storage setting changes are also persisted to the call record. To
        update a call that is no longer ongoing, use /v2/update-call/{call_id}.
      operationId: updateLiveCall
      parameters:
        - in: path
          name: call_id
          schema:
            type: string
            example: call_a4441234567890777c4a4a123e6
          required: true
          description: The call id of the ongoing call to be updated.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                fields_to_override:
                  type: object
                  description: >
                    Call fields to override on the running call. Each field is
                    applied to the live call immediately; omitted fields are
                    left unchanged.
                  properties:
                    override_dynamic_variables:
                      type: object
                      additionalProperties:
                        type: string
                      example:
                        additional_discount: 15%
                      description: >-
                        Override dynamic variables represented as key-value
                        pairs of strings. Setting this will override or add the
                        dynamic variables set in the agent during the call. Only
                        need to set the delta where you want to override, no
                        need to set the entire dynamic variables object. Setting
                        this to null will remove any existing override.
                      nullable: true
                    metadata:
                      type: object
                      description: >-
                        An arbitrary object for storage purpose only. Overrides
                        the metadata on the call. Size limited to 50kB max.
                      example:
                        customer_id: cust_123
                        notes: Follow-up required
                    data_storage_setting:
                      type: string
                      enum:
                        - everything
                        - everything_except_pii
                        - basic_attributes_only
                      description: >-
                        Data storage setting for this call. Overrides the
                        agent's default setting. "everything" stores all data,
                        "everything_except_pii" excludes PII when possible,
                        "basic_attributes_only" stores only metadata. Cannot be
                        downgraded from more restrictive to less restrictive
                        settings.
                      example: everything_except_pii
                  additionalProperties: false
                call_control:
                  type: object
                  description: >
                    Live agent control. At least one of `additional_context` or
                    `trigger_response` should be supplied; an empty object is a
                    no-op.
                  properties:
                    trigger_response:
                      type: boolean
                      description: >
                        Only `true` has an effect. When set, if the agent is
                        currently speaking the response is interrupted and a new
                        one is generated. If the agent has already finished
                        speaking and is waiting silently for the user, the agent
                        is nudged to produce another response. If the user is
                        currently speaking, this field is a no-op so the agent
                        does not talk over them. This field respects the agent's
                        `interruption_sensitivity`: when sensitivity is `0` the
                        agent's current speech is treated as uninterruptible, so
                        `trigger_response` is a no-op while the agent is
                        speaking. Omitting or setting `false` leaves the call
                        untouched.
                    additional_context:
                      type: string
                      minLength: 1
                      description: >
                        Free-form text appended to the call transcript with role
                        "injected" and injected into the next agent response
                        context. Must be non-empty.
                  additionalProperties: false
              additionalProperties: false
            example:
              fields_to_override:
                override_dynamic_variables:
                  additional_discount: 15%
              call_control:
                additional_context: Customer just opened a support ticket about billing.
                trigger_response: true
      responses:
        '200':
          description: Live call updated successfully
          content:
            application/json:
              schema:
                type: object
                required:
                  - success
                properties:
                  success:
                    type: boolean
                    example: true
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


            const response = await
            client.call.updateLive('call_a4441234567890777c4a4a123e6', {
              call_control: {
                additional_context: 'Customer just opened a support ticket about billing.',
                trigger_response: true,
              },
              fields_to_override: { override_dynamic_variables: { additional_discount: '15%' } },
            });


            console.log(response.success);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            response = client.call.update_live(
                call_id="call_a4441234567890777c4a4a123e6",
                call_control={
                    "additional_context": "Customer just opened a support ticket about billing.",
                    "trigger_response": True,
                },
                fields_to_override={
                    "override_dynamic_variables": {
                        "additional_discount": "15%"
                    }
                },
            )
            print(response.success)
components:
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