> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Concurrency

> Get the current concurrency and concurrency limit of the org



## OpenAPI

````yaml openapi-final get /get-concurrency
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
  /get-concurrency:
    get:
      description: Get the current concurrency and concurrency limit of the org
      operationId: getConcurrency
      responses:
        '200':
          description: Successfully retrieved concurrency information.
          content:
            application/json:
              schema:
                type: object
                properties:
                  current_concurrency:
                    type: integer
                    example: 10
                    description: >-
                      The current concurrency (amount of ongoing calls) of the
                      org.
                  concurrency_limit:
                    type: integer
                    example: 100
                    description: >-
                      The total concurrency limit (at max how many ongoing calls
                      one can make) of the org. This should be the sum of
                      `base_concurrency` and `purchased_concurrency`.
                  base_concurrency:
                    type: integer
                    example: 20
                    description: The free concurrency limit of the org.
                  purchased_concurrency:
                    type: integer
                    example: 80
                    description: >-
                      The amount of concurrency that the org has already
                      purchased.
                  concurrency_purchase_limit:
                    type: integer
                    example: 100
                    description: >-
                      The maximum amount of concurrency that the org can
                      purchase.
                  remaining_purchase_limit:
                    type: integer
                    example: 20
                    description: >-
                      The remaining amount of concurrency that the org can
                      purchase. This is the difference between
                      `concurrency_purchase_limit` and `purchased_concurrency`.
                  reserved_inbound_concurrency:
                    type: integer
                    example: 10
                    description: >-
                      Number of normal concurrency slots reserved for inbound
                      calls.
                  concurrency_burst_enabled:
                    type: boolean
                    example: true
                    description: >-
                      Whether burst concurrency mode is enabled. When enabled,
                      allows the org to exceed their normal concurrency limit
                      with a surcharge.
                  concurrency_burst_limit:
                    type: integer
                    example: 60
                    readOnly: true
                    description: >-
                      The maximum concurrency limit when burst mode is enabled.
                      This is calculated as min(3x normal limit, normal limit +
                      300). Returns 0 if burst mode is disabled.
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

            const concurrency = await client.concurrency.retrieve();

            console.log(concurrency.base_concurrency);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            concurrency = client.concurrency.retrieve()
            print(concurrency.base_concurrency)
components:
  responses:
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