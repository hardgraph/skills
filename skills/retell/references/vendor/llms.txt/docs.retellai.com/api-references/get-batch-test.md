> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Batch Test

> Get a batch test job by ID



## OpenAPI

````yaml openapi-final get /get-batch-test/{test_case_batch_job_id}
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
  /get-batch-test/{test_case_batch_job_id}:
    get:
      description: Get a batch test job by ID
      operationId: getBatchTest
      parameters:
        - in: path
          name: test_case_batch_job_id
          schema:
            type: string
          required: true
          description: ID of the batch test job to retrieve
      responses:
        '200':
          description: Batch test job retrieved successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TestCaseBatchJob'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '404':
          $ref: '#/components/responses/NotFound'
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


            const batchTestResponse = await
            client.tests.getBatchTest('test_case_batch_job_id');


            console.log(batchTestResponse.test_case_batch_job_id);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            batch_test_response = client.tests.get_batch_test(
                "test_case_batch_job_id",
            )
            print(batch_test_response.test_case_batch_job_id)
components:
  schemas:
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