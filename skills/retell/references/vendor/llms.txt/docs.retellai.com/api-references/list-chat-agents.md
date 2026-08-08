> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# List Chat Agents

> List unique agents with pagination.

Use `filter_criteria.channel` with `value: "chat"` to list only chat agents.

The response is paginated. Read agents from `items`, then pass the returned `pagination_key` on the next request while `has_more` is `true`.

<RequestExample>
  ```javascript Javascript theme={"dark"}
  import Retell from 'retell-sdk';

  const client = new Retell({
    apiKey: 'YOUR_RETELL_API_KEY',
  });

  const agents = await client.agent.list({
    filter_criteria: {
      channel: {
        op: 'eq',
        value: 'chat',
      },
    },
  });

  console.log(agents.items);
  ```

  ```Python Python theme={"dark"}
  from retell import Retell

  client = Retell(api_key="YOUR_RETELL_API_KEY")

  agents = client.agent.list(
      filter_criteria={
          "channel": {
              "op": "eq",
              "value": "chat",
          },
      },
  )

  print(agents.items)
  ```

  ```bash cURL theme={"dark"}
  curl --request POST \
    --url 'https://api.retellai.com/v2/list-agents?limit=50' \
    --header 'Authorization: Bearer YOUR_RETELL_API_KEY' \
    --header 'Content-Type: application/json' \
    --data '{
      "filter_criteria": {
        "channel": {
          "op": "eq",
          "value": "chat"
        }
      }
    }'
  ```
</RequestExample>


## OpenAPI

````yaml openapi-final post /v2/list-agents
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
  /v2/list-agents:
    post:
      description: List unique agents with pagination.
      operationId: listAgents
      parameters:
        - $ref: '#/components/parameters/LimitParam'
        - $ref: '#/components/parameters/SortOrderParam'
        - $ref: '#/components/parameters/PaginationKeyParam'
      requestBody:
        required: false
        content:
          application/json:
            schema:
              type: object
              properties:
                filter_criteria:
                  $ref: '#/components/schemas/AgentListFilter'
            example:
              filter_criteria:
                channel:
                  type: string
                  op: eq
                  value: voice
      responses:
        '200':
          description: Successfully retrieved agents.
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
                          $ref: '#/components/schemas/AgentListItemResponse'
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
          source: |-
            import Retell from 'retell-sdk';

            const client = new Retell({
              apiKey: process.env['RETELL_API_KEY'], // This is the default and can be omitted
            });

            const agents = await client.agent.list({
              filter_criteria: {
                channel: {
                  type: 'string',
                  op: 'eq',
                  value: 'voice',
                },
              },
            });

            console.log(agents.has_more);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            agents = client.agent.list(
                filter_criteria={
                    "channel": {
                        "type": "string",
                        "op": "eq",
                        "value": "voice",
                    }
                },
            )
            print(agents.has_more)
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
    SortOrderParam:
      in: query
      name: sort_order
      schema:
        type: string
        enum:
          - ascending
          - descending
        default: descending
      description: Sort order for results.
    PaginationKeyParam:
      in: query
      name: pagination_key
      schema:
        type: string
      description: Pagination key for fetching the next page.
  schemas:
    AgentListFilter:
      type: object
      description: Filters for listing agents. All provided filters are connected with AND.
      properties:
        channel:
          allOf:
            - $ref: '#/components/schemas/StringFilter'
            - description: 'Filter by agent channel. Use `op: eq`.'
              properties:
                op:
                  enum:
                    - eq
                value:
                  enum:
                    - voice
                    - chat
        query:
          type: string
          description: >-
            Case-insensitive substring search over agent name, plus substring
            search over agent id.
    PaginatedResponseBase:
      type: object
      properties:
        pagination_key:
          type: string
          description: Pagination key for the next page.
        has_more:
          type: boolean
          description: Whether more results are available.
    AgentListItemResponse:
      type: object
      required:
        - agent_id
        - agent_name
        - channel
        - user_modified_timestamp
        - tags
      properties:
        agent_id:
          type: string
          example: agent_1ffdb9717444d0e77346838911
          description: Unique id of agent.
        agent_name:
          type: string
          example: Jarvis
          description: The name of the agent. Only used for your own reference.
        channel:
          type: string
          enum:
            - voice
            - chat
          example: voice
        user_modified_timestamp:
          type: integer
          format: int64
          example: 1703413636133
          description: >-
            User modification timestamp (milliseconds since epoch). Either the
            time of last update or creation if no updates available.
        tags:
          type: object
          description: Authoritative root tags for this agent, keyed by tag name.
          additionalProperties:
            $ref: '#/components/schemas/AgentRootTagState'
    StringFilter:
      type: object
      required:
        - type
        - op
        - value
      properties:
        type:
          type: string
          enum:
            - string
        op:
          type: string
          enum:
            - eq
            - ne
            - sw
            - ew
            - co
          description: >-
            eq: equal, ne: not equal, sw: starts with, ew: ends with, co:
            contains
        value:
          type: string
    AgentRootTagState:
      type: object
      properties:
        version:
          type: integer
          minimum: 0
          example: 15
        dynamic_variables:
          type: object
          additionalProperties:
            type: string
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