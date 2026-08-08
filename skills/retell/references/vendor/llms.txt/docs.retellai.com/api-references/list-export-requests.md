> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# List Export Requests

> List export requests with pagination



## OpenAPI

````yaml openapi-final get /v2/list-export-requests
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
  /v2/list-export-requests:
    get:
      description: List export requests with pagination
      operationId: listExportRequests
      parameters:
        - $ref: '#/components/parameters/LimitParam'
        - $ref: '#/components/parameters/SortOrderParam'
        - $ref: '#/components/parameters/PaginationKeyParam'
      responses:
        '200':
          description: Export requests retrieved successfully
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
                          type: object
                          properties:
                            export_request_id:
                              type: string
                            channel:
                              type: string
                              enum:
                                - call
                                - chat
                            status:
                              type: string
                              enum:
                                - created
                                - processing
                                - completed
                                - error
                            url:
                              type: string
                            created_timestamp:
                              type: integer
                            timezone:
                              type: string
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

            const exportRequests = await client.exportRequest.list();

            console.log(exportRequests.has_more);
        - lang: Python
          source: |-
            import os
            from retell import Retell

            client = Retell(
                api_key=os.environ.get("RETELL_API_KEY"),  # This is the default and can be omitted
            )
            export_requests = client.export_request.list()
            print(export_requests.has_more)
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
    PaginatedResponseBase:
      type: object
      properties:
        pagination_key:
          type: string
          description: Pagination key for the next page.
        has_more:
          type: boolean
          description: Whether more results are available.
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