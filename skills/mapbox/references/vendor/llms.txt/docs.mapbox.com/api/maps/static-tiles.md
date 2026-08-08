# Static Tiles API

The **Mapbox Static Tiles API** serves raster tiles generated from Mapbox Studio styles. Raster tiles can be used in traditional web mapping libraries like [Leaflet](https://leafletjs.com/), OpenLayers, and others to create interactive slippy maps. The Static Tiles API is well-suited for maps with limited interactivity or use on devices that do not support WebGL.

The **Mapbox Static Tiles API** is not compatible with the [Mapbox Standard](https://docs.mapbox.com/map-styles/guides/standard-styles/) and Mapbox Standard Satellite styles. Custom styles that import either of these styles are not supported.

Support for the Mapbox Standard and Mapbox Standard Satellite styles is planned for a future release.

## Retrieve raster tiles from styles

**GET** : `https://api.mapbox.com/styles/v1/{username}/{style_id}/tiles/{tilesize}/{z}/{x}/{y}{@2x}{.format}`

Retrieve raster tiles from a Mapbox Studio style.

-   The returned raster tile will be 512 pixels by 512 pixels by default.
-   You can use the `format` parameter to specify the tile output format, otherwise it will be defined implicitly, [depending on the used sources](#response-retrieve-raster-tiles-from-styles).

Leaflet.js uses this endpoint to render raster tiles from a Mapbox Studio style with [`L.tileLayer`](https://leafletjs.com/index.html#tilelayer).

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style from which to return a raster tile. |
| `{z}/{x}/{y}` | `integer` | The tile coordinates as described in the [Slippy Map Tilenames specification](http://wiki.openstreetmap.org/wiki/Slippy_map_tilenames). They specify the tile's zoom level `{z}`, column `{x}`, and row `{y}`. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `tilesize` | `integer` | The size in pixels of the returned tile, either `512` or `256`. The default is 512×512 pixels. Requesting 256×256 tiles from the endpoint can have significant cost implications because they are one quarter of the size of 512×512 tiles. Thus requiring 4 times as many API requests to render the same area. Review the [pricing guide](https://docs.mapbox.com/api/maps/static-tiles/#static-tiles-api-pricing) for details. |
| `@2x` | `string` | Render the raster tile at a `@2x` scale factor, so tiles are scaled to 1024×1024 pixels. |
| `format` | `string` | Format of the output tile. `png`, `jpeg` and `webp` values are supported. If not specified the tile format will be defined implicitly [depending on the used sources](#response-retrieve-raster-tiles-from-styles) |

> **Note: Zoom level and tile size**
> 
> Note that 512×512 image tiles are offset by one zoom level compared to 256×256 tiles. For example, 512×512 tiles at zoom level 4 are equivalent to 256×256 tiles at zoom level 5.

### Example request: Retrieve raster tiles from styles

```bash
# Returns a default 512×512 pixel tile as a JPEG
$ curl "https://api.mapbox.com/styles/v1/mapbox/satellite-v9/tiles/1/1/0?access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Returns a 1024×1024 pixel tile as a WebP
$ curl "https://api.mapbox.com/styles/v1/mapbox/satellite-v9/tiles/512/1/1/0@2x.webp?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Retrieve raster tiles from styles

-   The returned raster tile will be 512 pixels by 512 pixels unless you specify otherwise by using the optional `tilesize` or `@2x` parameters.
-   If the queried tileset contains raster layers, the returned tile will be a JPEG.
-   If the queried tileset contains only vector layers, the returned tile will be a PNG.

## Static Tiles API errors

<table><thead><tr><th>Response body <code>message</code></th><th>HTTP status code</th><th>Description</th></tr></thead><tbody><tr><td><code>Not Authorized - Invalid Token</code></td><td><code>401</code></td><td>Check the access token you used in the query.</td></tr><tr><td><code>Forbidden</code></td><td><code>403</code></td><td>There may be an issue with your account. Check your <a href="https://console.mapbox.com/">Account page</a> for more details.<br><br>In some cases, using an access tokens with URL restrictions can also result in a <code>403</code> error. For more information, see our <a href="https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions">Token management guide</a>.</td></tr><tr><td><code>Style not found</code></td><td><code>404</code></td><td>Check the style ID used in the query.</td></tr><tr><td><code>Classic styles are no longer supported</code></td><td><code>410</code></td><td>Classic styles are no longer supported.</td></tr><tr><td><code>Style may not composite raster sources with vector sources</code></td><td><code>400</code></td><td>The style the request uses contains a source reference that incorrectly composites sources of two different types.</td></tr><tr><td><code>Invalid style source</code></td><td><code>422</code></td><td>The <code>sources</code> key within the style your request references contains an invalid value.</td></tr><tr><td><code>Zoom level must be between 0-22.</code></td><td><code>422</code></td><td>The zoom level specified in the query is larger than 22 or contains non-numeric characters.</td></tr></tbody></table>

## Static Tiles API restrictions and limits

-   The default rate limit for the Mapbox Static Tiles API endpoint is 6,000 requests per minute. If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).
-   If you exceed the rate limit, you will receive an `HTTP 429 Too Many Requests` response. For information on rate limit headers, see the [Rate limit headers](https://docs.mapbox.com/api/guides/#rate-limit-headers) section.
-   The caching behavior of the Static Tiles API is different than that of other Mapbox services. The longest amount of time you could potentially wait until a change is propagated to a static map is 12 hours. The Static Tiles API endpoint sets the following caching headers in the response: `max-age=43200, s-maxage=604800` if the style uses `mapbox.satellite`, and `max-age=43200, s-maxage=43200` otherwise. These `Cache-Control` headers show how long a source is considered valid for either the client or any request handled by our CDN. So, it is expected caching behavior that your static map might take up to 12 hours to update after making changes. Note that styles or tilesets which set custom cache headers will override these default header values.
-   For general information on caching, see the [Maps APIs caching dive deeper guide](https://docs.mapbox.com/help/dive-deeper/api-caching/).

## Static Tiles API pricing

-   Billed by **API requests**
-   See rates and discounts per Static Tiles API request in the pricing page's **[Maps](https://www.mapbox.com/pricing/#gltile)** section

The Static Tiles API is measured in **API requests**. It can be used in conjunction with a raster tile client such as Leaflet, a third-party tool such as ArcGIS or Carto, or a hybrid framework. Note that these tiling clients are not actively maintained by Mapbox. Details about the number of requests included in the free tier and the cost per request beyond what is included in the free tier are available on the [pricing page](https://www.mapbox.com/pricing/#gltile).

### Manage Static Tiles API costs

The Static Tiles API returns 512×512 pixel map tiles by default. While it is possible to use the optional [`tilesize`](https://docs.mapbox.com/api/maps/static-tiles/#retrieve-raster-tiles-from-styles) parameter to request 256x256 pixel tiles, those tiles are one quarter the size of 512x512 tiles and require 4 times as many billable API requests to render a map covering the same area. We recommend that you avoid specifying a `tileSize` of 256 pixels to prevent unnecessarily high request volume and associated costs.

Some mapping libraries (including leaflet) expect 256x256 pixel tiles but support using a zoom level offset to accurately render 512x512 tiles when specified. Your implementation will vary if you are using a different mapping library.

-   If you are using Leaflet, you can use the [`zoomOffset`](https://leafletjs.com/reference.html#tilelayer-zoomoffset) and [`tileSize`](https://leafletjs.com/reference.html#gridlayer-tilesize) options.

You can also request 256×256 pixel raster tiles directly with these libraries and the [Raster Tiles API](https://docs.mapbox.com/api/maps/raster-tiles/) by referencing a [tileset ID](https://docs.mapbox.com/help/glossary/tileset-id/) like `mapbox.satellite` in your implementation. Consider setting [`maxBounds`](https://leafletjs.com/reference.html#map-maxbounds) and [`maxZoom`](https://leafletjs.com/reference.html#map-maxzoom) in the tiling client to limit the number of tiles end users can load. For highly interactive applications, we recommend transitioning to Mapbox GL JS, which is billed by [map loads](https://docs.mapbox.com/help/glossary/map-loads/) instead of API requests. For additional options, review the [guide to managing web map costs](https://docs.mapbox.com/help/troubleshooting/manage-web-map-costs/).