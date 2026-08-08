> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Knowledge Base

> Retrieve details of a specific knowledge base



## OpenAPI

````yaml openapi-final get /get-knowledge-base/{knowledge_base_id}
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
  /get-knowledge-base/{knowledge_base_id}:
    get:
      description: Retrieve details of a specific knowledge base
      operationId: getKnowledgeBase
      parameters:
        - in: path
          name: knowledge_base_id
          schema:
            type: string
            example: kb_1234567890
          required: true
          description: The knowledge base id to retrieve.
      responses:
        '200':
          description: Successfully retrieved a knowledge base.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/KnowledgeBaseResponse'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '422':
          $ref: '#/components/responses/UnprocessableContent'
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


            const knowledgeBaseResponse = await
            client.knowledgeBase.retrieve('kb_1234567890');


            console.log(knowledgeBaseResponse.knowledge_base_id);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            knowledge_base_response = client.knowledge_base.retrieve(
                "kb_1234567890",
            )
            print(knowledge_base_response.knowledge_base_id)
components:
  schemas:
    KnowledgeBaseResponse:
      type: object
      required:
        - knowledge_base_id
        - knowledge_base_name
        - status
      properties:
        knowledge_base_id:
          type: string
          example: knowledge_base_a456426614174000
          description: Unique id of the knowledge base.
        knowledge_base_name:
          type: string
          example: Sample KB
          description: Name of the knowledge base. Must be less than 40 characters.
        status:
          type: string
          enum:
            - in_progress
            - complete
            - error
            - refreshing_in_progress
          example: in_progress
          description: >-
            Status of the knowledge base. When it's created and being processed,
            it's "in_progress". When the processing is done, it's "complete".
            When there's an error in processing, it's "error". When it is during
            kb updating, it's "refreshing_in_progress".
        max_chunk_size:
          type: integer
          example: 2000
          description: >-
            Maximum number of characters per chunk when splitting knowledge base
            content.
        min_chunk_size:
          type: integer
          example: 400
          description: >-
            Minimum number of characters per chunk. Chunks smaller than this are
            merged with adjacent chunks.
        knowledge_base_sources:
          type: array
          items:
            oneOf:
              - $ref: '#/components/schemas/KnowledgeBaseSourceDocument'
              - $ref: '#/components/schemas/KnowledgeBaseSourceText'
              - $ref: '#/components/schemas/KnowledgeBaseSourceUrl'
          description: >-
            Sources of the knowledge base. Will be populated after the
            processing is done (when status is "complete").
        enable_auto_refresh:
          type: boolean
          example: true
          description: >-
            Whether to enable auto refresh for the knowledge base urls. If set
            to true, will retrieve the data from the specified url every 12
            hours.
        last_refreshed_timestamp:
          type: integer
          example: 1703413636133
          description: >-
            Last refreshed timestamp (milliseconds since epoch). Only applicable
            when enable_auto_refresh is true.
    KnowledgeBaseSourceDocument:
      type: object
      required:
        - type
        - source_id
        - filename
        - file_url
        - file_size
      properties:
        type:
          type: string
          enum:
            - document
          description: Type of the knowledge base source.
        source_id:
          type: string
          description: Unique id of the knowledge base source.
        filename:
          type: string
          description: Filename of the document.
        file_url:
          type: string
          description: URL of the document stored.
        file_size:
          type: number
          description: File size in bytes.
    KnowledgeBaseSourceText:
      type: object
      required:
        - type
        - source_id
        - title
        - content_url
      properties:
        type:
          type: string
          enum:
            - text
          description: Type of the knowledge base source.
        source_id:
          type: string
          description: Unique id of the knowledge base source.
        title:
          type: string
          description: Title of the text.
        content_url:
          type: string
          description: URL of the text content stored.
    KnowledgeBaseSourceUrl:
      type: object
      required:
        - type
        - source_id
        - url
      properties:
        type:
          type: string
          enum:
            - url
          description: Type of the knowledge base source.
        source_id:
          type: string
          description: Unique id of the knowledge base source.
        url:
          type: string
          description: URL used to be scraped and added to the knowledge base.
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