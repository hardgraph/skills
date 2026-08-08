> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Clone Voice

> Clone a voice from audio files



## OpenAPI

````yaml openapi-final post /clone-voice
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
  /clone-voice:
    post:
      description: Clone a voice from audio files
      operationId: cloneVoice
      requestBody:
        required: true
        content:
          multipart/form-data:
            schema:
              type: object
              required:
                - files
                - voice_name
                - voice_provider
              properties:
                files:
                  type: array
                  items:
                    type: string
                    format: binary
                  description: >-
                    Audio files to use for voice cloning. Up to 25 files
                    allowed. For Cartesia and MiniMax, only 1 file is supported.
                    For Inworld, up to 3 files are supported.
                voice_name:
                  type: string
                  minLength: 1
                  maxLength: 200
                  description: Name for the cloned voice
                voice_provider:
                  type: string
                  enum:
                    - elevenlabs
                    - cartesia
                    - minimax
                    - fish_audio
                    - platform
                  description: Voice provider to use for cloning.
      responses:
        '201':
          description: Voice cloned successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/VoiceResponse'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '402':
          $ref: '#/components/responses/PaymentRequired'
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

            const voiceResponse = await client.voice.clone({
              files: [fs.createReadStream('path/to/file')],
              voice_name: 'x',
              voice_provider: 'elevenlabs',
            });

            console.log(voiceResponse.provider);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            voice_response = client.voice.clone(
                files=[b"Example data"],
                voice_name="x",
                voice_provider="elevenlabs",
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