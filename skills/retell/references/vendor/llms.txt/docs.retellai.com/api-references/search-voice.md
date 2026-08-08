> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Search Voice

> Search for community voices from voice providers



## OpenAPI

````yaml openapi-final post /search-community-voice
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
  /search-community-voice:
    post:
      description: Search for community voices from voice providers
      operationId: searchCommunityVoice
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - search_query
              properties:
                voice_provider:
                  type: string
                  enum:
                    - elevenlabs
                    - cartesia
                    - minimax
                    - fish_audio
                  description: Voice provider to search.
                search_query:
                  type: string
                  description: Search query to find voices by name, description, or ID.
      responses:
        '200':
          description: Community voices retrieved successfully
          content:
            application/json:
              schema:
                type: object
                required:
                  - voices
                properties:
                  voices:
                    type: array
                    items:
                      type: object
                      description: Voices retrieved from the provider.
                      properties:
                        provider_voice_id:
                          type: string
                          description: id of the voice from the provider.
                        name:
                          type: string
                          description: Name of the voice.
                        description:
                          type: string
                          description: Description of the voice.
                        public_user_id:
                          type: string
                          description: For elevenlabs only. User id of the voice owner.
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '429':
          $ref: '#/components/responses/TooManyRequests'
        '500':
          $ref: '#/components/responses/InternalServerError'
      x-codeSamples:
        - lang: JavaScript
          source: >-
            import Retell from 'retell-sdk';


            const client = new Retell({
              apiKey: process.env['RETELL_API_KEY'], // This is the default and can be omitted
            });


            const response = await client.voice.search({ search_query:
            'search_query' });


            console.log(response.voices);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            response = client.voice.search(
                search_query="search_query",
            )
            print(response.voices)
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