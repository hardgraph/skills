# Matrix API

The **Mapbox Matrix API** returns travel times between many points.

For example, given three locations A, B, and C, the Matrix API will return a matrix of all travel times in seconds between the locations:

|  | A | B | C |
| --- | --- | --- | --- |
| A | A → A | A → B | A → C |
| B | B → A | B → B | B → C |
| C | C → A | C → B | C → C |

The Matrix API will always return the duration or the distance on the fastest route for each element in the matrix, where an element is an origin-destination pair in the matrix. The Matrix API returns durations in seconds and distances in meters. It does not return route geometries.

Durations or distances between points may not be symmetric, since the routes may differ by direction due to one-way streets or turn restrictions. For example, A to B may have a different duration than B to A.

The Matrix API allows you to efficiently check the reachability of coordinates from each other, filter points by travel time, or run your own algorithms for solving optimization problems.

> **Related content (playground): [Matrix API Playground](https://docs.mapbox.com/playground/matrix/)**
> 
> Create a matrix, make API queries and visualize the response to the Matrix API.

## Retrieve a matrix

**GET** : `https://api.mapbox.com/directions-matrix/v1/{profile}/{coordinates}`

Returns a duration matrix, a distance matrix, or both, showing travel times and distances between coordinates.

In the default case, this endpoint returns a symmetric matrix that uses all the input coordinates as sources and destinations (N×N). Using the optional `sources` and `destination` parameters, you can also generate an asymmetric matrix that uses only some coordinates as sources or destinations: one to many (1×N), many to one (N×1), and several to several (M×N). To learn more about how to use these parameters to create each of these matrix types, experiment with the `sources` and `destinations` parameters in the [Matrix API Playground](https://docs.mapbox.com/playground/matrix/).

| Required parameters | Type | Description |
| --- | --- | --- |
| `profile` | `string` | A Mapbox Directions [routing profile](https://docs.mapbox.com/api/navigation/directions/#routing-profiles) ID.
| Profile ID | Description |
| --- | --- |
| `mapbox/driving` | Car travel times, distances, or both. |
| `mapbox/walking` | Pedestrian and hiking travel times, distances, or both |
| `mapbox/cycling` | Bicycle travel times, distances, or both |
| `mapbox/driving-traffic` | Car travel times, distances, or both as informed by traffic data |

 |
| `coordinates` | `number` | A semicolon-separated list of `{longitude},{latitude}` coordinates. There must be between two and 25 coordinates. For the `mapbox/driving-traffic` profile, the maximum is 10 coordinates. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `annotations` | `string` | Used to specify the resulting matrices. Possible values are: `duration` (default), `distance`, or both values separated by a comma. |
| `approaches` | `string` | A semicolon-separated list indicating the side of the road from which to approach waypoints in a requested route. Accepts `unrestricted` (default, route can arrive at the waypoint from either side of the road) or `curb` (route will arrive at the waypoint on the `driving_side` of the region). If provided, the number of approaches must be the same as the number of waypoints. But, you can skip a coordinate and show its position in the list with the `;` separator. |
| `bearings` | `string` | A semicolon-separated list of headings and allowed deviation indicating the direction of movement. Used to filter the road segment a waypoint will be placed on by the device's orientation. This is useful for making sure new routes of rerouted vehicles continue traveling in their desired direction. Input as two comma-separated values per location: a heading course measured clockwise from true north between 0 and 360, and the range of degrees by which the angle can deviate (recommended value is 45° or 90°), formatted as `{angle, degrees}`. If provided, the number of provided bearings must equal the number of locations; you can skip a coordinate and show its position in the list with the ';' separator. |
| `destinations` | `integer` or `string` | Use the coordinates at a given index as destinations. Possible values are: a semicolon-separated list of 0-based indices, or `all` (default). The option `all` allows using all coordinates as destinations. |
| `sources` | `integer` or `string` | Use the coordinates at a given index as sources. Possible values are: a semicolon-separated list of 0-based indices, or `all` (default). The option `all` allows using all coordinates as sources. |
| `fallback_speed`
LEGACY

 | `integer` | By default, if there is no possible route between two points, the Matrix API sets the resulting matrix element to `null`. To circumvent this behavior, set the `fallback_speed` parameter to a value greater than `0` in kilometers per hour. The Matrix API will replace a `null` value with a straight-line estimate between the source and destination based on the provided speed value. |
| `depart_at`

BETA

 | `string` | Specifies the desired time of departure to account for future traffic conditions and road restrictions; the matrix will be calculated based on a specified profile to determine conveyance type and desired or required traffic data. Formatted in one of three [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) formats: `YYYY-MM-DDThh:mm:ssZ`, `YYYY-MM-DDThh:mmss±hh:mm`, or `YYYY-MM-DDThh:mm`. In the last format, the timezone is calculated from the route origin. If left blank, depart_at will be calculated as the present time in the local timezone of the first coordinates. Set the `depart_at` property to the current time or some time in the future. It cannot be in the past. To use this feature, [sign up here](https://www.mapbox.com/contact/matrix-api-depart-at). |

Unrecognized options in the query string will result in an `InvalidInput` error.

### Example request: Retrieve a matrix

```bash
# Request a symmetric 3x3 matrix for cars with a curbside approach for each destination
$ curl "https://api.mapbox.com/directions-matrix/v1/mapbox/driving/-122.42,37.78;-122.45,37.91;-122.48,37.73?approaches=curb;curb;curb&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request a symmetric 3x3 matrix for cars to depart in 30 minutes, the specified time zone applied to the given coordinates
$ DEPARTURE_TIME=$(TZ=US/Pacific date -r $(($(date "+%s") + 30 * 60)) "+%Y-%m-%dT%H:%M")
$ curl "https://api.mapbox.com/directions-matrix/v1/mapbox/driving/-122.42,37.78;-122.45,37.91;-122.48,37.73?depart_at=$DEPARTURE_TIME&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request an asymmetric 2x3 matrix for bicycles
$ curl "https://api.mapbox.com/directions-matrix/v1/mapbox/cycling/-122.42,37.78;-122.45,37.91;-122.48,37.73?sources=0;2&destinations=all&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request a 1x3 matrix for walking that includes both duration and distance
$ curl "https://api.mapbox.com/directions-matrix/v1/mapbox/walking/-122.418563,37.751659;-122.422969,37.75529;-122.426904,37.759617?sources=1&annotations=distance,duration&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request a 3x2 asymmetric matrix with destinations for driving with bearings
$ curl "https://api.mapbox.com/directions-matrix/v1/mapbox/driving/-73.985738,40.757958;-74.043030,40.750292;-73.986440,40.713199?bearings=240.1,45;68.9,90;129,45&destinations=0;2&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Retrieve a matrix

The response to a Matrix API request is a JSON object that contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `code` | `string` | A string indicating the state of the response. This is a separate code than the HTTP status code. On normal valid responses, the value will be `Ok`. See the errors section below for more information. |
| `durations` | `array` | Durations as an array of arrays that represent the matrix in row-major order. `durations[i][j]` gives the travel time from the ith source to the jth destination. All values are in seconds. The duration between the same coordinate is always `0`. Finding no duration, the result will be `null`. |
| `distances` | `array` | Distances as an array of arrays that represent the matrix in row-major order. `distances[i][j]` gives the travel distance from the ith source to the jth destination. All values are in meters. The distance between the same coordinate is always `0`. Finding no distance, the result will be `null`. |
| `sources` | `array` | An `array` of [`waypoint` objects](https://docs.mapbox.com/api/navigation/directions/#waypoint-object). Each waypoint is an input coordinate snapped to the road and path network. The waypoints appear in the array in the order of the input coordinates, or in the order specified in the `sources` query parameter. |
| `destinations` | `array` | An `array` of [`waypoint` objects](https://docs.mapbox.com/api/navigation/directions/#waypoint-object). Each waypoint is an input coordinate snapped to the road and path network. The waypoints appear in the array in the order of the input coordinates, or in the order specified in the `destinations` query parameter. |

**Note:** When a route is not found between a source and a destination, the corresponding value in the `durations` or `distances` matrix will be `null`.

### Example response: Retrieve a matrix

```json
{
  "code": "Ok",
  "durations": [
    [0, 573, 1169.5],
    [573, 0, 597],
    [1169.5, 597, 0]
  ],
  "destinations": [
    {
      "name": "Mission Street",
      "location": [-122.418408, 37.751668],
      "distance": 5
    },
    {
      "name": "22nd Street",
      "location": [-122.422959, 37.755184],
      "distance": 8
    },
    {
      "name": "",
      "location": [-122.426911, 37.759695],
      "distance": 10
    }
  ],
  "sources": [
    {
      "name": "Mission Street",
      "location": [-122.418408, 37.751668],
      "distance": 5
    },
    {
      "name": "22nd Street",
      "location": [-122.422959, 37.755184],
      "distance": 8
    },
    {
      "name": "",
      "location": [-122.426911, 37.759695],
      "distance": 10
    }
  ]
}
```

### Supported libraries: Retrieve a matrix

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Java SDK](https://docs.mapbox.com/android/java/api/libjava-services/5.8.0/com/mapbox/api/matrix/v1/package-frame.html)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#getmatrix)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Matrix API errors

On error, the server responds with different HTTP status codes:

-   For responses with HTTP status codes lower than `500`, the JSON response body includes the `code` property, which may be used by client programs to manage control flow. The response body may also include a `message` property with a human-readable explanation of the error.
-   If a server error occurs, the HTTP status code will be `500` or higher and the response will not include a `code` property.

<table><thead><tr><th>Response body <code>code</code></th><th>HTTP status code</th><th>Description</th></tr></thead><tbody><tr><td><code>Ok</code></td><td><code>200</code></td><td>Normal success case.</td></tr><tr><td><code>NoRoute</code></td><td><code>200</code></td><td>There was no route found for the given coordinates. Check for impossible routes (for example, routes over oceans without ferry connections) or incorrectly formatted coordinates.</td></tr><tr><td>null</td><td><code>200</code></td><td>Request contains both routable and unroutable coordinate pairs; "null" is returned for unroutable coordinate pairs only.</td></tr><tr><td><code>Not Authorized - No Token</code></td><td><code>401</code></td><td>No token was used in the query.</td></tr><tr><td><code>Not Authorized - Invalid Token</code></td><td><code>401</code></td><td>Check the access token you used in the query.</td></tr><tr><td><code>Forbidden</code></td><td><code>403</code></td><td>There may be an issue with your account. Check your <a href="https://console.mapbox.com/">Account page</a> for more details.<br><br>In some cases, using an access tokens with URL restrictions can also result in a <code>403</code> error. For more information, see our <a href="https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions">Token management guide</a>.</td></tr><tr><td><code>ProfileNotFound</code></td><td><code>404</code></td><td>Use a valid profile as described in <a href="#retrieve-a-matrix">Retrieve a matrix</a>.</td></tr><tr><td><code>InvalidInput</code></td><td><code>422</code></td><td>The given request was not valid. The <code>message</code> key of the response will hold an explanation of the invalid input.</td></tr></tbody></table>

## Matrix API restrictions and limits

-   For the `mapbox/driving`, `mapbox/walking`, and `mapbox/cycling` profiles:
    -   Maximum 25 input coordinates per request
    -   Maximum 60 requests per minute
-   For the `mapbox/driving-traffic` profile:
    -   Maximum 10 input coordinates per request
    -   Maximum 30 requests per minute

If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).

## Matrix API pricing

-   Billed by **elements**
-   See rates and discounts per Matrix API element in the pricing page's **[Navigation](https://www.mapbox.com/pricing/#directionsmatrix)** section

The Matrix API handles bulk requests of widely varying sizes, so billing is tracked by the number of **[elements](https://docs.mapbox.com/help/glossary/matrix-api/)** returned instead of the number of requests made. For instance, one request from the Matrix API can be many different origin and destination pairs, so one request from the Matrix API is equal to *the number of sources* multiplied by *the number of destinations*:

-   The minimum number of elements per request is two (one source × two destinations, or two sources × one destination); requests with single-element results are not supported.
-   The maximum number of elements per request is 625 (25 sources × 25 destinations).

By default, the Matrix API returns a symmetric matrix, using all input coordinates as both sources and destinations. In this case, the number of elements is *the number of coordinates2*. You can also generate an asymmetric matrix, with only some coordinates as sources or destinations. In an asymmetric matrix, the number of elements per request is *the number of sources* multiplied by *the number of destinations*. The minimum number of elements to retrieve travel times and distances for is *two*.

Details about the number of Matrix API requests included in the free tier and the cost per request beyond what is included in the free tier are available on the [pricing page](https://www.mapbox.com/pricing/#directionsmatrix). For high volume use of the Mapbox Matrix API, [contact our sales team](https://www.mapbox.com/contact/sales/) for pricing.