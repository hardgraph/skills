> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Get MCP Tools

> Get MCP tools for a specific agent



## OpenAPI

````yaml openapi-final get /get-mcp-tools/{agent_id}
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
  /get-mcp-tools/{agent_id}:
    get:
      description: Get MCP tools for a specific agent
      operationId: getMCPTools
      parameters:
        - in: path
          name: agent_id
          schema:
            type: string
            minLength: 1
            example: oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD
          required: true
          description: Unique id of the agent to get MCP tools for.
        - in: query
          name: version
          schema:
            $ref: '#/components/schemas/AgentVersionReference'
          required: false
          description: >-
            Optional version of the agent to use for this request. Default to
            latest version.
        - in: query
          name: mcp_id
          schema:
            type: string
            example: mcp-server-1
          required: true
          description: The ID of the MCP server to get tools from.
        - in: query
          name: component_id
          schema:
            type: string
            example: component-123
          required: false
          description: >-
            The ID of the component if the MCP server is configured under a
            component.
      responses:
        '200':
          description: Successfully retrieved MCP tools.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/MCPToolDefinition'
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
          source: >-
            import Retell from 'retell-sdk';


            const client = new Retell({
              apiKey: process.env['RETELL_API_KEY'], // This is the default and can be omitted
            });


            const mcpToolDefinitions = await
            client.mcpTool.getMcpTools('oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD', {
              mcp_id: 'mcp-server-1',
            });


            console.log(mcpToolDefinitions);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            mcp_tool_definitions = client.mcp_tool.get_mcp_tools(
                agent_id="oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD",
                mcp_id="mcp-server-1",
            )
            print(mcp_tool_definitions)
components:
  schemas:
    AgentVersionReference:
      oneOf:
        - type: string
          minLength: 1
          maxLength: 20
          pattern: >-
            ^(latest|latest_published|(?!(?:latest|latest_published|v\d+)$)[a-z][a-z0-9_-]{0,19})$
          example: latest_published
        - type: integer
          minimum: 0
          example: 1
      description: >-
        Agent version reference. Supports a numeric version (for example 3) or a
        tag/environment name (for example "prod"). The string "latest" resolves
        to the most recently created version (the largest version number), and
        "latest_published" resolves to the most recently published version. When
        a tag is provided, resolution uses that exact tag assignment (including
        its dynamic variables). If the tag exists but is currently unassigned,
        it resolves to latest. When a numeric version, latest, or
        latest_published is provided, resolution applies dynamic variables from
        the preferred tag for that resolved version (most recently assigned), if
        any.
    MCPToolDefinition:
      type: object
      properties:
        name:
          type: string
          description: Name of the MCP tool.
          example: search_files
        description:
          type: string
          description: Description of what the MCP tool does.
          example: Search for files in the filesystem
        inputSchema:
          type: object
          description: JSON schema defining the input parameters for the tool.
          example:
            type: object
            properties:
              query:
                type: string
                description: Search query
            required:
              - query
      required:
        - name
        - description
        - inputSchema
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