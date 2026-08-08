> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Test Case Definition

> Create a new test case definition



## OpenAPI

````yaml openapi-final post /create-test-case-definition
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
  /create-test-case-definition:
    post:
      description: Create a new test case definition
      operationId: createTestCaseDefinition
      requestBody:
        required: true
        content:
          application/json:
            schema:
              allOf:
                - $ref: '#/components/schemas/TestCaseDefinitionInput'
                - type: object
                  required:
                    - name
                    - response_engine
                    - user_prompt
                    - metrics
      responses:
        '201':
          description: Test case definition created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TestCaseDefinition'
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
          source: >-
            import Retell from 'retell-sdk';


            const client = new Retell({
              apiKey: process.env['RETELL_API_KEY'], // This is the default and can be omitted
            });


            const testCaseDefinitionResponse = await
            client.tests.createTestCaseDefinition({
              metrics: ['string'],
              name: 'name',
              response_engine: { llm_id: 'llm_id', type: 'retell-llm' },
              user_prompt: 'user_prompt',
            });


            console.log(testCaseDefinitionResponse.test_case_definition_id);
        - lang: Python
          source: >-
            import os

            from retell import Retell


            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )

            test_case_definition_response =
            client.tests.create_test_case_definition(
                metrics=["string"],
                name="name",
                response_engine={
                    "llm_id": "llm_id",
                    "type": "retell-llm",
                },
                user_prompt="user_prompt",
            )

            print(test_case_definition_response.test_case_definition_id)
components:
  schemas:
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