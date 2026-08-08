> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# List Test Runs

> List test case jobs (test runs) for a batch test job with pagination



## OpenAPI

````yaml openapi-final get /v2/list-test-runs/{test_case_batch_job_id}
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
  /v2/list-test-runs/{test_case_batch_job_id}:
    get:
      description: List test case jobs (test runs) for a batch test job with pagination
      operationId: listTestRuns
      parameters:
        - in: path
          name: test_case_batch_job_id
          schema:
            type: string
          required: true
          description: ID of the batch test job
        - $ref: '#/components/parameters/LimitParam'
        - $ref: '#/components/parameters/PaginationKeyParam'
      responses:
        '200':
          description: Test case jobs retrieved successfully
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
                          $ref: '#/components/schemas/TestCaseJob'
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


            const response = await
            client.tests.listTestRuns('test_case_batch_job_id');


            console.log(response.has_more);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            response = client.tests.list_test_runs(
                test_case_batch_job_id="test_case_batch_job_id",
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
    TestCaseJob:
      type: object
      required:
        - test_case_job_id
        - status
        - test_case_definition_id
        - test_case_definition_snapshot
        - creation_timestamp
        - user_modified_timestamp
      properties:
        test_case_job_id:
          type: string
          description: Unique identifier for the test case job
        status:
          type: string
          enum:
            - pending
            - in_progress
            - pass
            - fail
            - error
          description: >-
            Status of the test case job. `pending` means the run is queued but
            has not started yet; it becomes `in_progress` once a worker picks it
            up, then resolves to `pass`, `fail`, or `error`.
        test_case_definition_id:
          type: string
          description: ID of the test case definition used
        test_case_definition_snapshot:
          $ref: '#/components/schemas/TestCaseDefinition'
          description: Snapshot of the test case definition at time of execution
        transcript_snapshot:
          type: object
          nullable: true
          description: >-
            Snapshot of the transcript generated during test execution. Can be
            either ConversationFlowPlaygroundSnapshot or
            RetellLlmPlaygroundSnapshot
        result_explanation:
          type: string
          nullable: true
          description: Explanation of the test result
        creation_timestamp:
          type: integer
          description: >-
            Timestamp when the test case job was created (milliseconds since
            epoch)
        user_modified_timestamp:
          type: integer
          description: >-
            Timestamp when the test case job was last modified (milliseconds
            since epoch)
    TestCaseDefinition:
      allOf:
        - $ref: '#/components/schemas/TestCaseDefinitionInput'
        - type: object
          required:
            - name
            - response_engine
            - metrics
            - user_prompt
            - dynamic_variables
            - tool_mocks
            - llm_model
            - test_case_definition_id
            - type
            - creation_timestamp
            - user_modified_timestamp
          properties:
            test_case_definition_id:
              type: string
              description: Unique identifier for the test case definition
            type:
              type: string
              enum:
                - simulation
              description: Type of test case definition
            creation_timestamp:
              type: integer
              description: >-
                Timestamp when the test case definition was created
                (milliseconds since epoch)
            user_modified_timestamp:
              type: integer
              description: >-
                Timestamp when the test case definition was last modified
                (milliseconds since epoch)
    TestCaseDefinitionInput:
      type: object
      properties:
        name:
          type: string
          description: Name of the test case definition
        response_engine:
          $ref: '#/components/schemas/RetellResponseEngine'
          description: >-
            Response engine to use for the test case. Custom LLM is not
            supported.
        user_prompt:
          type: string
          description: User prompt to simulate in the test case
        metrics:
          type: array
          items:
            type: string
          description: Array of metric names to evaluate
        dynamic_variables:
          type: object
          additionalProperties:
            type: string
          description: Dynamic variables to inject into the response engine
        tool_mocks:
          type: array
          items:
            $ref: '#/components/schemas/ToolMock'
          description: Mock tool calls for testing
        llm_model:
          $ref: '#/components/schemas/LLMModel'
          description: LLM model to use for simulation
    RetellResponseEngine:
      oneOf:
        - $ref: '#/components/schemas/ResponseEngineRetellLm'
        - $ref: '#/components/schemas/ResponseEngineConversationFlow'
      description: Response engine for test cases. Custom LLM is not supported.
    ToolMock:
      description: >-
        A fake response for one tool. During a simulation, when the LLM calls a
        tool whose name matches `tool_name` and whose arguments satisfy
        `input_match_rule`, the real tool is not run; `output` is returned to
        the LLM instead. This keeps runs deterministic and avoids calling live
        integrations. A tool call that matches no mock falls through to the real
        tool.
      type: object
      required:
        - tool_name
        - input_match_rule
        - output
      properties:
        tool_name:
          type: string
          description: >-
            The tool's function name, not the tool ID, i.e. the name the LLM
            uses when it calls the tool (for example `check_availability_cal`,
            `book_appointment_cal`, or the name you gave a custom function).
        input_match_rule:
          $ref: '#/components/schemas/ToolMockInputMatchRule'
          description: Decides which calls to this tool the mock applies to.
        output:
          type: string
          maxLength: 15000
          description: >-
            The tool result fed back to the LLM in place of the real tool's
            output. Should be a JSON string, the same shape the real tool would
            return.
        result:
          type: boolean
          nullable: true
          description: >-
            For tool calls like transfer_call that require a boolean result.
            Optional for most tools.
    LLMModel:
      type: string
      enum:
        - gpt-4.1
        - gpt-4.1-mini
        - gpt-4.1-nano
        - gpt-5
        - gpt-5-mini
        - gpt-5-nano
        - gpt-5.1
        - gpt-5.2
        - gpt-5.4
        - gpt-5.4-mini
        - gpt-5.4-nano
        - gpt-5.5
        - gpt-5.6-terra
        - gpt-5.6-luna
        - claude-4.5-sonnet
        - claude-4.6-sonnet
        - claude-5-sonnet
        - claude-4.5-haiku
        - gemini-3.0-flash
        - gemini-3.1-flash-lite
        - gemini-3.5-flash
      description: Available LLM models for agents.
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
    ToolMockInputMatchRule:
      description: >-
        Decides which calls to the tool this mock applies to, based on the
        arguments the LLM passes to the tool.
      oneOf:
        - type: object
          required:
            - type
          properties:
            type:
              type: string
              enum:
                - any
              description: >-
                Match every call to the tool, no matter what arguments were
                passed. Use this for a catch-all mock.
        - type: object
          required:
            - type
            - args
          properties:
            type:
              type: string
              enum:
                - partial_match
              description: >-
                Match only calls whose arguments contain the values listed in
                `args`.
            args:
              type: object
              description: >-
                Argument values the call must have to match. Only the fields you
                list here are checked, and each must equal the value in the
                actual call. Extra fields in the call are ignored, so this is a
                subset match.
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