# Styles API

The **Mapbox Styles API** lets you read and change map styles, fonts, and images. This API is the basis for [Mapbox Studio](https://console.mapbox.com/studio/).

If you use Studio, [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js/api/), or the [Mapbox Mobile SDKs](https://www.mapbox.com/mobile/), you are already using the Styles API. This documentation is useful for software developers who want to programmatically read and write these resources. It isn't necessary for you to read or understand this reference to design or use Mapbox maps.

You will need to be familiar with the [Mapbox Style Specification](https://docs.mapbox.com/style-spec/) to use the Styles API. The Mapbox Style Specification defines the structure of map styles and is the open standard that helps Studio communicate with APIs and produce maps that are compatible with Mapbox libraries.

## Mapbox styles

Mapbox provides a range of map styles designed for various use cases. For a full list of available styles, their style URLs, and guidance on when to use each one, see the [Map Styles guides](https://docs.mapbox.com/map-styles/guides/) and [Mapbox Styles reference documentation](https://docs.mapbox.com/map-styles/reference/).

## The style object

A style object is an object that conforms to the [Mapbox Style Specification](https://docs.mapbox.com/style-spec/), with some additional account-related properties:

| Property | Type | Description |
| --- | --- | --- |
| `version` | `number` | The style specification version number. |
| `name` | `string` | A human-readable name for the style. |
| `metadata` | `object` | Information about the style that is used in Mapbox Studio. |
| `sources` | `object` | Sources supply the data that will be displayed on the map. |
| `layers` | `array` | Layers will be created in the order of this array. |
| `created` | `string` | The date and time the style was created. |
| `id` | `string` | The ID of the style. |
| `modified` | `string` | The date and time the style was last modified. |
| `owner` | `string` | The username of the style owner. |
| `visibility` | `string` | Access control for the style, either `public` or `private`. Private styles require an access token belonging to the owner. Public styles may be requested with an access token belonging to any user. |
| `protected` | `boolean` | Indicates whether the style is protected (`true`) or not (`false`). Protected styles cannot be edited and deleted. |
| `draft` | `boolean` | Indicates whether the style is a draft (`true`) or whether it has been published (`false`). |

A style object must conform to the following rules:

-   Must be valid JSON
-   Must be aligned to the most recent version of the [Mapbox Style Specification](https://docs.mapbox.com/style-spec/)
-   Can be composed of no more than 15 [`sources`](https://docs.mapbox.com/style-spec/reference/sources/)
-   Cannot contain any keys in the body of the style beyond those listed in the [Style Specification](https://docs.mapbox.com/style-spec/)
-   The `url` property in the [source object](https://docs.mapbox.com/style-spec/reference/sources/) must be a valid Mapbox tileset ID
-   Only [raster](https://docs.mapbox.com/style-spec/reference/sources/#raster) and [vector](https://docs.mapbox.com/style-spec/reference/sources/#vector) sources are supported

You can use the `gl-style-validate` CLI tool with the [`--mapbox-api-supported` flag](https://github.com/mapbox/mapbox-gl-js/tree/main/src/style-spec#gl-style-validate) to validate a style object. Invalid styles will produce a descriptive validation error.

### Example style object

```json
{
  "version": 8,
  "name": "{name}",
  "metadata": "{metadata}",
  "sources": "{sources}",
  "sprite": "mapbox://sprites/{username}/{style_id}",
  "glyphs": "mapbox://fonts/{username}/{fontstack}/{range}.pbf",
  "layers": ["{layers}"],
  "created": "2015-10-30T22:18:31.111Z",
  "id": "{style_id}",
  "modified": "2015-10-30T22:22:06.077Z",
  "owner": "{username}",
  "visibility": "private",
  "protected": false,
  "draft": true
}
```

## Drafts

The Styles API supports drafts, so every style can have both published and draft versions. This means that you can make changes to a style without publishing them or deploying them in your app. For each style-related endpoint, you can interact with the draft version of a style by placing `draft/` after the style ID, like `/styles/v1/{username}/{style_id}/draft/sprite`.

Note that the style you see in Studio's style editor is always the draft version. If you make changes to the published version of a style or sprite by calling the API without `draft/` in the URI, those changes will not be reflected in the draft version.

## Retrieve a style

**GET** : `https://api.mapbox.com/styles/v1/{username}/{style_id}` [Token scope: **styles:read**]

Retrieve a style as a JSON document.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style to be retrieved. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:read` scope. |

### Example request: Retrieve a style

```bash
$ curl "https://api.mapbox.com/styles/v1/examples/cjikt35x83t1z2rnxpdmjs7y7?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Retrieve a style

The returned [style object](#the-style-object) will be in the [Mapbox Style](https://docs.mapbox.com/style-spec/) format.

### Example response: Retrieve a style

```json
{
  "version": 8,
  "name": "Meteorites",
  "metadata": {
    "mapbox:origin": "basic-template-v1",
    "mapbox:autocomposite": true,
    "mapbox:type": "template",
    "mapbox:sdk-support": {
      "js": "0.45.0",
      "android": "6.0.0",
      "ios": "4.0.0"
    }
  },
  "center": [74.24426803763072, -2.2507114487818853],
  "zoom": 0.6851443156248076,
  "bearing": 0,
  "pitch": 0,
  "sources": {
    "composite": {
      "url": "mapbox://mapbox.mapbox-streets-v8,examples.0fr72zt8",
      "type": "vector"
    }
  },
  "sprite": "mapbox://sprites/examples/cjikt35x83t1z2rnxpdmjs7y7",
  "glyphs": "mapbox://fonts/{username}/{fontstack}/{range}.pbf",
  "layers": [
    {
      "id": "background",
      "type": "background",
      "layout": {},
      "paint": {
        "background-color": []
      }
    },
    {}
  ],
  "created": "2015-10-30T22:18:31.111Z",
  "id": "cjikt35x83t1z2rnxpdmjs7y7",
  "modified": "2015-10-30T22:22:06.077Z",
  "owner": "examples",
  "visibility": "public",
  "protected": true,
  "draft": false
}
```

### Supported libraries: Retrieve a style

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#getstyle)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## List styles

**GET** : `https://api.mapbox.com/styles/v1/{username}` [Token scope: **styles:list**]

Retrieve a list of styles for a specific account. This endpoint supports [pagination](https://docs.mapbox.com/api/guides/#pagination). Since styles are generally quite large, it's likely that a response to this endpoint will start paginating sooner than other list endpoints. If you have many styles in your account, you may need to repeatedly use the `next` link relation in the [`Link` header](http://tools.ietf.org/html/rfc5988) of the response to retrieve them all.

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the styles belong. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:list` scope. |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `draft` | `boolean` | List only [draft](#drafts) styles (`true`), or return all styles (`false`, default). |
| `limit` | `integer` | The maximum number of styles to return. |
| `start` | `string` | The ID of the style after which to start the listing. Find the style ID in the `Link` header of a response. See the [pagination](https://docs.mapbox.com/api/guides/#pagination) section for details. |

### Example request: List styles

```bash
$ curl "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::styles:list"
```

### Response: List styles

This endpoint returns style metadata instead of returning full styles.

### Example response: List styles

```json
[
  {
    "version": 8,
    "name": "My Awesome Style",
    "created": "{timestamp}",
    "id": "cige81msw000acnm7tvsnxcp5",
    "modified": "{timestamp}",
    "owner": "{username}"
  },
  {
    "version": 8,
    "name": "My Cool Style",
    "created": "{timestamp}",
    "id": "cig9rvfe300009lj9kekr0tm2",
    "modified": "{timestamp}",
    "owner": "{username}"
  }
]
```

### Supported libraries: List styles

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#liststyles)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Retrieve a style ZIP bundle

**GET** : `https://api.mapbox.com/styles/v1/{username}/{style_id}.zip` [Token scope: **styles:download**]

> **Note**
> 
> Access to this endpoint is available upon request. To request that access be enabled for your account, contact [Mapbox support](https://support.mapbox.com/hc/en-us).

Retrieves a ZIP file containing the [style JSON](https://docs.mapbox.com/help/glossary/style/), [sprite images](https://docs.mapbox.com/help/glossary/sprite/), referenced custom fonts, and a license file. After retrieval, the style ZIP bundle response is cached for a few minutes, so later requests may return the same content even if the style has been modified in the interim.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style to be retrieved. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:download` scope. |

| Optional path parameters | Type | Description |
| --- | --- | --- |
| `draft` | `string` | If used, indicates that the style is a draft and has not been published. For more information, see the [Drafts](#drafts) section. |

### Example request: Retrieve a style ZIP bundle

```bash
# Request the style bundle for a published style
$ curl "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME/cjikt35x83t1z2rnxpdmjs7y7.zip?access_token=TOKEN_PLACEHOLDER::styles:download"

# Request the style bundle for a draft style
$ curl "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME/cjikt35x83t1z2rnxpdmjs7y7/draft.zip?access_token=TOKEN_PLACEHOLDER::styles:download"
```

### Response: Retrieve a style ZIP bundle

The response will be a ZIP file named for the downloaded style. It will contain the relevant `style.json`, sprite images, referenced custom fonts, and a `license.txt` file. The hierarchy is illustrated below.

```
Monochrome-draft(cjikt35x83t1z2rnxpdmjs7y7)/
├── fonts/
│   ├── My Font Bold.ttf
│   └── My Font Regular.ttf
├── license.txt
├── sprite_images/
│   ├── aquarium-11.svg
│   ├── bank-15.svg
│   ├── car-11.svg
│   └── ...
└── style.json
```

## Create a style

**POST** : `https://api.mapbox.com/styles/v1/{username}` [Token scope: **styles:write**]

Creates a style in your account. The posted style object must conform to the rules outlined in the [style object](#the-style-object) section of this documentation. Invalid styles will produce a descriptive validation error.

Additionally, when you create a style using the Styles API:

-   The [`glyphs` field](https://docs.mapbox.com/style-spec/reference/root/#glyphs) will be overwritten to point to your user glyph endpoint, unless it's referring to the Mapbox glyph endpoint, `mapbox://fonts/mapbox/{fontstack}/{range}.pbf`.
-   If the [`sprite` field](https://docs.mapbox.com/style-spec/reference/root/#sprite) does not include your username, and the sprite field points to the sprite of a style that either belongs to `mapbox` or is public, the Styles API will copy all images to the new style's spritesheet and overwrite the `sprite` value to point to the new style's sprite.
-   If the [optional `name` property](https://docs.mapbox.com/style-spec/reference/root/#name) is not used in the request body, the `name` of the new style will be automatically set to the style's ID.

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the new style will belong. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:write` scope. |

### Example request: Create a style

```bash
$ curl -X POST "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::styles:write" \
  --data @basic-v9.json \
  --header "Content-Type:application/json"
```

### Example request body: Create a style

```json
{
  "version": 8,
  "name": "My Awesome Style",
  "metadata": {},
  "sources": {
    "myvectorsource": {
      "url": "mapbox://{tileset_id}",
      "type": "vector"
    },
    "myrastersource": {
      "url": "mapbox://{tileset_id}",
      "type": "raster"
    }
  },
  "glyphs": "mapbox://fonts/{username}/{fontstack}/{range}.pbf",
  "layers": []
}
```

### Response: Create a style

The style you get back from the API will contain new properties that the server has added: `created`, `id`, `modified`, `owner`, and `draft`.

### Example response: Create a style

```json
{
  "version": 8,
  "name": "My Awesome Style",
  "metadata": {},
  "sources": {
    "myvectorsource": {
      "url": "mapbox://{tileset_id}",
      "type": "vector"
    },
    "myrastersource": {
      "url": "mapbox://{tileset_id}",
      "type": "raster"
    }
  },
  "sprite": "mapbox://sprites/{username}/{style_id}",
  "glyphs": "mapbox://fonts/{username}/{fontstack}/{range}.pbf",
  "layers": [],
  "created": "2015-10-30T22:18:31.111Z",
  "id": "{style_id}",
  "modified": "2015-10-30T22:22:06.077Z",
  "owner": "{username}",
  "draft": true
}
```

### Supported libraries: Create a style

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#createstyle)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Update a style

**PATCH** : `https://api.mapbox.com/styles/v1/{username}/{style_id}` [Token scope: **styles:write**]

Updates an existing style in your account with new content. The request body must be a style object that conforms to the rules outlined in the [style object](#the-style-object) section of this documentation. Invalid styles will produce a descriptive validation error.

Additionally, when you update a style using the Styles API:

-   The `name` property, which is optional for [creating a style](#create-a-style), is required in the request body to update a style.
-   If you request a style and then use the unaltered response to update the style, this action will fail. You must remove the `created` and `modified` properties before updating a style.
-   The [`glyphs` field](https://docs.mapbox.com/style-spec/reference/root/#glyphs) will be overwritten to point to your user glyph endpoint, unless it's referring to the Mapbox glyph endpoint, `mapbox://fonts/mapbox/{fontstack}/{range}.pbf`.
-   If the [`sprite` field](https://docs.mapbox.com/style-spec/reference/root/#sprite) does not include your username, and the sprite field points to the sprite of a style that either belongs to `mapbox` or is public, the Styles API will copy all images to the updated style's spritesheet and overwrite the `sprite` value to point to the updated style's sprite.

Cross-version `PATCH` requests are rejected.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style to be updated. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:write` scope. |

### Example request: Update a style

```bash
$ curl -X PATCH "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME/{style_id}?access_token=TOKEN_PLACEHOLDER::styles:write" \
  --data @basic-v9.json \
  --header "Content-Type:application/json"
```

### Example request body: Update a style

```json
{
  "version": 8,
  "name": "New Style Name",
  "metadata": {},
  "sources": {},
  "sprite": "mapbox://sprites/{username}/{style_id}",
  "glyphs": "mapbox://fonts/{username}/{fontstack}/{range}.pbf",
  "layers": [
    {
      "id": "new-layer",
      "type": "background",
      "paint": {
        "background-color": "#111"
      },
      "interactive": true
    }
  ],
  "owner": "{username}",
  "draft": true
}
```

### Response: Update a style

A successful request to this endpoint will return the updated [style object](#the-style-object).

### Example response: Update a style

```json
{
  "version": 8,
  "name": "New Style Name",
  "metadata": {},
  "sources": {},
  "sprite": "mapbox://sprites/{username}/{style_id}",
  "glyphs": "mapbox://fonts/{username}/{fontstack}/{range}.pbf",
  "layers": [
    {
      "id": "new-layer",
      "type": "background",
      "paint": {
        "background-color": "#111"
      },
      "interactive": true
    }
  ],
  "created": "2015-10-30T22:18:31.111Z",
  "id": "{style_id}",
  "modified": "2015-10-30T22:22:06.077Z",
  "owner": "{username}",
  "draft": true
}
```

### Supported libraries: Update a style

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#updatestyle)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Delete a style

**DELETE** : `https://api.mapbox.com/styles/v1/{username}/{style_id}` [Token scope: **styles:write**]

Delete a style. All sprites that belong to this style will also be deleted, and the style will no longer be available.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style to be deleted. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:write` scope. |

### Example request: Delete a style

```bash
$ curl -X DELETE "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME/{style_id}?access_token=TOKEN_PLACEHOLDER::styles:write"
```

### Response: Delete a style

```
HTTP 204 No Content
```

### Supported libraries: Delete a style

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#deletestyle)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Protect a style

**PUT** : `https://api.mapbox.com/styles/v1/{username}/{style_id}/protected` [Token scope: **styles:protect**]

> **Note**
> 
> Access to this endpoint is available upon request. To request that access be enabled for your account, contact [Mapbox support](https://support.mapbox.com/hc/en-us).

Updates the protected status of a style. The request body must be a plain text string either `true` or `false`.

Changing the status is only valid for styles without an active draft (which has a `modified` field ahead of the published version). This update will not change the style's `modified` field or sprite hashes. Protected styles cannot be [edited](#update-a-style) and [deleted](#delete-a-style) using the Styles API or in [Mapbox Studio](https://console.mapbox.com/studio/).

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style to be protected. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:protect` scope. |

### Example request: Protect a style

```bash
$ curl "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME/cjikt35x83t1z2rnxpdmjs7y7/protected?access_token=TOKEN_PLACEHOLDER::styles:protect" \
  --data-raw "true" \
  --header "Content-Type: text/plain"
```

### Response: Protect a style

```bash
{
    "protected": true
}
```

### Response to invalid request: Protect a draft style

You cannot protect a style with an active draft. This endpoint will return a `400 Bad Request` error.

[Publish](https://docs.mapbox.com/studio-manual/reference/styles/#publish) the draft style or [revert](https://docs.mapbox.com/studio-manual/reference/styles/#revert) to its last published version before protecting.

## Request embeddable HTML

**GET** : `https://api.mapbox.com/styles/v1/{username}/{style_id}.html` [Token scope: **styles:read**]

Request embeddable or shareable HTML.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style to be embedded. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:read` scope. |

The embeddable HTML that is returned can be further modified with the following optional query parameters:

| Optional path parameters | Type | Description |
| --- | --- | --- |
| `draft` | `string` | Retrieve the draft version of a style. For more information, see the [Drafts](#drafts) section. |

| Optional query parameters | Type | Description |
| --- | --- | --- |
| `zoomwheel` | `boolean` | Whether to provide a zoom wheel, which enables a viewer to zoom in and out of the map using the mouse (`true`, default), or not (`false`). |
| `title` | `string` | Display a title box with the map's title, owner, and a default message along the bottom of the map. Possible values are `copy` (message reads "Copy this style to your account" and provides a **Copy** button) and `view` (message reads "Design your own maps with Mapbox Studio" and provides a **Sign Up** button). The `copy` option will only work if a style's [`visibility` is `public`](#the-style-object). If this parameter is not used or its value is `false`, a title box is not displayed. |
| `fallback` | `boolean` | Serve a fallback raster map (`true`) or not (`false`, default). |
| `mapboxGLVersion` | `string` | Specify a version of [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js/api/) to use to render the map. |
| `mapboxGLGeocoderVersion` | `string` | Specify a version of the [Mapbox GL geocoder plugin](https://github.com/mapbox/mapbox-gl-geocoder) to use to render the map search box. |
| `hash` | `number` | Specify a zoom level and location for the map to center on, in the format `#zoom/latitude/longitude/bearing/pitch`. [Bearing and pitch](https://docs.mapbox.com/help/glossary/camera/) are optional, and both values will default to 0° if not specified. The hash is placed after the `access_token` in the request. |

### Example: Request embeddable HTML

```html
{/* Map is centered on San Francisco at z15, with a bearing of 45° and a pitch
of 70°. */}
<iframe
  src="https://api.mapbox.com/styles/v1/mapbox/streets-v12.html?title=true&zoomwheel=false&access_token=YOUR_MAPBOX_ACCESS_TOKEN#15/37.771/-122.436/45/70"
/>
```

### Supported libraries: Request embeddable HTML

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#getembeddablehtml)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Retrieve a map's WMTS document

**GET** : `https://api.mapbox.com/styles/v1/{username}/{style_id}/wmts`

Mapbox supports access via the [WMTS](http://www.opengeospatial.org/standards/wmts) standard, which lets you use maps with desktop and online GIS software like ArcMap and QGIS.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style for which to return a WMTS document. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |

### Example request: Retrieve a map's WMTS document

```bash
$ curl "https://api.mapbox.com/styles/v1/mapbox/streets-v12/wmts?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Retrieve a map's WMTS document

The response to a request to this endpoint will be a map's WMTS document.

## Sprites

Sprites are the way that Mapbox GL JS and the Mapbox mobile SDKs efficiently request and show images. Sprites are collections of images that can be used in styles as icons or patterns in `symbol` layers. An image in a sprite can be an icon, a pattern, or an illustration. These SVG images can be added and removed from the sprite at will. The Styles API automatically collects these SVG images and renders them into a single PNG image and a JSON document that describes where each image is positioned.

The sprite JSON document is specified as part of [the Mapbox Style Specification](https://docs.mapbox.com/style-spec/reference/sprite/).

Sprites are managed on a per-style basis. Each sprite belongs to a style, so the sprite limit of 1,000 images is also a per-style limit.

Sprite images are PNG-8 files that are optimized to have a limited color palette. Usually, visual differences from the source SVG files are not noticeable but you may in some cases notice slight changes in colors or dithering when inspecting sprite images closely.

All sprite-related API methods require a `{style_id}` parameter that refers to the style to which the sprite belongs.

## Retrieve a sprite image or JSON

**GET** : `https://api.mapbox.com/styles/v1/{username}/{style_id}/{sprite_id}/sprite{@2x}.{format}` [Token scope: **styles:read**]

Retrieve a sprite image or its JSON document from a Mapbox style.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style to which the sprite belongs. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:read` scope. |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `sprite_id` | `string` | The ID of the immutable sprite. To learn how to find a sprite's unique ID, see the [How to retrieve a sprite ID](#how-to-retrieve-a-sprite-id) section. |
| `@2x` | `string` | Render the sprite at a `@2x`, `@3x`, or `@4x` scale factor for high-density displays. Decimal values such as `@2.5x` are also supported. |
| `format` | `string` | By default, this endpoint returns a sprite's JSON document. Specify `.png` to return the sprite image instead. |

### Example request: Retrieve a sprite image or JSON

```bash
# Request the sprite image as a png

$ curl "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME/{style_id}/sprite.png?access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request json for a 3x scale sprite

$ curl "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME/{style_id}/sprite@3x?access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request the immutable sprite image as a png

$ curl "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME/{style_id}/{sprite_id}/sprite.png?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Retrieve a sprite image or JSON

The response to a successful request to this endpoint is either a sprite image or its JSON response, depending on which was requested.

### Example response: Retrieve a sprite image or JSON

```json
{
  "default_marker": {
    "width": 20,
    "height": 50,
    "x": 0,
    "y": 0,
    "pixelRatio": 2
  },
  "secondary_marker": {
    "width": 20,
    "height": 50,
    "x": 20,
    "y": 0,
    "pixelRatio": 2
  }
}
```

### How to retrieve a sprite ID

Because sprites tend to stay static for long durations, we offer immutable sprites with unique URLs. This allows us to have long cache durations resulting in faster load times. This also prevents styles from breaking if the sprites are updated "beneath" them. These unique URLs are possible because of the `sprite_id` that can be placed in the URL.

After you have uploaded your sprite images using the [Add new image to sprite](#add-new-image-to-sprite) endpoint or the [Add multiple new images to sprite](#add-multiple-new-images-to-sprite) endpoint, make a request to [update your style](#update-a-style). The response payload for the style update will contain the unique sprite URL on the `sprite` payload.

Note: [Mapbox Studio](https://console.mapbox.com/studio/) does all this work for you automatically.

### Supported libraries: Retrieve a sprite image or JSON

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#getstylesprite)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Add new image to sprite

**PUT** : `https://api.mapbox.com/styles/v1/{username}/{style_id}/sprite/{icon_name}` [Token scope: **styles:write**]

Add a new image to an existing sprite in a Mapbox style. The request body should be [raw SVG data](https://docs.mapbox.com/help/glossary/svg/).

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style to which the sprite belongs. |
| `icon_name` | `string` | The name of the new image that is being added to the style. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:write` scope. |

### Example request: Add new image to sprite

```bash
# Add a new image (`aerialway`) to an existing sprite

$ curl -X PUT \
  "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME/{style_id}/sprite/aerialway?access_token=TOKEN_PLACEHOLDER::styles:write" \
  --data @aerialway-12.svg
```

### Response: Add new image to sprite

The response to a successful request to this endpoint will be the updated sprite.

### Example response: Add new image to sprite

```json
{
  "newsprite": {
    "width": 1200,
    "height": 600,
    "x": 0,
    "y": 0,
    "pixelRatio": 1
  },
  "default_marker": {
    "width": 20,
    "height": 50,
    "x": 0,
    "y": 600,
    "pixelRatio": 1
  }
}
```

### Supported libraries: Add new image to sprite

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#putstyleicon)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Add multiple new images to sprite

**POST** : `https://api.mapbox.com/styles/v1/{username}/{style_id}/sprite` [Token scope: **styles:write**]

Add a batch of new images to an existing sprite in a Mapbox style. The request body must be multipart form data that uses the form field name `images` to reference the SVG image files.

A request can contain a maximum of 25 image files. Each individual image file in a request must be under 30 KB.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style to which the sprite belongs. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:write` scope. |

### Example request: Add multiple new images to sprite

```bash
# Add two new images from local files ('star.svg' and 'square.svg') to an existing sprite:

$ curl -F images=@star.svg -F images=@square.svg
  "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME/{style_id}/sprite?access_token=TOKEN_PLACEHOLDER::styles:write"
```

### Response: Add multiple new images to sprite

The response to a successful request to this endpoint will be the updated sprite.

### Example response: Add multiple new images to sprite

```json
{
  "star": {
    "width": 15,
    "height": 15,
    "x": 0,
    "y": 0,
    "pixelRatio": 1
  },
  "square": {
    "width": 15,
    "height": 15,
    "x": 0,
    "y": 15,
    "pixelRatio": 1
  },
  "default_marker": {
    "width": 20,
    "height": 50,
    "x": 0,
    "y": 65,
    "pixelRatio": 1
  }
}
```

## Delete image from sprite

**DELETE** : `https://api.mapbox.com/styles/v1/{username}/{style_id}/sprite/{icon_name}` [Token scope: **styles:write**]

Remove an image from an existing sprite.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style to which the sprite belongs. |
| `icon_name` | `string` | The name of the new image to delete from the style. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:write` scope. |

### Example request: Delete image from sprite

```bash
$ curl -X DELETE "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME/{style_id}/sprite/{icon_name}?access_token=TOKEN_PLACEHOLDER::styles:write"
```

### Response: Delete image from sprite

The response to a successful request to this endpoint will be the modified sprite.

### Example response: Delete image from sprite

```json
{
  "default_marker": {
    "width": 20,
    "height": 50,
    "x": 0,
    "y": 600,
    "pixelRatio": 1
  },
  "secondary_marker": {
    "width": 20,
    "height": 50,
    "x": 20,
    "y": 600,
    "pixelRatio": 1
  }
}
```

### Supported libraries: Delete image from sprite

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#deletestyleicon)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Delete multiple images from sprite

**DELETE** : `https://api.mapbox.com/styles/v1/{username}/{style_id}/sprite` [Token scope: **styles:write**]

Remove a batch of images from an existing sprite.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style to which the sprite belongs. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:write` scope. |

The request body should be a JSON-encoded array of the names of the images you want to delete from the specified sprite.

### Example request: Delete multiple images from sprite

```bash
# Delete two images named "star" and "square" from an existing sprite:

$ curl -X DELETE "https://api.mapbox.com/styles/v1/YOUR_MAPBOX_USERNAME/{style_id}/sprite/?access_token=TOKEN_PLACEHOLDER::styles:write" \
  --data '["star", "square"]' \
  --header "Content-Type:application/json"
```

### Response: Delete multiple images from sprite

The response to a successful request to this endpoint will be the modified sprite.

### Example response: Delete multiple images from sprite

```json
{
  "default_marker": {
    "width": 20,
    "height": 50,
    "x": 0,
    "y": 600,
    "pixelRatio": 1
  },
  "secondary_marker": {
    "width": 20,
    "height": 50,
    "x": 20,
    "y": 600,
    "pixelRatio": 1
  }
}
```

## Styles API errors

<table><thead><tr><th>Response body <code>message</code></th><th>HTTP status code</th><th>Description</th></tr></thead><tbody><tr><td><code>Not Authorized - Invalid Token</code></td><td><code>401</code></td><td>Check the access token you used in the query.</td></tr><tr><td><code>This endpoint requires a token with {scope} scope</code></td><td><code>403</code></td><td>The access token used in the query needs the specified <a href="https://docs.mapbox.com/accounts/guides/tokens/#scopes">scope</a>.</td></tr><tr><td><code>Forbidden</code></td><td><code>403</code></td><td>You do not have permission to view styles for the requested account.<br><br>In some cases, using an access tokens with URL restrictions can also result in a <code>403</code> error. For more information, see our <a href="https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions">Token management guide</a>.</td></tr><tr><td><code>Style not found</code></td><td><code>404</code></td><td>Check the style ID used in the query.</td></tr><tr><td><code>Failed to create style</code></td><td><code>422</code></td><td>Check the syntax of the JSON in your request body when creating a style.</td></tr></tbody></table>

## Styles API restrictions and limits

-   The default read rate limit for the Mapbox Styles API endpoint is 2,000 requests per minute and the write rate limit is 50. If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).
-   If you exceed the rate limit, you will receive an `HTTP 429 Too Many Requests` response. For information on rate limit headers, see the [Rate limit headers](https://docs.mapbox.com/api/guides/#rate-limit-headers) section.
-   Styles cannot reference more than 15 sources.
-   Styles cannot be larger than 1 MB. This limit only applies to the style document itself, not the sprites, fonts, tilesets, or other resources it references.
-   An account is allowed to have an unlimited number of styles regardless of its [pricing plan](https://www.mapbox.com/pricing/).
-   Responses from the Styles API set both the device and CDN TTLs to 15 minutes.
-   For general information on caching, see the [Maps APIs caching dive deeper guide](https://docs.mapbox.com/help/dive-deeper/api-caching/).

## Styles API sprites restrictions and limits

-   Each image must be smaller than 400 KB.
-   Mapbox supports most, but not all, SVG properties. These limits are described in our [SVG troubleshooting guide](https://docs.mapbox.com/help/troubleshooting/studio-svg-upload-errors/).
-   Images can be up to 512 pixels in each dimension.
-   Image names must be fewer than 255 characters in length.
-   Sprites can contain up to 1,000 images.