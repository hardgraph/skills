> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Retell AI API Reference

> Reference for the Retell AI API: authentication, base URL, SDKs, and endpoints for building and managing voice and chat agents programmatically.

Use this section to build and manage Retell AI voice and chat agents programmatically.

## What you can do

* Create, update, and manage calls and chats
* Configure voice agents and chat agents
* Manage phone numbers, voices, and knowledge bases
* Run tests and retrieve test run results
* Integrate custom telephony and custom LLM workflows

## Base URL

All REST API requests use the same base URL:

```
https://api.retellai.com
```

Append the endpoint path from the reference (for example, `POST https://api.retellai.com/v2/list-calls`). The audio WebSocket uses a separate host — see [Audio WebSocket](/api-references/audio-websocket).

## Authentication

Every request must include your API key as a bearer token in the `Authorization` header:

```
Authorization: Bearer YOUR_API_KEY
```

Create and manage keys from the **API Keys** tab in the [dashboard](https://dashboard.retellai.com). See [Manage API Keys](/accounts/manage-api-keys) for details.

## SDKs

Official SDKs handle the base URL and authentication for you:

* [Node.js / TypeScript SDK](https://www.npmjs.com/package/retell-sdk)
* [Python SDK](https://pypi.org/project/retell-sdk/)

See [SDKs](/get-started/sdk) for installation, initialization, and usage examples, and [SDK versioning](/get-started/sdk#versioning) for how the TypeScript and Python versions line up.

## Getting started

If you are new to the API:

1. Create your API key in the dashboard.
2. Make your first request with the `Create phone call` endpoint.
3. Use the grouped endpoint sections in the sidebar to expand your integration.
