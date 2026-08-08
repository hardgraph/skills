> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Retell SDKs for TypeScript and Python

> Official Retell SDKs for Node.js and Python — typed clients, simple API key auth, structured error handling, and full coverage of voice and chat endpoints.

## Overview

Retell provides official SDKs for Node.js and Python to simplify integration with our platform. While you can use our [REST API](/api-references/create-phone-call) directly, our SDKs offer:

* **Type safety**: Full TypeScript support with autocomplete
* **Simplified authentication**: Built-in API key handling
* **Error handling**: Structured error responses with detailed messages
* **Reduced boilerplate**: Cleaner, more maintainable code

## Available SDKs & Requirements

### Node.js TypeScript SDK

* **Package**: [retell-sdk on NPM](https://www.npmjs.com/package/retell-sdk)
* **Requirements**: Node.js version 18.10.0 or higher
* **Features**: Full TypeScript support, async/await, promise-based API

### Python SDK

* **Package**: [retell-sdk on PyPI](https://pypi.org/project/retell-sdk/)
* **Requirements**: Python 3.9 or higher
* **Features**: Type hints, async support, comprehensive error handling

<Steps>
  <Step title="Get Your API Key">
    Navigate to the "API Keys" tab in your dashboard to obtain your API key.

    <Frame>
      <img height="200" src="https://mintcdn.com/retellai/gY538VnArOndFhp0/images/api_key.png?fit=max&auto=format&n=gY538VnArOndFhp0&q=85&s=eafc7807d7c80f3268ed2f2a1b9d2ebf" alt="API Keys tab in Retell dashboard showing where to find and copy your API key" data-path="images/api_key.png" />
    </Frame>
  </Step>

  <Step title="Install the SDK">
    Choose your preferred language and install the SDK:

    <CodeGroup>
      ```bash Node Client theme={"dark"}
      npm i retell-sdk
      ```

      ```bash Python Client theme={"dark"}
      pip install retell-sdk
      ```
    </CodeGroup>
  </Step>

  <Step title="Initialize the Client">
    Create a new client instance using your API key:

    <CodeGroup>
      ```typescript Node Client theme={"dark"}
      import Retell from 'retell-sdk';

      const retellClient = new Retell({
        apiKey: "YOUR_API_KEY",
      });
      ```

      ```python Python Client theme={"dark"}
      from retell import Retell

      retell_client = Retell(
        api_key="YOUR_API_KEY"
      )
      ```
    </CodeGroup>
  </Step>

  <Step title="Make API Calls">
    Here's an example of making a phone call using the SDK:

    <CodeGroup>
      ```typescript Node Client theme={"dark"}
      try {
        const response = await retellClient.call.createPhoneCall({
          from_number: '+14157774444',
          to_number: '+12137774445',
        });
        console.log('Call initiated:', response);
      } catch (error) {
        console.error('Error making call:', error);
      }
      ```

      ```python Python Client theme={"dark"}
      try:
        response = retell_client.call.create_phone_call(
          from_number="+14157774444",
          to_number="+12137774445"
        )
        print(f"Call initiated: {response}")
      except Exception as e:
        print(f"Error making call: {e}")
      ```
    </CodeGroup>
  </Step>
</Steps>

## Versioning

The current release is **5.60.0** (1 August 2026), published as `retell-sdk` on both npm and PyPI.

The version number tracks releases, not compatibility. One release can add endpoints and drop retired ones at the same time, and holding an older version doesn't keep the old behavior: the API changes on the server, so an old client stops matching what the API accepts. Run the latest release, and follow the [deprecation notices](/deprecation-notice/overview) — by RSS if you want them as they're announced — because that's where a change that affects your code is announced ahead of time.

Each release notes what changed: [TypeScript releases](https://github.com/RetellAI/retell-typescript-sdk/releases) and [Python releases](https://github.com/RetellAI/retell-python-sdk/releases).

## Create an agent with the TypeScript SDK

Creating a voice agent requires a response engine and an agent configuration. Follow the [end-to-end TypeScript guide](/get-started/create-agent-with-sdk) to create both resources, test the draft, and publish a version.

## Best Practices

### 1. Error Handling

Always wrap SDK calls in try-catch blocks to handle potential errors gracefully:

```typescript theme={"dark"}
try {
  const response = await retellClient.call.createPhoneCall(params);
  // Handle success
} catch (error) {
  if (error.code === 'insufficient_funds') {
    // Handle specific error
  }
  // Log error details
}
```

### 2. Environment Variables

Store your API key securely using environment variables:

```typescript theme={"dark"}
const retellClient = new Retell({
  apiKey: process.env.RETELL_API_KEY,
});
```

### 3. Type Safety

Leverage TypeScript types for better developer experience:

```typescript theme={"dark"}
import { Retell, AgentCreateParams } from 'retell-sdk';

const params: AgentCreateParams = {
  // TypeScript will provide autocomplete here
};
```

## Rate Limits

Limits apply per **organization + route**, enforced at the HTTP layer.

| Endpoint group                                           | Limit                                        |
| -------------------------------------------------------- | -------------------------------------------- |
| Call creation                                            | **1000 / 10s**, plus **60 4xx errors / min** |
| List endpoints (`list-*`, `batch-get-*`)                 | **15 / 10s**                                 |
| All other SDK endpoints (get / create / update / delete) | **100 / 10s**                                |
| LLM / agent playground completions                       | **8 / 2s**                                   |

Outbound calls are also subject to [CPS limits](/deploy/outbound-call#step-3-configure-cps-calls-per-second) (excess calls are queued, not rejected) and the per-org [concurrent call limit](/deploy/concurrency).

### 429 Response

```http theme={"dark"}
HTTP/1.1 429 Too Many Requests
X-RateLimit-Limiter: general
RateLimit-Limit: 100
RateLimit-Remaining: 0
RateLimit-Reset: 7

{ "status": "error", "message": "Too many API requests, you are being throttled, please try again later." }
```

`X-RateLimit-Limiter` identifies which limiter fired: `general`, `list`, `call`, `call-error`, or `llm-playground`.

### Handling 429s

* Use `RateLimit-Reset` (seconds) for backoff; retry with jittered exponential backoff.
* Don't parallelize `list-*` calls — the per-route budget is small.
* For high call volume, design around CPS rather than HTTP throughput.
