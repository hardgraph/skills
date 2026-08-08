> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Delete Phone Number

> Delete an existing phone number



## OpenAPI

````yaml openapi-final delete /delete-phone-number/{phone_number}
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
  /delete-phone-number/{phone_number}:
    delete:
      description: Delete an existing phone number
      operationId: deletePhoneNumber
      parameters:
        - in: path
          name: phone_number
          schema:
            type: string
            minLength: 1
            example: '+14157774444'
          required: true
          description: >-
            E.164 format of the number (+country code, then number with no
            space, no special characters), used as the unique identifier for
            phone number APIs.
      responses:
        '204':
          description: No Content
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

            await client.phoneNumber.delete('+14157774444');
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            client.phone_number.delete(
                "+14157774444",
            )
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