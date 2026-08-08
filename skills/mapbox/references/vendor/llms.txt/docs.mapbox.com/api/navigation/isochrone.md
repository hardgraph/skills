# Isochrone API

An **isochrone**, from the Greek root words `iso` (equal) and `chrone` (time), is a line that connects points of equal travel time around a given location. The **Mapbox Isochrone API** computes areas that are reachable within a specified amount of time from a location, and returns the reachable regions as contours of polygons or lines that you can display on a map. This API also supports contours based on distance.

With the Isochrone API, you can:

-   Calculate isochrones up to 60 minutes using driving, cycling, or walking profiles
-   Define delivery or service zones based on travel times from a starting location
-   Illustrate travel times on a map with lines or polygons
-   Create [geofences](https://docs.mapbox.com/help/dive-deeper/geospatial-analysis/#using-spatial-analysis) to trigger location-aware user experiences, notifications, or events

> **Related content (playground): [Isochrone API playground](https://docs.mapbox.com/playground/isochrone/)**
> 
> Build an Isochrone API request and see the reachable areas from a location on a map.

> **Related content (tutorial): [Get started with the Isochrone API](https://docs.mapbox.com/help/tutorials/get-started-isochrone-api/)**
> 
> Use the Isochrone API to build a web app that helps users estimate how far they can travel by foot, bicycle, or car within a set amount of time.

## Retrieve isochrones around a location

**GET** : `https://api.mapbox.com/isochrone/v1/{profile}/{coordinates}?{contours_minutes|contours_meters}`

Given a location and a routing profile, retrieve up to four isochrone contours. The contours are calculated using rasters and are returned as either polygon or line features, depending on your input setting for the `polygons` parameter.

| Required parameters | Type | Description |
| --- | --- | --- |
| `profile` | `string` | A Mapbox Directions [routing profile](https://docs.mapbox.com/api/navigation/directions/#routing-profiles) ID. Options are:
| Profile ID | Description |
| --- | --- |
| `mapbox/driving-traffic` | For fastest travel by car using current and historic traffic conditions. Traffic information is available for the supported geographies listed in our [Traffic Data page](https://docs.mapbox.com/help/getting-started/directions/#traffic-data). |
| `mapbox/driving` | For fastest travel by car using average conditions. |
| `mapbox/walking` | For fastest travel by pedestrian and hiking travel paths. |
| `mapbox/cycling` | For fastest travel by bicycle. |

 |
| `coordinates` | `number` | A `{longitude,latitude}` coordinate pair around which to center the isochrone lines. |
| `contours_minutes` | `integer` | The times, in minutes, to use for each isochrone contour. You can specify up to four contours. Times must be in increasing order. The maximum time that can be specified is 60 minutes. |
| `contours_meters` | `integer` | The distances, in meters, to use for each isochrone contour. You can specify up to four contours. Distances must be in increasing order. The maximum distance that can be specified is 100000 meters (100km). |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `contours_colors` | `string` | The colors to use for each isochrone contour, specified as hex values without a leading `#` (for example, `ff0000` for red). If this parameter is used, there must be the same number of colors as there are entries in `contours_minutes` or `contours_meters`. If no colors are specified, the Isochrone API will assign a default rainbow color scheme to the output. |
| `polygons` | `boolean` | Specify whether to return the contours as GeoJSON polygons (`true`) or linestrings (`false`, default). When `polygons=true`, any contour that forms a ring is returned as a polygon. |
| `denoise` | `number` | A floating point value from `0.0` to `1.0` that can be used to remove smaller contours. The default is `1.0`. A value of `1.0` will only return the largest contour for a given value. A value of `0.5` drops any contours that are less than half the area of the largest contour in the set of contours for that same value. |
| `generalize` | `number` | A positive floating point value, in meters, used as the tolerance for [Douglas-Peucker generalization](https://en.wikipedia.org/wiki/Ramer%E2%80%93Douglas%E2%80%93Peucker_algorithm) specified for each contour. There is no upper bound. If no value is specified in the request, the Isochrone API will choose the most optimized generalization to use for the contours. If multiple contours are requested but only one generalize value is specified then this value is used for all contours. Note that the generalization of contours can lead to self-intersections, as well as intersections of adjacent contours. |
| `exclude` | `string` | Exclude certain road types and custom locations from routing. Default is to not exclude anything from the list below. You can specify multiple values as a comma-separated list. The following exclude values are available:
| Possible values | Description |
| --- | --- |
| `motorway` | Exclude highways or motorways. Available in `mapbox/driving` and `mapbox/driving-traffic` profiles. |
| `toll` | Exclude roads with tolls. Available in `mapbox/driving` and `mapbox/driving-traffic` profiles. |
| `ferry` | Exclude ferry rides across waterways. Available in `mapbox/driving` and `mapbox/driving-traffic` profiles. |
| `unpaved` | Exclude dirt roads or track. Available in `mapbox/driving` and `mapbox/driving-traffic` profiles. |
| `cash_only_tolls` | Exclude roads with tolls that accept only cash. Available in `mapbox/driving` and `mapbox/driving-traffic` profiles. |

 |
| `depart_at` | `string` | The departure time from the given coordinates. Formatted in one of three [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) formats: `YYYY-MM-DDThh:mm:ssZ`, `YYYY-MM-DDThh:mmss±hh:mm`, or `YYYY-MM-DDThh:mm`. In the last format, the timezone is calculated from the given coordinates. If not provided then `depart_at` is considered to be the present time in the local timezone of the coordinates. The isochrone contours will reflect traffic conditions at the time provided. |

### Example request: Retrieve isochrones around a location

```bash
$ curl "https://api.mapbox.com/isochrone/v1/mapbox/driving/-118.22258,33.99038?contours_minutes=5,10,15&contours_colors=6706ce,04e813,4286f4&polygons=true&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Retrieve isochrones around a location

In the response to a request to the Mapbox Isochrone API, the isochrone contours are returned as a [GeoJSON Feature Collection](https://tools.ietf.org/html/rfc7946). Each feature object in the Feature Collection is an isochrone, and contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `type` | `string` | `"Feature"`, a GeoJSON type from the [GeoJSON specification](https://tools.ietf.org/html/rfc7946). |
| `properties` | `object` | An object that describes the properties used for drawing the isochrone. |
| `properties.contour` | `integer` | the value of the metric used in this contour. See the `metric` property to determine whether this is a time or distance contour. |
| `properties.color` | `string` | The color of the isochrone line if the `geometry` property is `Linestring`. |
| `properties.opacity` | `number` | The opacity of the isochrone line if the `geometry` property is `Linestring`. |
| `properties.fill` | `string` | The fill color of the isochrone polygon if the `geometry` property is `Polygon`, suitable for use in [geojson.io](http://geojson.io/). |
| `properties.fill-opacity` | `number` | The fill opacity of the isochrone polygon if the `geometry` property is `Polygon`, suitable for use in [geojson.io](http://geojson.io/). |
| `properties.fillColor` | `string` | The fill color of the isochrone polygon if the `geometry` property is `Polygon`, suitable for use in [Leaflet](https://leafletjs.com/). |
| `properties.fillOpacity` | `number` | The fill opacity of the isochrone polygon if the `geometry` property is `Polygon`, suitable for use in [Leaflet](https://leafletjs.com/). |
| `properties.metric` | `string` | The metric that the contour is - either `distance` or `time`. |
| `geometry` | `object` | An object that describes the geometry of the isochrone. |
| `geometry.coordinates` | `array` | An array of arrays that contain the coordinates for the isochrone, formatted as `[longitude,latitude]`. |
| `geometry.type` | `string` | The GeoJSON type of the returned geometry. Either `"Linestring"` (default) or `"Polygon"` (if `polygons=true` was specified in the query). |

### Example response: Retrieve isochrones around a location

```json
{
  "features": [
    {
      "properties": {
        "contour": 15,
        "color": "#4286f4",
        "opacity": 0.33,
        "fill": "#4286f4",
        "fill-opacity": 0.33,
        "fillColor": "#4286f4",
        "fillOpacity": 0.33
      },
      "type": "Feature",
      "geometry": {
        "coordinates": ["arrays of longitude, latitude coordinates"],
        "type": "Polygon"
      }
    },
    {
      "properties": {
        "contour": 10,
        "color": "#04e813",
        "opacity": 0.33,
        "fill": "#04e813",
        "fill-opacity": 0.33,
        "fillColor": "#04e813",
        "fillOpacity": 0.33
      },
      "type": "Feature",
      "geometry": {
        "coordinates": ["arrays of longitude, latitude coordinates"],
        "type": "Polygon"
      }
    },
    {
      "properties": {
        "contour": 5,
        "color": "#6706ce",
        "opacity": 0.33,
        "fill": "#6706ce",
        "fill-opacity": 0.33,
        "fillColor": "#6706ce",
        "fillOpacity": 0.33
      },
      "type": "Feature",
      "geometry": {
        "coordinates": ["arrays of longitude, latitude coordinates"],
        "type": "Polygon"
      }
    }
  ],
  "type": "FeatureCollection"
}
```

### Supported libraries: Retrieve isochrones around a location

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox Ruby SDK](https://github.com/mapbox/mapbox-sdk-rb/blob/master/docs/isochrone.md)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Isochrone API errors

On error, the server responds with different HTTP status codes:

-   For responses with HTTP status codes lower than `500`, the JSON response body includes the code property, which may be used by client programs to manage control flow. The response body may also include a message property, with a human-readable explanation of the error.
-   If a server error occurs, the HTTP status code will be `500` or higher and the response will not include a `code` property.

<table><thead><tr><th>Response body <code>message</code></th><th>HTTP status code</th><th>Description</th></tr></thead><tbody><tr><td><code>Could not find a matching segment for input coordinates</code></td><td><code>200</code></td><td>No road segment could be matched for coordinates. Check for coordinates that are too far away from a road.</td></tr><tr><td><code>No route found</code></td><td><code>200</code></td><td>There was no route found for the given coordinates. Check for impossible routes (for example, routes over oceans without ferry connections).</td></tr><tr><td><code>Not Authorized - No Token</code></td><td><code>401</code></td><td>No token was used in the query.</td></tr><tr><td><code>Not Authorized - Invalid Token</code></td><td><code>401</code></td><td>Check the access token you used in the query.</td></tr><tr><td><code>Forbidden</code></td><td><code>403</code></td><td>There may be an issue with your account. Check your <a href="https://console.mapbox.com/">Account page</a> for more details.<br><br>In some cases, using an access tokens with URL restrictions can also result in a <code>403</code> error. For more information, see our <a href="https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions">Token management guide</a>.</td></tr><tr><td><code>Not Found</code></td><td><code>404</code></td><td>Check the <code>profile</code> you used in the query, which must be one of <code>mapbox/driving</code>, <code>mapbox/walking</code>, or <code>mapbox/cycling</code>.</td></tr><tr><td><code>Latitude must be between -90 and 90</code></td><td><code>422</code></td><td>Check the coordinates used in the query.</td></tr><tr><td><code>Should have one of the required properties 'contours_minutes' or 'contours_meters'</code></td><td><code>422</code></td><td>The query must include one of the <code>contours_minutes</code> or <code>contours_meters</code> parameters.</td></tr><tr><td><code>contours_minutes must be an integer between 1 and 60</code></td><td><code>422</code></td><td>The <code>contours_minutes</code> specified in the query is larger than 60 or contains non-numeric characters.</td></tr><tr><td><code>contours_meters must be an integer between 1 and 100000</code></td><td><code>422</code></td><td>The <code>contours_meters</code> specified in the query is larger than 100000 or contains non-numeric characters.</td></tr><tr><td><code>contours_colors should be a hexadecimal</code></td><td><code>422</code></td><td><code>contours_colors</code> must be specified as a hexadecimal color code.</td></tr><tr><td><code>You may only supply contours_minutes or contours_meters, not both</code></td><td><code>422</code></td><td>A single request may only contain one of <code>contours_minutes</code> or <code>contours_meters</code>.</td></tr></tbody></table>

## Isochrone API restrictions and limits

-   The Isochrone API is limited to 300 requests per minute.
-   The Isochrone API supports 1 coordinate per request.
-   The Isochrone API can support a maximum of 4 isochrone contours per request.
-   The maximum time that can be specified for an isochrone contour is 60 minutes.
-   The maximum distance that can be specified for an isochrone contour is 100000 meters.
-   Results must be displayed on a Mapbox map using one of the Mapbox [libraries or SDKs](https://www.mapbox.com/documentation/).

If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).

## Isochrone API pricing

-   Billed by **requests**
-   See rates and discounts per Isochrone API request in the pricing page's **[Navigation](https://www.mapbox.com/pricing/#isochrone)** section

Usage of the Isochrone API is measured in **API requests**. Details about the number of Isochrone API requests included in the free tier and the cost per request beyond what is included in the free tier are available on the [pricing page](https://www.mapbox.com/pricing/#isochrone).