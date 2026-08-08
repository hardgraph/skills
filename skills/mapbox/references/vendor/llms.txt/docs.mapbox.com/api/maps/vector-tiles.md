# Vector Tiles API

The **Mapbox Vector Tiles API** serves [vector tiles](https://docs.mapbox.com/help/glossary/vector-tiles/) from Mapbox-hosted vector tilesets.

## Retrieve vector tiles

**GET** : `https://api.mapbox.com/v4/{tileset_id}/{zoom}/{x}/{y}.{format}`

| Required parameters | Type | Description |
| --- | --- | --- |
| `tileset_id` | `string` | Unique identifier for the vector tileset in the format `username.id`. To composite multiple vector tilesets, use a comma-separated list of up to 15 tileset IDs. |
| `zoom` | `integer` | Specifies the tile's zoom level, as described in the [Slippy Map Tilenames](http://wiki.openstreetmap.org/wiki/Slippy_map_tilenames) specification. |
| `{x}/{y}` | `integer` | Specifies the tile's column `{x}` and row `{y}`, as described in the [Slippy Map Tilenames](http://wiki.openstreetmap.org/wiki/Slippy_map_tilenames) specification. |
| `format` | `string` | Specifies the format of the returned tiles:
<table class="my12"><tbody><tr><td><code>.mvt</code></td><td>Vector tile</td></tr><tr><td><code>.vector.pbf</code></td><td>Vector tile</td></tr></tbody></table>

 |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `style` | `string` | Required for style-optimized tile requests. The `style` parameter has two parts, the style ID and the style's recently edited `timestamp`, in the format `<style ID>@<timestamp>`. The timestamp parameter comes from the style JSON's `modified` property, which is included with any style created with Mapbox Studio. |

> **Note: Retrieve style-optimized vector tiles**
> 
> Vector tiles can be further optimized by including the [style ID](https://docs.mapbox.com/api/maps/styles/#the-style-object) with the tile request. If the style parameter is provided, the sources, [filters](https://docs.mapbox.com/style-spec/reference/layers/#filter), [`minzoom`](https://docs.mapbox.com/style-spec/reference/sources/#vector-minzoom), and [`maxzoom`](https://docs.mapbox.com/style-spec/reference/sources/#vector-maxzoom) properties of that style are analyzed, and data that won't be visible on the map is removed from the vector tile. Mapbox GL JS can request style-optimized vector tiles that are hosted on Mapbox with a Mapbox Style JSON.Unused layers and features are removed from optimized styles. If you plan to dynamically change the style at runtime using Mapbox GL JS or a Mapbox mobile SDK, broadening filters and zoom ranges won't work the same way since any data that isn't visible with the loaded style also won't be included in the data.

### Example request: Retrieve vector tiles

```bash
$ curl "https://api.mapbox.com/v4/mapbox.mapbox-streets-v8/1/0/0.mvt?access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Return a style-optimized tile using the style query parameter

$ curl "https://api.mapbox.com/v4/mapbox.mapbox-streets-v8/12/1171/1566.mvt?style=mapbox://styles/mapbox/streets-v12@00&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Retrieve vector tiles

The response is a vector tile in the specified format. For performance, image tiles are delivered with a `max-age` header value set 12 hours in the future.

## Vector Tiles API errors

<table><thead><tr><th>Response body <code>message</code></th><th>HTTP status code</th><th>Description</th></tr></thead><tbody><tr><td><code>Not Authorized - No Token</code></td><td><code>401</code></td><td>No token was used in the query.</td></tr><tr><td><code>Not Authorized - Invalid Token</code></td><td><code>401</code></td><td>Check the access token you used in the query.</td></tr><tr><td><code>Forbidden</code></td><td><code>403</code></td><td>There may be an issue with your account. Check your <a href="https://console.mapbox.com/">Account page</a> for more details.<br><br>In some cases, using an access tokens with URL restrictions can also result in a <code>403</code> error. For more information, see our <a href="https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions">Token management guide</a>.</td></tr><tr><td><code>Tileset {tileset name} does not exist</code></td><td><code>404</code></td><td>Check the name of the tileset you used in the query.</td></tr><tr><td><code>Tile not found</code></td><td><code>404</code></td><td>Check the column and row of the tile requested in the query.</td></tr><tr><td><code>Zoom level must be between 0-30.</code></td><td><code>422</code></td><td>The zoom level specified in the query is larger than 30 or contains non-numeric characters.</td></tr><tr><td><code>Tileset does not reference vector data</code></td><td><code>422</code></td><td>The tileset specified in the query is a raster tileset rather than a vector tileset. To retrieve raster tiles, use the <a href="/maps/raster-tiles/">Raster Tiles API</a>.</td></tr></tbody></table>

## Vector Tiles API restrictions and limits

-   The default rate limit for the Mapbox Vector Tiles API endpoint is 100,000 requests per minute. If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).
-   If you exceed the rate limit, you will receive an `HTTP 429 Too Many Requests` response. For information on rate limit headers, see the [Rate limit headers](https://docs.mapbox.com/api/guides/#rate-limit-headers) section.
-   Responses from the Vector Tiles API set default Cache-Control headers to `max-age=43200,s-maxage=300`, or a device cache TTL of 12 hours and a CDN cache TTL of 5 minutes. New tileset data is cached for up to a further 5 minutes on the backend as well.
-   If you composite multiple tileset IDs, the API request will have the cache time of the tileset with the highest `s-maxage` value.
-   For general information on caching, see the [Maps APIs caching dive deeper guide](https://docs.mapbox.com/help/dive-deeper/api-caching/).

## Vector Tiles API pricing

-   Billed by **requests**
-   See rates and discounts per tile requests in the pricing page's **[Maps](https://www.mapbox.com/pricing/#vectortile)** section

Usage of the Vector Tiles API is measured in **tile requests**. Details about the number of tile requests included in the free tier and the cost per request beyond what is included in the free tier are available on the [pricing page](https://www.mapbox.com/pricing/#vectortile).