# Fonts API

The **Mapbox Fonts API** accepts fonts as raw binary data, allows those fonts to be deleted, and generates encoded letters for map renderers. Two types of fonts are supported: TrueType fonts, usually with `.ttf` file extensions, and OpenType fonts, with `.otf` extensions.

Fonts are managed on a per-account basis. Styles can use any font from the same account.

## Retrieve font glyph ranges

**GET** : `https://api.mapbox.com/fonts/v1/{username}/{font}/{start}-{end}.pbf` [Token scope: **fonts:read**]

While glyph ranges are usually not of interest unless you're building a map renderer, this is the endpoint you can use to access them.

Font glyph ranges are protocol buffer-encoded signed distance fields. They can be used to show fonts at a variety of scales and rotations. One glyph is used at all scales.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the font belongs. |
| `font` | `string` | The name of the font. This endpoint supports queries with a maximum of 10 comma-separated font names. |
| `start` | `integer` | A multiple of `256` between `0` and `65280`. |
| `end` | `integer` | The number indicated by `start`, plus `255`. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `fonts:read` scope. |

### Example request: Retrieve font glyph ranges

```bash
$ curl "https://api.mapbox.com/fonts/v1/YOUR_MAPBOX_USERNAME/Arial%20Unicode%20MS%20Regular/0-255.pbf?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

> **Note: Endpoint support**
> 
> Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:- [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#getfontglyphrange)See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

### Response: Retrieve font glyph ranges

A successful request will return `HTTP 200 Success`. The response body will be a buffer of the glyphs with `Content-Type: application/x-protobuf`.

Note: For historical reasons, this API will ignore the 'Accept-Encoding' header, and always encode the response with gzip.

## List fonts

**GET** : `https://api.mapbox.com/fonts/v1/{username}` [Token scope: **fonts:list**]

Retrieve a list of fonts for a specific account.

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the fonts belong. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `fonts:list` scope. |

### Example request: List fonts

```bash
$ curl "https://api.mapbox.com/fonts/v1/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::fonts:list"
```

### Response: List fonts

This endpoint returns an array of font names.

### Example response: List fonts

```json
["Custom Font Regular", "Custom Font Italic", "Custom Font Bold"]
```

## Add a font

**POST** : `https://api.mapbox.com/fonts/v1/{username}` [Token scope: **fonts:write**]

Adds a font to your account. The posted font must conform to the True Type Font (`.ttf`) or Open Type Font (`.otf`) file type. Invalid files will produce a descriptive validation error.

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the new font will be added. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `fonts:write` scope. |

### Example request: Add a font

```bash
$ curl -X POST "https://api.mapbox.com/fonts/v1/YOUR_MAPBOX_USERNAME/?access_token=TOKEN_PLACEHOLDER::fonts:write" \
  --data-binary @PermanentMarker-Regular.ttf
```

### Response: Add a font

The response will contain metadata about the font including `family_name`, `style_name`, `owner`, and `visibility`.

### Example response: Add a font

```json
{
  "family_name": "Permanent Marker",
  "style_name": "Regular",
  "owner": "YOUR_MAPBOX_USERNAME",
  "visibility": "private"
}
```

## Delete a font

**DELETE** : `https://api.mapbox.com/fonts/v1/{username}/{font}` [Token scope: **fonts:write**]

Delete a font from your account. WARNING: If you delete a font that's used by a map style, it may break your map!

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account from which the font will be deleted. |
| `font` | `string` | The name of the font to delete. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `fonts:write` scope. |

### Example request: Delete a font

```bash
$ curl -X DELETE "https://api.mapbox.com/fonts/v1/YOUR_MAPBOX_USERNAME/Custom Font Regular?access_token=TOKEN_PLACEHOLDER::fonts:write"
```

### Response: Delete a font

```
HTTP 204 No Content
```

## Retrieve font metadata

**GET** : `https://api.mapbox.com/fonts/v1/{username}/{font}/metadata` [Token scope: **fonts:read**]

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account for which the font metadata will be retrieved. |
| `font` | `string` | The name of the font for which the metadata will be retrieved. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `fonts:read` scope. |

### Example request: Retrieve font metadata

```bash
$ curl "https://api.mapbox.com/fonts/v1/YOUR_MAPBOX_USERNAME/Custom Font Regular/metadata?access_token=TOKEN_PLACEHOLDER::fonts:read"
```

### Response: Retrieve font metadata

```json
{
  "family_name": "Custom Font",
  "style_name": "Regular",
  "owner": "YOUR_MAPBOX_USERNAME",
  "visibility": "public"
}
```

## Update font metadata

**PATCH** : `https://api.mapbox.com/fonts/v1/{username}/{font}/metadata` [Token scope: **fonts:write**]

Update the `visibility` property of font metadata. Although the metadata contains a few properties, only `visibility` can be changed. The only valid values for `visibility` are `public` and `private`.

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account for which the font metadata will be updated. |
| `font` | `string` | The name of the font for which the metadata will be updated. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `fonts:write` scope. |

### Example request: Update font metadata

```bash
$ curl -X PATCH "https://api.mapbox.com/fonts/v1/YOUR_MAPBOX_USERNAME/Custom Font Regular/metadata?access_token=TOKEN_PLACEHOLDER::fonts:write" -H "Content-Type: application/json" -d '{"visibility": "public"}'
```

### Response: Update font metadata

```json
{
  "family_name": "Custom Font",
  "style_name": "Regular",
  "owner": "YOUR_MAPBOX_USERNAME",
  "visibility": "public"
}
```

## Fonts API errors

<table><thead><tr><th>Response body <code>message</code></th><th>HTTP status code</th><th>Description</th></tr></thead><tbody><tr><td><code>Invalid Range</code></td><td><code>400</code></td><td>Check the font glyph range. This error can also occur with empty fonts.</td></tr><tr><td><code>Maximum of 10 font faces permitted</code></td><td><code>400</code></td><td>Too many font faces in the query.</td></tr><tr><td><code>Not Authorized - No Token</code></td><td><code>401</code></td><td>No token was used in the query.</td></tr><tr><td><code>Not Authorized - Invalid Token</code></td><td><code>401</code></td><td>Check the access token you used in the query.</td></tr><tr><td><code>Forbidden</code></td><td><code>403</code></td><td>There may be an issue with your account. Check your <a href="https://console.mapbox.com/">Account page</a> for more details.<br><br>In some cases, using an access tokens with URL restrictions can also result in a <code>403</code> error. For more information, see our <a href="https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions">Token management guide</a>.</td></tr><tr><td><code>Not Found</code></td><td><code>404</code></td><td>Check the font name or names you used in the query.</td></tr><tr><td><code>Unprocessable Entity</code></td><td><code>422</code></td><td>Check the syntax of payload contents of the JSON in your request body when updating metadata.</td></tr></tbody></table>

## Fonts API restrictions and limits

-   Fonts must be smaller than 30 MB.
-   Accounts are limited to 100 fonts.
-   Responses from the Fonts API set both the device and CDN TTLs to 10 days.
-   For general information on caching, see the [Maps APIs caching dive deeper guide](https://docs.mapbox.com/help/dive-deeper/api-caching/).