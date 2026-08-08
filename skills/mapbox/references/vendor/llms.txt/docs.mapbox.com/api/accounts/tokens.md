# Tokens

An [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) provides access to Mapbox resources on behalf of a user. The Mapbox Tokens API provides you with a programmatic way to create, update, delete, and retrieve tokens, as well as list a user's tokens and token scopes.

All user accounts have a default public token. Additional tokens can be created to grant additional, or more limited, privileges.

The actions allowed by a token are based on **[scopes](https://docs.mapbox.com/help/getting-started/access-tokens/#scopes)**. A scope is a string that often is a resource type and action separated by a colon. For example, the `styles:read` scope allows read access to styles. Tokens will have access to different scopes depending on their account level and other features of their account.

To learn more about token basics, like creating a token, see our [Getting Started with Access Tokens guide](https://docs.mapbox.com/help/getting-started/access-tokens/).

## Token format

Mapbox uses [JSON Web Tokens](https://jwt.io/) (JWT) as the token format. Each token is a string delimited by dots into three parts: header, payload, and signature.

-   **Header.** A literal value of either `pk` (public token), `sk` (secret token), or `tk` (temporary token).
-   **Payload.** A [base64-URL](https://tools.ietf.org/html/rfc4648#section-5:~:text=character.%0A%0A5.-,Base%2064%20Encoding%20with%20URL,-and%20Filename%20Safe)-encoded JSON object containing the identity and authorities of the token. `pk` and `sk` tokens contain a reference to metadata that holds the rights granted for the token. `tk` tokens contain the contents of the metadata directly in the payload.
-   **Signature.** Signed by Mapbox and used to verify the token has not been tampered with.

## Token metadata object

Every token has a metadata object that contains information about the capabilities of the token. The token metadata object contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `id` | `string` | The token's unique identifier. This is *not* the access token itself, but rather an identifier for a specific `pk`, `sk`, or `tk` token. You can find a token's ID within the account dashboard by navigating to the token's page, clicking on the token's name, and copying the value at the end of the URL, like: `https://console.mapbox.com/account/access-tokens/<token_id>` |
| `usage` | `string` | The type of token. One of: `pk` (public), `sk` (secret), or `tk` (temporary). |
| `client` | `string` | The client for the token. This is always `api`. |
| `default` | `boolean` | Indicates whether the token is a default token. |
| `scopes` | `array` | An array that contains the scopes granted to the token. |
| `note` | `string` | A human-readable description of the token. |
| `created` | `string` | The date and time the token was created. |
| `modified` | `string` | The date and time the token was last modified. |
| `allowedUrls` | `array` | The URLs that the token is restricted to. |
| `token` | `string` | The token itself. |

### Example token metadata object

```json
{
  "id": "cijucimbe000brbkt48d0dhcx",
  "usage": "pk",
  "client": "api",
  "default": false,
  "note": "My website",
  "scopes": ["styles:read", "fonts:read"],
  "created": "2018-01-25T19:07:07.621Z",
  "modified": "2018-01-26T00:39:57.941Z",
  "allowedUrls": ["https://docs.mapbox.com"],
  "token": "pk.EXAMPLE_REDACTED.sbihZCZJ56-fsFNKHXF8YQ"
}
```

> **Note: Support for allowed URLs**
> 
> The **allowed URLs** feature is compatible with many Mapbox tools, with some limitations. For web applications using Mapbox GL JS, it requires version [0.53.1+](https://docs.mapbox.com/mapbox-gl-js/api/). It is not compatible with Mapbox native SDKs. Adding a URL restriction to a token makes it unusable by a mobile application. A separate token should be maintained for mobile applications.See the [Adding URL restrictions to access tokens](https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions) guide to learn more about this feature for web requests.

## List tokens

**GET** : `https://api.mapbox.com/tokens/v2/{username}` [Token scope: **tokens:read**]

List all the tokens that belong to an account. This endpoint supports [pagination](https://docs.mapbox.com/api/guides/#pagination).

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account for which to list tokens. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tokens:read` scope. |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameter | Type | Description |
| --- | --- | --- |
| `default` | `boolean` | If this parameter is `true`, the response will only include the account's default token. If this parameter is `false`, the response will include all the account's tokens *except* for the default token. |
| `limit` | `integer` | The maximum number of tokens to return. |
| `sortby` | `string` | Sort the tokens in the response by their `created` or `modified` timestamps. |
| `start` | `string` | The token after which to start the listing. Find the token key in the `Link` header of a response. See the [pagination](https://docs.mapbox.com/api/guides/#pagination) section for details. |
| `usage` | `string` | Use this parameter to return either only public tokens (`pk`) or secret tokens (`sk`). By default, this endpoint returns both types of token. |

### Example request: List tokens

```bash
$ curl "https://api.mapbox.com/tokens/v2/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::tokens:read"
```

### Response: List tokens

The response body will contain all the tokens that belong to the username specified in the query, each containing the properties described in the [token metadata object section](#token-metadata-object).

If a listed token's `usage` property is `sk`, the `token` property will not be included in the response.

### Example response: List tokens

```json
[
  {
    "client": "api",
    "note": "a public token",
    "usage": "pk",
    "id": "cijucimbe000brbkt48d0dhcx",
    "default": false,
    "scopes": ["styles:read", "fonts:read"],
    "allowedUrls": ["https://docs.mapbox.com", "https://account.mapbox.com"],
    "created": "2016-01-25T19:07:07.621Z",
    "modified": "2016-01-26T00:39:57.941Z",
    "token": "pk.EXAMPLE_REDACTED.sbihZCZJ56-fsFNKHXF8YQ"
  },
  {
    "client": "api",
    "note": "a secret token",
    "usage": "sk",
    "id": "juorumy001cutm5r4fl2y1b",
    "default": false,
    "scopes": ["styles:list"],
    "created": "2016-01-26T00:50:13.701Z",
    "modified": "2016-01-26T00:50:13.701Z"
  }
]
```

### Supported libraries: List tokens

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#listtokens)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Create a token

**POST** : `https://api.mapbox.com/tokens/v2/{username}` [Token scope: **tokens:write**]

Creates a new token. Every requested scope must be present in the access token used to allow the request. It is not possible to create a token with access to more scopes than the token that created it.

To create additional tokens using the Mapbox Tokens API, you need to have an authorizing token that has the `tokens:write` scope, as well as all the scopes you want to add to the newly created token. To create the authorization token, visit the [Access Token](https://console.mapbox.com/accounts/access-tokens) page in your account, and click **Create a token**.

Note that while it is possible to create a token with no scopes, you will not be able to update this token later to include any scopes.

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account for which to list scopes. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tokens:write` scope. |

The request body must be a JSON object that contains the following properties:

| Parameter | Type | Description |
| --- | --- | --- |
| `note` | `string` | Create a description for the token. |
| `scopes` | `array` | Specify the scopes that the new token will have. The authorizing token needs to have the same scopes as, or more scopes than, the new token you are creating. |
| `allowedUrls` | `array` | URLs that this token is allowed to work with. |

### Available token scopes

The scopes included in the token determine whether the token is public or secret. A public token can only contain public scopes, while a secret token can contain both public and secret scopes. For a full list of all public and secret scopes, see the [Token management documentation](https://docs.mapbox.com/accounts/guides/tokens/#scopes).

### Example request: Create a token

```bash
# Create a public token with "styles:read" and "fonts:read" scopes and a "https://docs.mapbox.com" allowed URL

$ curl -H "Content-Type: application/json" -X POST -d '{"note": "My top secret project","scopes": ["styles:read", "fonts:read"], "allowedUrls": ["https://docs.mapbox.com"]}' 'https://api.mapbox.com/tokens/v2/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::tokens:write'
```

### Response: Create a token

The response body for a successful request will be a new public or secret token.

### Example response: Create a token

```json
{
  "client": "api",
  "note": "My top secret project",
  "usage": "pk",
  "id": "cijucimbe000brbkt48d0dhcx",
  "default": false,
  "scopes": ["styles:read", "fonts:read"],
  "created": "2016-01-25T19:07:07.621Z",
  "modified": "2016-01-25T19:07:07.621Z",
  "allowedUrls": ["https://docs.mapbox.com"],
  "token": "pk.EXAMPLE_REDACTED.sbihZCZJ56-fsFNKHXF8YQ"
}
```

### Supported libraries: Create a token

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#createtoken)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Create a temporary token

**POST** : `https://api.mapbox.com/tokens/v2/{username}` [Token scope: **tokens:write**]

Creates a new temporary token that automatically expires at a set time. You can create a temporary token using a secret token that has the `tokens:write` [scope](https://docs.mapbox.com/accounts/guides/tokens/#scopes). You can also create a temporary token using another temporary token as long as the authorizing token has `tokens:write` scope. Temporary tokens can't be updated or revoked after they are created.

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account for which to create a temporary token. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tokens:write` scope. |

The **request body** must be a JSON object that contains the following properties:

| Request body properties | Type | Description |
| --- | --- | --- |
| `expires` | `string` | Specify when the temporary token will expire. Cannot be a time in the past or more than one hour in the future. If the authorizing token is temporary, the `expires` time for the new temporary token cannot be later than that of the authorizing temporary token. |
| `scopes` | `array` | Specify the scopes that the new temporary token will have. The authorizing token needs to have the same scopes as, or more scopes than, the new temporary token you are creating. |

### Example request: Create a temporary token

```bash
# Request a temporary token with "styles:read" and "font:read" scopes

$ curl -H "Content-Type: application/json" -X POST -d '{"expires": "TEMP_TOKEN_EXPIRES","scopes": ["styles:read", "fonts:read"]}' 'https://api.mapbox.com/tokens/v2/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::tokens:write'
```

### Example request body: Create a temporary token

```json
{
  "expires": "2016-09-15T19:27:53.000Z",
  "scopes": ["styles:read", "fonts:read"]
}
```

### Response: Create a temporary token

The response body for a successful request will be a new temporary token. Unlike public and secret tokens, a temporary token contains its metadata inside the payload of the token instead of referencing a metadata object that persists on the server.

### Example response: Create a temporary token

```json
{
  "token": "REDACTED_EXAMPLE_TOKEN.ZepsWvpjTMlpePE4IQBs0g",
  "issued_at": "2016-09-15T18:27:53.000Z",
  "expires": "2016-09-15T19:27:53.000Z",
  "expires_in": 3600
}
```

### Supported libraries: Create a temporary token

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#createtemporarytoken)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Update a token

**PATCH** : `https://api.mapbox.com/tokens/v2/{username}/{token_id}` [Token scope: **tokens:write**]

Update the `note`, the `scopes`, the `allowedUrls`, or all three in a token's metadata. When updating scopes for an existing token, the token sent along with the request must also have the scopes you're requesting. It is not possible to create a token with access to more scopes than the token that updated it.

| Required parameter | Type | Description |
| --- | --- | --- |
| `token_id` | `string` | The ID of the token that you want to update. This is *not* the access token itself, but rather the unique identifier for a specific token. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tokens:write` scope. |

The **request body** must be a JSON object that contains one or both of the following properties:

| Request body properties | Type | Description |
| --- | --- | --- |
| `note` | `string` | Update the token's description. |
| `scopes` | `array` | Update the token's scopes. The authorizing token needs to have the same scopes as, or more scopes than, the token you are updating. A public token may only be updated to include other public scopes. A secret token may be updated to contain public and secret scopes. |
| `allowedUrls` | `array` | Update the restricted token's allowed URLs. |

### Example request: Update a token

```bash
# Update a token to have "fonts:read" scope and a "https://docs.mapbox.com" allowed URL

$ curl -H 'Content-Type: application/json' -X PATCH -d '{"scopes": ["styles:read", "fonts:read"], "allowedUrls": ["https://docs.mapbox.com"]}' 'https://api.mapbox.com/tokens/v2/YOUR_MAPBOX_USERNAME/{token_id}?access_token=TOKEN_PLACEHOLDER::tokens:write'
```

### Response: Update a token

The response body for a successful request will be a new temporary token.

### Example response: Update a token

```json
{
  "client": "api",
  "note": "My top secret project",
  "usage": "pk",
  "id": "cijucimbe000brbkt48d0dhcx",
  "default": false,
  "scopes": ["styles:tiles", "styles:read", "fonts:read"],
  "allowedUrls": ["https://docs.mapbox.com"],
  "created": "2016-01-25T19:07:07.621Z",
  "modified": "2016-01-25T19:07:07.621Z",
  "token": "pk.EXAMPLE_REDACTED.sbihZCZJ56-fsFNKHXF8YQ"
}
```

### Supported libraries: Update a token

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#updatetoken)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Delete a token

**DELETE** : `https://api.mapbox.com/tokens/v2/{username}/{token_id}` [Token scope: **tokens:write**]

Delete an access token. This will revoke its authorization and remove its access to Mapbox APIs. Applications using the revoked token will need to get a new access token before they can access Mapbox APIs.

Note that cached resources may continue to be accessible for a while after a token is deleted. No new or updated resources will be accessible with the deleted token.

| Required parameter | Type | Description |
| --- | --- | --- |
| `token_id` | `string` | The ID of the token that you want to delete. This is *not* the access token itself, but rather the unique identifier for a specific token. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tokens:write` scope. |

### Example request: Delete a token

```bash
$ curl -X DELETE "https://api.mapbox.com/tokens/v2/YOUR_MAPBOX_USERNAME/{token_id}?access_token=TOKEN_PLACEHOLDER::tokens:write"
```

### Response: Delete a token

```
HTTP 204 No Content
```

### Supported libraries: Delete a token

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#deletetoken)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Retrieve a token

**GET** : `https://api.mapbox.com/tokens/v2?access_token={access_token}`

Retrieve an access token and check whether it is valid. If the token is invalid, an explanation is returned as the `code` property in the response body.

| Required parameter | Type | Description |
| --- | --- | --- |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). This is the token to retrieve and validate. |

### Example request: Retrieve a token

```bash
$ curl "https://api.mapbox.com/tokens/v2?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Retrieve a token

The body of the token is parsed and included as the `token` property in object form. The returned object contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `code` | `string` | Indicates whether the token is valid. If the token is invalid, describes the reason. One of:
| Code | Description |
| --- | --- |
| `TokenValid` | The token is valid and active. |
| `TokenMalformed` | The token cannot be parsed. |
| `TokenInvalid` | The signature for the token does not validate. |
| `TokenExpired` | The token was temporary and has expired. |
| `TokenRevoked` | The token's authorization has been deleted. |

 |
| `token` | `object` | The token object. Contains the following properties: |
| `token.usage` | `string` | The token type. One of `pk`, `sk`, or `tk`. |
| `token.user` | `string` | The user to whom the token belongs. |
| `token.authorization` | `string` | The token's unique identifier. |
| `token.expires` | `string` | `tk` tokens only. The expiration time of the token. |
| `token.created` | `string` | `tk` tokens only. The creation time of the token. |
| `token.scopes` | `array` | `tk` tokens only. The token's assigned scopes. |
| `token.client` | `string` | `tk` tokens only. Always `"api"`. |

### Example response: Retrieve a token

For a public token:

```json
{
  "code": "TokenValid",
  "token": {
    "usage": "pk",
    "user": "mapbox",
    "authorization": "cijucimbe000brbkt48d0dhcx"
  }
}
```

For a temporary token:

```json
{
  "code": "TokenExpired",
  "token": {
    "usage": "tk",
    "user": "mapbox",
    "expires": "2016-09-15T19:27:53.000Z",
    "created": "2016-09-15T19:27:23.000Z",
    "scopes": ["styles:read", "fonts:read"],
    "client": "api"
  }
}
```

### Supported libraries: Retrieve a token

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#gettoken)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## List scopes

**GET** : `https://api.mapbox.com/scopes/v1/{username}?access_token={access_token}` [Token scope: **scopes:list**]

List scopes for a user. All potential scopes a user has access to are listed.

Public tokens may only contain scopes with the `public` property set to `true`. Secret tokens may contain any scope.

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account for which to list scopes. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `scopes:list` scope. |

### Example request: List scopes

```bash
$ curl "https://api.mapbox.com/scopes/v1/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::scopes:list"
```

### Response: List scopes

The response body will contain an object for each scope the user has access to, each with the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `id` | `string` | The identifier of the scope. |
| `description` | `string` | A description of permissions granted by the scope. |
| `public` | `boolean` | `true` if the scope is available for public tokens. |

### Example response (truncated): List scopes

```json
[
  {
    "id": "scopes:list",
    "description": "List all available scopes."
  },
  {
    "id": "styles:read",
    "public": true,
    "description": "Read styles."
  }
]
```

### Supported libraries: List scopes

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#listscopes)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Tokens API errors

| Response body `code` or `message` | HTTP status code | Description |
| --- | --- | --- |
| `TokenInvalid`, `TokenMalformed` | `200` | Check the access token used in the query when [retrieving a token](https://docs.mapbox.com/api/accounts/tokens/#retrieve-a-token). |
| `TokenExpired` | `200` | The temporary token has expired and needs to be regenerated when [retrieving a token](https://docs.mapbox.com/api/accounts/tokens/#retrieve-a-token). |
| `TokenRevoked` | `200` | The token has been revoked and needs to be regenerated when [retrieving a token](https://docs.mapbox.com/api/accounts/tokens/#retrieve-a-token). |
| `Unauthorized` | `401` | The token used in the query was not valid, or no token was used in the query. If a temporary token was used, it may be expired. |
| `Not found` | `404` | The access token used in the query needs the `tokens:read` (to list) or `tokens:write` [scope](https://docs.mapbox.com/accounts/guides/tokens/#scopes) (to create, update, or delete). This error may also mean that a token is not associated with a user account. |
| `No such user` | `404` | Check the username used in the query. |
| `access token is required` | `422` | No access token was used in the query. |
| `usage must be pk or sk` | `422` | The `usage` parameter must be one of `pk` (public token) or `sk` (secret token). |
| `expires is in the past` | `422` | You can't create a [temporary token](https://docs.mapbox.com/api/accounts/tokens/#create-a-temporary-token) with an `expires` parameter that occurs in the past. |
| `expires is more than one hour in the future` | `422` | When creating a temporary token, the expiration must be no more than one hour in the future. |
| `expires may not be greater than the expiration of the authorizing token` | `422` | When creating a temporary token using another temporary token, the expiration of the created token cannot be greater than that of the creating token. |
| `scopes are invalid` | `422` | You cannot create a new token with scopes that exceed those of the token you are using to create it. |
| `resources are invalid` | `422` | When creating or updating a token, the resources in the body are malformed, empty, or require higher permissions that those of the creating token. |
| `Internal Server Error` | `500` | This error can occur if the `start` value is not valid. |

## Tokens API restrictions and limits

-   Requests must be over HTTPS. HTTP is not supported.
-   The Tokens API is limited to 100 requests per minute per account. If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).
-   Each token is limited to 100 allowed URLs.
-   Temporary tokens cannot have allowed URLs, but public tokens and secret tokens can.