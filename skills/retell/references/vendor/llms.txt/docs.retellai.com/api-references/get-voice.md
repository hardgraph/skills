> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Voice

> Retrieve details of a specific voice



## OpenAPI

````yaml openapi-final get /get-voice/{voice_id}
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
  /get-voice/{voice_id}:
    get:
      description: Retrieve details of a specific voice
      operationId: getVoice
      parameters:
        - in: path
          name: voice_id
          schema:
            type: string
            example: retell-Cimo
          required: true
          description: Unique id for the voice.
      responses:
        '200':
          description: Successfully retrieved a voice.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/VoiceResponse'
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

            const voiceResponse = await client.voice.retrieve('retell-Cimo');

            console.log(voiceResponse.provider);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            voice_response = client.voice.retrieve(
                "retell-Cimo",
            )
            print(voice_response.provider)
components:
  schemas:
    VoiceResponse:
      type: object
      required:
        - voice_id
        - voice_name
        - provider
        - gender
      properties:
        voice_id:
          type: string
          example: retell-Cimo
          description: Unique id for the voice.
        voice_name:
          type: string
          example: Adrian
          description: Name of the voice.
        provider:
          type: string
          enum:
            - elevenlabs
            - openai
            - cartesia
            - minimax
            - fish_audio
            - platform
          example: elevenlabs
          description: Indicates the provider of voice.
        accent:
          type: string
          example: American
          description: Accent annotation of the voice.
        gender:
          type: string
          enum:
            - male
            - female
          example: male
          description: Gender of voice.
        age:
          type: string
          example: Young
          description: Age annotation of the voice.
        preview_audio_url:
          type: string
          example: https://retell-utils-public.s3.us-west-2.amazonaws.com/adrian.mp3
          description: URL to the preview audio of the voice.
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