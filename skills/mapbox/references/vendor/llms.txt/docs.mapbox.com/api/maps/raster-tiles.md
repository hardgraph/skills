# Raster Tiles API

The **Mapbox Raster Tiles API** serves [raster tiles](https://docs.mapbox.com/help/glossary/raster/) generated from satellite imagery tilesets and tilesets generated from raster data uploaded to Mapbox.com.

Tiles served from the Raster Tiles API can be consumed in any spatial application that supports image-based [XYZ tilesets](https://docs.mapbox.com/help/glossary/tileset/), including frontend mapping libraries like [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js/) and the Maps SDKs for [iOS](https://docs.mapbox.com/ios/maps/overview/) and [Android](https://docs.mapbox.com/android/maps/overview/), as well as GIS software like [QGIS](https://www.qgis.org/).

You can access [Mapbox raster tilesets](https://docs.mapbox.com/data/tilesets/reference/) including [Mapbox Satellite](https://docs.mapbox.com/data/tilesets/reference/mapbox-satellite/), or tilesets created from your own raster data. For information about creating raster tilesets from your own data, see the [Mapbox Tiling Service](https://docs.mapbox.com/mapbox-tiling-service/raster/) documentation.

## Retrieve raster tiles

**GET** : `https://api.mapbox.com/v4/{tileset_id}/{zoom}/{x}/{y}{@2x}.{format}`

<table><thead><tr><th>Required parameters</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>tileset_id</code></td><td><code>string</code></td><td>Unique identifier for the raster tileset in the format <code>username.id</code>. To composite multiple tilesets, use a comma-separated list of up to 15 tileset IDs.</td></tr><tr><td><code>zoom</code></td><td><code>integer</code></td><td>Specifies the tile's zoom level, as described in the <a href="http://wiki.openstreetmap.org/wiki/Slippy_map_tilenames">Slippy Map Tilenames</a> specification.</td></tr><tr><td><code>{x}/{y}</code></td><td><code>integer</code></td><td>Specifies the tile's column <code>{x}</code> and row <code>{y}</code>, as described in the <a href="http://wiki.openstreetmap.org/wiki/Slippy_map_tilenames">Slippy Map Tilenames</a> specification.</td></tr><tr><td><code>format</code></td><td><code>string</code></td><td>Specifies the format of the returned tiles:<table class="my12"><tbody><tr><td><code>.grid.json</code></td><td>UTFGrid</td></tr><tr><td><code>.png</code></td><td>True color PNG</td></tr><tr><td><code>.png32</code></td><td>32 color indexed PNG</td></tr><tr><td><code>.png64</code></td><td>64 color indexed PNG</td></tr><tr><td><code>.png128</code></td><td>128 color indexed PNG</td></tr><tr><td><code>.png256</code></td><td>256 color indexed PNG</td></tr><tr><td><code>.jpg</code></td><td>80% quality JPG</td></tr><tr><td><code>.jpg70</code></td><td>70% quality JPG</td></tr><tr><td><code>.jpg80</code></td><td>80% quality JPG</td></tr><tr><td><code>.jpg90</code></td><td>90% quality JPG</td></tr><tr><td><code>.webp</code></td><td>80% quality WebP</td></tr></tbody></table>The <code>format</code> of any image request can be replaced by any of these formats to adjust image quality for different bandwidth requirements. Higher-compression formats like <code>jpg70</code> or <code>png32</code> can be useful to favor performance over image quality.<br><br><strong>Note:</strong> Tiles that include <code>mapbox.satellite</code> are always delivered as JPEGs, even if the URL specifies PNG. The PNG format can't efficiently encode photographic images like those used by <code>mapbox.satellite</code>.<br><br><strong>Note</strong>: Some tilesets may display a black background in older browser versions but not in newer browser versions. This is because many modern browsers support the <a href="https://developers.google.com/speed/webp/">WebP</a> image format. For more information see <a href="https://docs.mapbox.com/help/troubleshooting/raster-transparency-issues/">Troubleshoot raster image with black background</a>.</td></tr><tr><td><code>access_token</code></td><td><code>string</code></td><td>A valid Mapbox <a href="/guides/#access-tokens-and-token-scopes">access token</a>.</td></tr></tbody></table>

| Optional parameters | Type | Description |
| --- | --- | --- |
| `@2x` | `string` | Request a higher DPI version of the image. |

### Example request: Retrieve raster tiles

```bash
# Retrieve a 2x tile; this 512x512 tile is appropriate for high-density displays

$ curl "https://api.mapbox.com/v4/mapbox.satellite/1/0/0@2x.jpg90?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Retrieve raster tiles

The response is a raster image tile in the specified format. For performance, image tiles are delivered with a `max-age` header value set 12 hours in the future.

## Raster Tiles API errors

<table><thead><tr><th>Response body <code>message</code></th><th>HTTP status code</th><th>Description</th></tr></thead><tbody><tr><td><code>Not Authorized - No Token</code></td><td><code>401</code></td><td>No token was used in the query.</td></tr><tr><td><code>Not Authorized - Invalid Token</code></td><td><code>401</code></td><td>Check the access token you used in the query.</td></tr><tr><td><code>Forbidden</code></td><td><code>403</code></td><td>There may be an issue with your account. Check your <a href="https://console.mapbox.com/">Account page</a> for more details.<br><br>In some cases, using an access tokens with URL restrictions can also result in a <code>403</code> error. For more information, see our <a href="https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions">Token management guide</a>.</td></tr><tr><td><code>Tileset {tileset name} does not exist</code></td><td><code>404</code></td><td>Check the name of the tileset you used in the query.</td></tr><tr><td><code>Tile not found</code></td><td><code>404</code></td><td>Check the column and row of the tile requested in the query.</td></tr><tr><td><code>Tileset does not contain any tiles</code></td><td><code>404</code></td><td>The tileset does not yet contain any tiles. It may not have been published or is still being processed.</td></tr><tr><td><code>Zoom level must be between 0-30.</code></td><td><code>422</code></td><td>The zoom level specified in the query is larger than 30 or contains non-numeric characters.</td></tr><tr><td><code>Invalid quality value {value} for raster format {jpg/png}</code></td><td><code>422</code></td><td>The <code>format</code> specified in the query must be one of the formats listed in the <a href="/maps/raster-tiles/#retrieve-raster-tiles">Retrieve raster tiles</a> section of this documentation.</td></tr></tbody></table>

## Raster Tiles API restrictions and limits

-   The default rate limit for the Mapbox Raster Tiles API endpoint is 100,000 requests per minute. If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).
-   If you exceed the rate limit, you will receive an `HTTP 429 Too Many Requests` response. For information on rate limit headers, see the [Rate limit headers](https://docs.mapbox.com/api/guides/#rate-limit-headers) section.
-   Responses from the Raster Tiles API set default Cache-Control headers to `max-age=43200,s-maxage=300`, or a device cache TTL of 12 hours and a CDN cache TTL of 5 minutes. New tileset data is cached for up to a further 5 minutes on the backend as well.
-   If you composite multiple tileset IDs, the API request will have the cache time of the tileset with the highest `s-maxage` value.
-   For general information on caching, see the [Maps APIs caching dive deeper guide](https://docs.mapbox.com/help/dive-deeper/api-caching/).

## Raster Tiles API pricing

-   Billed by **requests**
-   See rates and discounts per tile requests in the pricing page's **[Maps](https://www.mapbox.com/pricing/#tile)** section

Usage of the Raster Tiles API is measured in **tile requests**. Details about the number of tile requests included in the free tier and the cost per request beyond what is included in the free tier are available on the [pricing page](https://www.mapbox.com/pricing/#tile).