---
title: Actor definition file (actor.json)
url: https://docs.apify.com/actors/development/actor-definition/actor-json.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Development](https://docs.apify.com/actors/development.md)
  - [Actor definition](https://docs.apify.com/actors/development/actor-definition.md)
previous: [Actor definition](https://docs.apify.com/actors/development/actor-definition.md)
next: [Source code](https://docs.apify.com/actors/development/actor-definition/source-code.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Actor definition file (actor.json)

Your main Actor configuration is in the `.actor/actor.json` file at the root of your Actor's directory. This file links your local development project to an Actor on the Apify platform. It should include details like the Actor's name, version, build tag, and environment variables. Make sure to commit this file to your Git repository.

For example, the `.actor/actor.json` file can look like this:

<!-- -->

**Full actor.json**


```json
{

    "actorSpecification": 1, // always 1

    "name": "name-of-my-scraper",

    "title": "My Web Scraper",

    "version": "0.0",

    "buildTag": "latest",

    "meta": {

        "templateId": "ts-crawlee-playwright-chrome"

    },

    "defaultMemoryMbytes": "get(input, 'startUrls.length', 1) * 1024",

    "minMemoryMbytes": 256,

    "maxMemoryMbytes": 4096,

    "environmentVariables": {

        "MYSQL_USER": "my_username",

        "MYSQL_PASSWORD": "@mySecretPassword"

    },

    "usesStandbyMode": false,

    "dockerfile": "./Dockerfile",

    "readme": "./ACTOR.md",

    "input": "./input_schema.json",

    "output": "./output_schema.json",

    "storages": {

        "dataset": "./dataset_schema.json"

    },

    "webServerSchema": "./web_server_openapi.json",

    "webServerMcpPath": "/mcp"

}
```


**Minimal actor.json**


```json
{

    "actorSpecification": 1, // always 1

    "name": "name-of-my-scraper",

    "version": "0.0"

}
```


## Reference

Deployment metadata

Actor `name`, `version`, `buildTag`, and `environmentVariables` are currently only used when you deploy your Actor using the [Apify CLI](https://docs.apify.com/cli) and not when deployed, for example, via GitHub integration. There, it serves for informative purposes only.

| Property               | Type     | Description                                                                                                                                                                                                                                                                                                                                                                                                         |
| ---------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `actorSpecification`   | Required | The version of the Actor specification. This property must be set to `1`, which is the only version available.                                                                                                                                                                                                                                                                                                      |
| `name`                 | Required | The name of the Actor.                                                                                                                                                                                                                                                                                                                                                                                              |
| `title`                | Optional | The display title of the Actor. This is the human-readable title shown in Apify Console and Apify Store. If not specified, the `name` property is used as the title.                                                                                                                                                                                                                                                |
| `version`              | Required | The version of the Actor, specified in the format `[Number].[Number]`, e.g., `0.1`, `0.3`, `1.0`, `1.3`, etc.                                                                                                                                                                                                                                                                                                       |
| `buildTag`             | Optional | The tag name to be applied to a successful build of the Actor. If not specified, defaults to `latest`. Refer to the [builds](https://docs.apify.com/actors/development/builds-and-runs/builds.md) for more information.                                                                                                                                                                                             |
| `meta`                 | Optional | Metadata object containing additional information about the Actor. Currently supports `templateId` field to identify the template from which the Actor was created.                                                                                                                                                                                                                                                 |
| `environmentVariables` | Optional | A map of environment variables to be used during local development. These variables will also be applied to the Actor when deployed on the Apify platform. For more details, see the [environment variables](https://docs.apify.com/cli/docs/vars) section of the Apify CLI documentation.                                                                                                                          |
| `dockerfile`           | Optional | The path to the Dockerfile to be used for building the Actor on the platform. If not specified, the system will search for Dockerfiles in the `.actor/Dockerfile` and `Dockerfile` paths, in that order. Refer to the [Dockerfile](https://docs.apify.com/actors/development/actor-definition/dockerfile.md) section for more information.                                                                          |
| `dockerContextDir`     | Optional | The path to the directory to be used as the Docker context when building the Actor. The path is relative to the location of the `actor.json` file. This property is useful for monorepos containing multiple Actors. Refer to the [Actor monorepos](https://docs.apify.com/actors/development/deployment/source-types.md#actor-monorepos) section for more details.                                                 |
| `readme`               | Optional | The path to the README file to be used on the platform. If not specified, the system will look for README files in the `.actor/README.md` and `README.md` paths, in that order of preference. For details, see [Create an Actor README](https://docs.apify.com/actors/publishing/actor-readme.md).                                                                                                                  |
| `input`                | Optional | You can embed your [input schema](https://docs.apify.com/actors/development/actor-definition/input-schema.md) object directly in `actor.json` under the `input` field. You can also provide a path to a custom input schema. If not provided, the input schema at `.actor/INPUT_SCHEMA.json` or `INPUT_SCHEMA.json` is used, in this order of preference. You can also use the `inputSchema` alias interchangeably. |
| `output`               | Optional | You can embed your [output schema](https://docs.apify.com/actors/development/actor-definition/output-schema.md) object directly in `actor.json` under the `output` field. You can also provide a path to a custom output schema. [Read more](https://docs.apify.com/actors/development/actor-definition/output-schema.md) about Actor output schemas. You can also use the `outputSchema` alias interchangeably.    |
| `changelog`            | Optional | The path to the CHANGELOG file displayed in the Information tab of the Actor in Apify Console next to Readme. If not provided, the CHANGELOG at `.actor/CHANGELOG.md` or `CHANGELOG.md` is used, in this order of preference. Your Actor doesn't need to have a CHANGELOG but it is a good practice to keep it updated for published Actors.                                                                        |
| `storages.dataset`     | Optional | You can define the schema of the items in your dataset under the `storages.dataset` field. This can be either an embedded object or a path to a JSON schema file. [Read more](https://docs.apify.com/storage/dataset-schema.md) about Actor dataset schemas.                                                                                                                                                        |
| `storages.datasets`    | Optional | You can define multiple datasets for the Actor under the `storages.datasets` field. This can be an object containing embedded objects or paths to a JSON schema files. [Read more](https://docs.apify.com/storage/dataset-schema/multiple-datasets.md) about multiple dataset schemas.                                                                                                                              |
| `defaultMemoryMbytes`  | Optional | Specifies the default amount of memory in megabytes to be used when the Actor is started. Can be an integer or a [dynamic memory expression string](https://docs.apify.com/actors/development/actor-definition/dynamic-actor-memory.md).                                                                                                                                                                            |
| `minMemoryMbytes`      | Optional | Specifies the minimum amount of memory in megabytes required by the Actor to run. Requires an *integer* value. If both `minMemoryMbytes` and `maxMemoryMbytes` are set, then `minMemoryMbytes` must be equal or lower than `maxMemoryMbytes`. Refer to the [Usage and resources](https://docs.apify.com/actors/running/usage-and-resources#memory) for more details about memory allocation.                        |
| `maxMemoryMbytes`      | Optional | Specifies the maximum amount of memory in megabytes required by the Actor to run. It can be used to control the costs of run. Requires an *integer* value. Refer to the [Usage and resources](https://docs.apify.com/actors/running/usage-and-resources#memory) for more details about memory allocation.                                                                                                           |
| `usesStandbyMode`      | Optional | Boolean specifying whether the Actor will have [Standby mode](https://docs.apify.com/actors/development/programming-interface/standby.md) enabled.                                                                                                                                                                                                                                                                  |
| `webServerSchema`      | Optional | Defines an OpenAPI v3 schema for the web server running in the Actor. This can be either an embedded object or a path to a JSON schema file. Use this when your Actor starts its own HTTP server and you want to describe its interface.                                                                                                                                                                            |
| `webServerMcpPath`     | Optional | The HTTP endpoint path where the Actor exposes its MCP (Model Context Protocol) server functionality. When set, the Actor is recognized as an MCP server. For example, setting `"/mcp"` designates the `/mcp` endpoint as the MCP interface. This path becomes part of the Actor's stable URL when [Standby mode](https://docs.apify.com/actors/development/programming-interface/standby.md) is enabled.           |
