---
title: Web server schema
url: https://docs.apify.com/actors/development/actor-definition/web-server-schema.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Development](https://docs.apify.com/actors/development.md)
  - [Actor definition](https://docs.apify.com/actors/development/actor-definition.md)
previous: [Actor output schema](https://docs.apify.com/actors/development/actor-definition/output-schema.md)
next: [Dockerfile](https://docs.apify.com/actors/development/actor-definition/dockerfile.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Web server schema

The `webServerSchema` field in `.actor/actor.json` attaches an [OpenAPI 3.x](https://spec.openapis.org/oas/v3.0.3) specification to your Actor. You can define the schema for any Actor that exposes an HTTP server. When you enable [standby mode](https://docs.apify.com/actors/development/programming-interface/standby.md), Apify Console and Apify Store render an interactive **Standby** tab on the Actor's detail page. From there you can browse endpoints, inspect request and response schemas, and send requests directly from the browser.

![Apify Console showing the Standby tab with the Endpoints section rendered from the Actor\&#39;s OpenAPI spec](/assets/images/console-standby-openapi-swagger-2d5c469a9e35d9cf5735d319c07d10c4.png)

## Define the web server schema

You can define the OpenAPI spec inline in `.actor/actor.json` or reference a separate file.

### Reference an external file

.actor/actor.json


```json
{

    "actorSpecification": 1,

    "name": "my-api-actor",

    "version": "1.0",

    "usesStandbyMode": true,

    "webServerSchema": "./openapi.json"

}
```


Place your OpenAPI spec in `.actor/openapi.json`:

.actor/openapi.json


```json
{

    "openapi": "3.0.0",

    "info": {

        "title": "My API Actor",

        "version": "1.0.0"

    },

    "paths": {

        "/search": {

            "get": {

                "summary": "Search for items",

                "parameters": [

                    {

                        "name": "query",

                        "in": "query",

                        "required": true,

                        "schema": { "type": "string" }

                    }

                ],

                "responses": {

                    "200": {

                        "description": "Search results"

                    }

                }

            }

        }

    }

}
```


### Embed inline

.actor/actor.json


```json
{

    "actorSpecification": 1,

    "name": "my-api-actor",

    "version": "1.0",

    "usesStandbyMode": true,

    "webServerSchema": {

        "openapi": "3.0.0",

        "info": {

            "title": "My API Actor",

            "version": "1.0.0"

        },

        "paths": {

            "/health": {

                "get": {

                    "summary": "Health check",

                    "responses": {

                        "200": { "description": "OK" }

                    }

                }

            }

        }

    }

}
```


Follow the standard [OpenAPI 3.x format](https://spec.openapis.org/oas/latest.html) to describe your endpoints, parameters, request bodies, and responses.

## Build and deploy

The build process validates `webServerSchema`, similar to other Actor schemas like [input schema](https://docs.apify.com/actors/development/actor-definition/input-schema.md) and [dataset schema](https://docs.apify.com/storage/dataset-schema.md). If the spec is malformed, the build fails with a validation error.

Once deployed, the **Standby** tab appears automatically on the Actor's detail page when you enable [standby mode](https://docs.apify.com/actors/development/programming-interface/standby.md). It renders your spec with [Swagger UI](https://swagger.io/tools/swagger-ui/) and handles authentication automatically - Actor users can send requests without configuring API tokens.

Servers field is overwritten

Your `servers` array is replaced with the Actor's standby URL at display time. Custom server URLs are ignored.

## Related fields

| Field             | Description                                                                                                                                                                                              |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `usesStandbyMode` | Must be `true` for the **Standby** tab to appear. See [standby mode](https://docs.apify.com/actors/development/programming-interface/standby.md).                                                        |
| `webServerSchema` | The OpenAPI spec that powers the **Standby** tab. Defined in [.actor/actor.json](https://docs.apify.com/actors/development/actor-definition/actor-json.md) as an inline object or a path to a JSON file. |
