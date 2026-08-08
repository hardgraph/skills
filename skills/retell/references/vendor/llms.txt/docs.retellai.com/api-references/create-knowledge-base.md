> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Knowledge Base

> Create a new knowledge base

<RequestExample>
  ```javascript Javascript theme={"dark"}
  import Retell from 'retell-sdk';

  const client = new Retell({
    apiKey: 'YOUR_RETELL_API_KEY',
  });

  async function main() {
    const knowledgeBaseResponse = await client.knowledgeBase.create({
      knowledge_base_name: "Sample KB",
      knowledge_base_texts: [
        {
          text: "Hello, how are you?",
          title: "Sample Question",
        },
      ],
      knowledge_base_urls: [
        "https://www.retellai.com",
        "https://docs.retellai.com",
      ],
      knowledge_base_files: [
        fs.createReadStream("../sample.txt"),
      ],
    });

    console.log(knowledgeBaseResponse.knowledge_base_id);
  }

  main();
  ```

  ```Python Python theme={"dark"}
  from retell import Retell

  client = Retell(
      api_key="YOUR_RETELL_API_KEY",
  )

  file = open("./test_data.txt", "rb")
  knowledge_base_response = client.knowledge_base.create(
      knowledge_base_name="Sample KB",
      knowledge_base_texts=[
          {
              "text": "Hello, how are you?",
              "title": "Sample Question",
          },
      ],
      knowledge_base_urls=[
        "https://www.retellai.com",
        "https://docs.retellai.com",
      ],
      knowledge_base_files=[file],
  )
  print(knowledge_base_response.knowledge_base_id)
  file.close()
  ```

  ```bash cURL theme={"dark"}
  curl --request POST \
    --url https://api.retellai.com/create-knowledge-base \
    --header 'Authorization: Bearer <token>' \
    --form 'knowledge_base_name=Sample KB' \
    --form 'knowledge_base_texts=[{"title": "Sample Question", "text": "Hello, how are you?"}]' \
    --form 'knowledge_base_urls=["https://www.retellai.com", "https://docs.retellai.com"]' \
    --form 'knowledge_base_files=@./test_data.txt'
  ```
</RequestExample>


## OpenAPI

````yaml openapi-final post /create-knowledge-base
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
  /create-knowledge-base:
    post:
      description: Create a new knowledge base
      operationId: createKnowledgeBase
      requestBody:
        required: true
        content:
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/KnowledgeBaseRequest'
      responses:
        '201':
          description: Successfully created a new knowledge base.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/KnowledgeBaseResponse'
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

            const knowledgeBaseResponse = await client.knowledgeBase.create({
              knowledge_base_name: 'Sample KB',
            });

            console.log(knowledgeBaseResponse.knowledge_base_id);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            knowledge_base_response = client.knowledge_base.create(
                knowledge_base_name="Sample KB",
            )
            print(knowledge_base_response.knowledge_base_id)
components:
  schemas:
    KnowledgeBaseRequest:
      type: object
      required:
        - knowledge_base_name
      properties:
        knowledge_base_name:
          type: string
          example: Sample KB
          description: Name of the knowledge base. Must be less than 40 characters.
        knowledge_base_texts:
          type: array
          items:
            type: object
            required:
              - title
              - text
            properties:
              title:
                type: string
                description: Title of the text.
              text:
                type: string
                description: Text to add to the knowledge base.
          description: Texts to add to the knowledge base.
        knowledge_base_files:
          type: array
          items:
            type: string
            format: binary
          description: >-
            Files to add to the knowledge base. Limit to 25 files, where each
            file is limited to 50MB.
        knowledge_base_urls:
          type: array
          items:
            type: string
          example:
            - https://www.example.com
            - https://www.retellai.com
          description: >-
            URLs to be scraped and added to the knowledge base. Must be valid
            urls.
        enable_auto_refresh:
          type: boolean
          example: true
          description: >-
            Whether to enable auto refresh for the knowledge base urls. If set
            to true, will retrieve the data from the specified url every 12
            hours.
        max_chunk_size:
          type: integer
          minimum: 600
          maximum: 6000
          example: 2000
          description: >-
            Maximum number of characters per chunk when splitting knowledge
            base. Default is 2000. content. Immutable after creation.
        min_chunk_size:
          type: integer
          minimum: 200
          maximum: 2000
          example: 400
          description: >-
            Minimum number of characters per chunk. Chunks smaller than this
            will be merged with adjacent chunks. Must be less than
            max_chunk_size. Immutable after creation. Default is 400.
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