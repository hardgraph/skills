# Tilequery API

The **Mapbox Tilequery API** retrieves data from `rasterarray` and `vector` tilesets using a location query (longitude, latitude). This allows for targeted data retrieval *without downloading full tiles*.

You can query:

-   [Mapbox tilesets](https://docs.mapbox.com/data/tilesets/reference/)
-   Custom tilesets created in [Mapbox Studio](https://studio.mapbox.com/)
-   Custom tilesets created via [Mapbox Tiling Service](https://docs.mapbox.com/api/maps/mapbox-tiling-service/)

The Tilequery API has slightly different functionality depending on the type of tileset you are querying.

-   For `vector` tilesets, you can query features within a radius, do point-in-polygon queries, and query multiple tilesets in the same request.
    
-   For `rasterarray` tilesets, you can query specific point values across different layers and bands
    

The Tilequery API does not support `raster` tilesets.

> **Related content (playground): [Tilequery API playground](https://docs.mapbox.com/playground/tilequery/)**
> 
> Create and run Tilequery API queries and see the results on a map.

## Retrieve features from a tileset

**GET** : `https://api.mapbox.com/v4/{tileset_id}/tilequery/{lon},{lat}.json`

Use this endpoint to retrieve features a tileset.

| Required parameters | Type | Description |
| --- | --- | --- |
| `tileset_id` | `string` | The ID of the map being queried. To query multiple tilesets at the same time, use a comma-separated list of up to 25 tileset IDs. |
| `{lon},{lat}` | `number` | The longitude and latitude to be queried. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |

You can further refine the results from this endpoint with the following optional parameters:

### Vector Specific Query Parameters

| Optional parameters | Type | Description |
| --- | --- | --- |
| `radius` | `number` | The approximate distance to query for features, in meters. Defaults to `0`, which does a point-in-polygon query. Has no upper bound. Required for queries against point and line data. Due to the nature of tile buffering, a query with a large radius made against equally large point or line data may not include all possible features in the results. Queries will use tiles from the maximum zoom of the tileset, and will only include the intersecting tile plus eight surrounding tiles when searching for nearby features. |
| `limit` | `integer` | The number of features between `1`-`50` to return. Defaults to `5`. The specified number of features are returned in order of their proximity to the queried `{lon},{lat}`. |
| `dedupe` | `boolean` | Determines whether the features in the result will be deduplicated (`true`, default) or not (`false`). The Tilequery API assumes that features are duplicates if all the following are true: the features are from the same layer; the features are the same geometry type; and the features have the same ID and the same properties (or the same properties, if the features do not have IDs). |
| `geometry` | `string` | Return only a specific geometry type. Options are `polygon`, `linestring`, or `point`. Defaults to returning all geometry types. |
| `layers` | `string` | A comma-separated list of layers to query, rather than querying all layers. If a specified layer does not exist, it is skipped. If no layers exist, returns an empty `FeatureCollection`. |

### Rasterarray Specific Query Parameters

Although the following parameters are listed as optional, if the `layers` parameter is not provided, the response will contain an empty `FeatureCollection` as a `rasterarray` tileset can contain many layers. If the `bands` parameter is not provided, the response will be limited to the first 50 bands in the `layers`.

| Optional parameters | Type | Description |
| --- | --- | --- |
| `bands` | `string` | A comma-separated list of bands to query, rather than querying all bands. If a specified band does not exist, it is skipped. If no bands exist, returns an empty `FeatureCollection`. |
| `layers` | `string` | A comma-separated list of layers to query, rather than querying all layers. If a specified layer does not exist, it is skipped. If no layers exist, returns an empty `FeatureCollection`. |

### Vector Point-in-polygon queries

To do a point-in-polygon query, do not use the `radius` parameter, which will leave it set at the default value of `0`. A point-in-polygon query will only return polygons that your query point is within. Any query point that exists directly along an edge of a polygon will not be returned.

Be aware of the number of results you are returning. There may be overlapping polygons in a tile, especially if you are querying multiple layers. If a query point exists within multiple polygons, there is no way to sort results such that they are returned in the order they were queried. If there are more results than you specified with the `limit` parameter, they will not be displayed after the query's `limit` is exceeded.

### Retrieve features from vector tilesets

#### Example requests

```bash
# Retrieve features within a 10 meter radius of the specified location

$ curl "https://api.mapbox.com/v4/mapbox.mapbox-streets-v8/tilequery/-122.42901,37.80633.json?radius=10&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Return at most 20 features

$ curl "https://api.mapbox.com/v4/mapbox.mapbox-streets-v8/tilequery/-122.42901,37.80633.json?limit=20&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Query multiple tilesets

$ curl "https://api.mapbox.com/v4/{tileset_id_1},{tileset_id_2},{tileset_id_3}/tilequery/-122.42901,37.80633.json?access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Return only results from the poi_label and building layers within a 30 meter radius of the specified location

$ curl "https://api.mapbox.com/v4/mapbox.mapbox-streets-v8/tilequery/-122.42901,37.80633.json?radius=30&layers=poi_label,building?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

#### Response format

A request to the Tilequery API for `vector` tilesets returns a GeoJSON `FeatureCollection` of features at or near the geographic point described by `{longitude},{latitude}` and within the distance described by the optional `radius` parameter.

The Tilequery API response is ordered based on which features are closest to the queried `{longitude},{latitude}`. If a point-in-polygon query was performed, then all returned features are directly at the query point and proximity does not influence the order in which they are returned.

Each feature in the response body contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `type` | `string` | This will always be `Feature`. |
| `id` | `string` | The identifier of the returned feature, as represented in the queried `vector` tile. |
| `geometry.type` | `string` | This will always be `Point`. The Tilequery API does not return the full geometry of a feature. Instead, it returns the closest point (`{longitude},{latitude}`) of a feature. |
| `geometry.coordinates` | `array` | The coordinates of the returned feature. |
| `properties` | `object` | The properties of the feature. |
| `properties.tilequery.distance` | `number` | The approximate surface distance from the feature result to the queried point, in meters. Note that this number is geographical distance, not distance along the road network. If `distance` is `0`, the query point is within the geometry (point-in-polygon). |
| `properties.tilequery.geometry` | `string` | The original geometry *type* of the feature. This can be `”point”`, `”linestring”`, or `”polygon”`. The actual geometry of the feature is not returned in the result set. The original geometry type of a feature impacts what gets returned:
| Original geometry of feature | Result |
| --- | --- |
| `polygon` | If the query point is *within* the polygon, the result will be the query point location. If the query point is *outside* the polygon, the result is the interpolated closest point along the nearest edge of the feature within the `radius` threshold. |
| `linestring` | The result is the interpolated closest point along the feature within the `radius` threshold. |
| `point` | The result is the nearest point within the `radius` threshold. |

 |
| `properties.tilequery.layer` | `string` | The `vector` tile layer of the feature result. |

#### Example response

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "id": 4174053671,
      "geometry": {
        "type": "Point",
        "coordinates": [-122.42901, 37.80633]
      },
      "properties": {
        "class": "national_park",
        "type": "national_park",
        "tilequery": {
          "distance": 0,
          "geometry": "polygon",
          "layer": "landuse_overlay"
        }
      }
    },
    {
      "type": "Feature",
      "id": 822070541,
      "geometry": {
        "type": "Point",
        "coordinates": [-122.4289919435978, 37.806283149631255]
      },
      "properties": {
        "localrank": 1,
        "maki": "park",
        "name": "Fort Mason",
        "name_ar": "Fort Mason",
        "name_de": "Fort Mason",
        "name_en": "Fort Mason Center",
        "name_es": "Fort Mason",
        "name_fr": "Fort Mason",
        "name_pt": "Fort Mason",
        "name_ru": "Fort Mason",
        "name_zh": "梅森堡",
        "name_zh-Hans": "梅森堡",
        "ref": "",
        "scalerank": 1,
        "type": "National Park",
        "tilequery": {
          "distance": 5.4376397785894595,
          "geometry": "point",
          "layer": "poi_label"
        }
      }
    }
  ]
}
```

### Retrieve features from rasterarray tilesets

#### Example requests

```bash
# Retrieve point specific features for multiple bands from a single layer
$ curl "https://api.mapbox.com/v4/{tileset_id}/tilequery/-122.42901,37.80633.json?bands=1663516800,1663520400&layers=temperature&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

```bash
# Query multiple tilesets
$ curl "https://api.mapbox.com/v4/{tileset_id_1},{tileset_id_2},{tileset_id_3}/tilequery/-122.42901,37.80633.json?bands=1663516800,1663520400&layers=temperature&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

#### Response format

A request to the Tilequery API for `rasterarray` tilesets returns a GeoJSON `FeatureCollection` of features at the geographic point described by `{longitude},{latitude}`.

The Tilequery API response is ordered by the `layer` and `bands` parameters that are used to filter the resulting `FeatureCollection`.

Each feature in the response body contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `type` | `string` | This will always be `Feature`. |
| `id` | `string` | This will always be `null` but exists to preserve the same response interface as the `vector` tilesets response. |
| `geometry.type` | `string` | This will always be `Point`. The Tilequery API returns the value of the point at the requested `{longitude},{latitude}` when used against `rasterarray` tilesets. |
| `geometry.coordinates` | `array` | The coordinates of the returned feature. |
| `properties` | `object` | The properties of the feature. |
| `properties.tilequery.layer` | `string` | The layer that the feature belongs to. |
| `properties.tilequery.band` | `string` | The band that the feature belongs to. |
| `properties.tilequery.zoom` | `number` | The maxzoom level of the tileset at which the point value was extracted from. |
| `properties.tilequery.units` | `string` | The unit of measurement for the point value. |
| `properties.val` | `array` | Point values at the requested `{longitude},{latitude}`. When requesting a point from a multi-dimensional tileset such as one representing a vector field, the array returned will contain 2 values representing the `u` and `v` components of the point. If the requested point falls within a location where no data exists, `null` will be returned instead of an array. |

#### Example response

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "id": null,
      "geometry": {
        "type": "Point",
        "coordinates": [133.3, 35.09]
      },
      "properties": {
        "tilequery": {
          "layer": "temperature",
          "band": "1663516800",
          "zoom": 5,
          "units": "C"
        },
        "val": 24
      }
    },
    {
      "type": "Feature",
      "id": null,
      "geometry": {
        "type": "Point",
        "coordinates": [133.3, 35.09]
      },
      "properties": {
        "tilequery": {
          "layer": "temperature",
          "band": "1663520400",
          "zoom": 5,
          "units": "C"
        },
        "val": 23.75
      }
    }
  ]
}
```

### Supported libraries

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Java SDK](https://docs.mapbox.com/android/java/api/libjava-services/5.8.0/com/mapbox/api/tilequery/package-frame.html)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#tilequery)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Tilequery API errors

<table><thead><tr><th>Response body <code>message</code></th><th>HTTP status code</th><th>Description</th></tr></thead><tbody><tr><td>Empty FeatureCollection</td><td><code>200</code></td><td>If an empty FeatureCollection is returned, check whether the specified layer exists.</td></tr><tr><td><code>Not Authorized - No Token</code></td><td><code>401</code></td><td>No token was used in the query.</td></tr><tr><td><code>Not Authorized - Invalid Token</code></td><td><code>401</code></td><td>Check the access token you used in the query.</td></tr><tr><td><code>Forbidden</code></td><td><code>403</code></td><td>There may be an issue with your account. Check your <a href="https://console.mapbox.com/">Account page</a> for more details.<br><br>In some cases, using an access tokens with URL restrictions can also result in a <code>403</code> error. For more information, see our <a href="https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions">Token management guide</a>.</td></tr><tr><td><code>Tileset {tileset name} does not exist</code></td><td><code>404</code></td><td>Check the name of the tileset you used in the query.</td></tr><tr><td><code>Tile does not exist</code></td><td><code>404</code></td><td>The tile that intersects the specified <code>{lon/lat}</code> location does not exist in the queried tileset. Or, if the tileset was <a href="/maps/mapbox-tiling-service/#create-a-tileset">created by MTS</a> but not successfully published, you will see this error.</td></tr><tr><td><code>options.limit must be a number between 1 and 50</code></td><td><code>422</code></td><td>The <code>limit</code> specified in the query is larger than 50 or contains non-numeric characters.</td></tr><tr><td><code>Invalid {lon/lat} value: {value}</code></td><td><code>422</code></td><td>Check the latitude and longitude values you used in the query.</td></tr><tr><td><code>options.geometry must be \"polygon\", \"linestring\", or \"point\"</code></td><td><code>422</code></td><td>The <code>geometry</code> parameter must be one of the options described in the <a href="/maps/tilequery/#vector-specific-query-parameters">Retrieve features from vector tiles</a> section of this documentation.</td></tr></tbody></table>

## Tilequery API restrictions and limits

-   The default rate limit for the Mapbox Tilequery API endpoint is 600 requests per minute. If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).
-   If you exceed the rate limit, you will receive an `HTTP 429 Too Many Requests` response. For information on rate limit headers, see the [Rate limit headers](https://docs.mapbox.com/api/guides/#rate-limit-headers) section.
-   Responses from the Tilequery API set default Cache-Control headers to `max-age=43200,s-maxage=300`, or a device cache TTL of 12 hours and a CDN cache TTL of 5 minutes.
-   If you composite multiple tileset IDs, the API request will have the cache time of the tileset with the highest `s-maxage` value.
-   For general information on caching, see the [Maps APIs caching dive deeper guide](https://docs.mapbox.com/help/dive-deeper/api-caching/).

## Tilequery API pricing

-   Billed by **requests**
-   See rates and discounts per Tilequery API request in the pricing page's **[Maps](https://www.mapbox.com/pricing/#tilequery)** section

Usage of the Tilequery API is measured in **API requests**. Each query for geographic features in one or more Mapbox-hosted tilesets at a given latitude and longitude will be counted as one Tilequery API request. Details about the number of Tilequery API requests included in the free tier and the cost per request beyond what is included in the free tier are available on the [pricing page](https://www.mapbox.com/pricing/#tilequery).