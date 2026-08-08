> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# List Batch Tests

> List batch test jobs with pagination



## OpenAPI

````yaml openapi-final get /v2/list-batch-tests
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
  /v2/list-batch-tests:
    get:
      description: List batch test jobs with pagination
      operationId: listBatchTests
      parameters:
        - in: query
          name: type
          schema:
            type: string
            enum:
              - retell-llm
              - conversation-flow
          required: true
          description: Type of response engine
        - in: query
          name: llm_id
          schema:
            type: string
          description: LLM ID (required when type is retell-llm)
        - in: query
          name: conversation_flow_id
          schema:
            type: string
          description: Conversation flow ID (required when type is conversation-flow)
        - in: query
          name: version
          schema:
            type: integer
          description: Version of the response engine (defaults to latest)
        - $ref: '#/components/parameters/LimitParam'
        - $ref: '#/components/parameters/PaginationKeyParam'
      responses:
        '200':
          description: Batch test jobs retrieved successfully
          content:
            application/json:
              schema:
                allOf:
                  - $ref: '#/components/schemas/PaginatedResponseBase'
                  - type: object
                    properties:
                      items:
                        type: array
                        items:
                          $ref: '#/components/schemas/TestCaseBatchJob'
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


            const response = await client.tests.listBatchTests({ type:
            'retell-llm' });


            console.log(response.has_more);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            response = client.tests.list_batch_tests(
                type="retell-llm",
            )
            print(response.has_more)
components:
  parameters:
    LimitParam:
      in: query
      name: limit
      schema:
        type: integer
        default: 50
        maximum: 1000
      description: Maximum number of items to return.
    PaginationKeyParam:
      in: query
      name: pagination_key
      schema:
        type: string
      description: Pagination key for fetching the next page.
  schemas:
    PaginatedResponseBase:
      type: object
      properties:
        pagination_key:
          type: string
          description: Pagination key for the next page.
        has_more:
          type: boolean
          description: Whether more results are available.
    TestCaseBatchJob:
      type: object
      required:
        - test_case_batch_job_id
        - status
        - response_engine
        - pass_count
        - fail_count
        - error_count
        - total_count
        - creation_timestamp
        - user_modified_timestamp
      properties:
        test_case_batch_job_id:
          type: string
          description: Unique identifier for the test case batch job
        status:
          type: string
          enum:
            - in_progress
            - complete
          description: Status of the batch job
        response_engine:
          $ref: '#/components/schemas/ResponseEngine'
        pass_count:
          type: integer
          description: Number of test cases that passed
          minimum: 0
        fail_count:
          type: integer
          description: Number of test cases that failed
          minimum: 0
        error_count:
          type: integer
          description: Number of test cases that encountered errors
          minimum: 0
        total_count:
          type: integer
          description: Total number of test cases in the batch
          minimum: 0
        creation_timestamp:
          type: integer
          description: Timestamp when the batch job was created (milliseconds since epoch)
        user_modified_timestamp:
          type: integer
          description: >-
            Timestamp when the batch job was last modified (milliseconds since
            epoch)
    ResponseEngine:
      oneOf:
        - $ref: '#/components/schemas/ResponseEngineRetellLm'
        - $ref: '#/components/schemas/ResponseEngineCustomLm'
        - $ref: '#/components/schemas/ResponseEngineConversationFlow'
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