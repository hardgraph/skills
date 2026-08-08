> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# API Key Overview

> How API keys authenticate your Retell REST API requests, SDK integrations, and webhook endpoints, plus best practices for storing and rotating keys safely.

API keys are used to authenticate your requests to:

* REST API endpoints
* SDK integrations
* CLI commands
* Webhook endpoints

Each workspace can have multiple API keys, all sharing the same permission level.

### REST API Authentication

To authenticate your REST API requests, include your API key in the request headers:

```http theme={"dark"}
Authorization: Bearer YOUR_API_KEY
```

Try out authentication in our [API Playground](/api-references/create-phone-call) to see it in action.

### Webhook API Key

For enhanced security, we automatically designate one of your API keys for [webhook authentication](/features/secure-webhook). This designated webhook API key:

* Is used to sign and verify webhook requests
* Cannot be deleted
* Ensures your webhook endpoints only receive legitimate requests

<Note>
  Keep your API keys secure and never share them in public repositories or client-side code.
</Note>
