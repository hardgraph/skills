# Map Matching API

> **Note: Map Matching API version**
> 
> This documentation is for `v5` of the Map Matching API. For information about the earlier version, see the [`v4` documentation](https://docs.mapbox.com/api/legacy/map-matching-v4/) or [view the changelog](https://docs.mapbox.com/api/navigation/changelog/#map-matching-api).

The **Mapbox Map Matching API** snaps fuzzy, inaccurate traces from a GPS unit or a phone to the road and path network using the Directions API. This produces clean paths that can be displayed on a map or used for other analysis.

> **Note: Turn-by-turn directions in the Map Matching API**
> 
> The Map Matching API can also return a full directions response to queries using the optional `steps` parameter. If you plan use the Map Matching API to return turn-by-turn directions, note that it does not consider all road rules and traffic conditions. To access up-to-date traffic and road conditions for navigation purposes, use the [Mapbox Directions API](https://docs.mapbox.com/api/navigation/directions/).

> **Related content (tutorial): [Get started with the Map Matching API](https://docs.mapbox.com/help/tutorials/get-started-map-matching-api/)**
> 
> Create a web app that uses the Map Matching API to allow users to specify their own driving route.

## Retrieve a match

**GET** : `https://api.mapbox.com/matching/v5/{profile}/{coordinates}.json`

Return a path on the road and path network that is closest to the input traces.

<table><thead><tr><th>Required parameters</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>profile</code></td><td><code>string</code></td><td>A Mapbox Directions <a href="/navigation/directions/#routing-profiles">routing profile</a> ID.<table class="mt12"><tbody><tr><th>Profile ID</th><th>Description</th></tr><tr><td><code>mapbox/driving</code></td><td>Car travel times, distances, or both.</td></tr><tr><td><code>mapbox/walking</code></td><td>Pedestrian and hiking travel times, distances, or both</td></tr><tr><td><code>mapbox/cycling</code></td><td>Bicycle travel times, distances, or both</td></tr><tr><td><code>mapbox/driving-traffic</code></td><td>Car travel times, distances, or both as informed by traffic data</td></tr></tbody></table></td></tr><tr><td><code>coordinates</code></td><td><code>number</code> or <code>string</code></td><td>A semicolon-separated list of <code>{longitude},{latitude}</code> coordinate pairs to visit in order.<br>Or OpenLR encoded string where <code>openlr_spec</code> describes the used specification and <code>openlr_format</code> describes the binary format. OpenLR strings are useful when using routes built on platforms outside of Mapbox.<br><em>If specified as coordinate pairs there can be between <code>2</code> and <code>100</code> coordinates.</em><br><em>If specified as OpenLR string length, the OpenLR string can be not greater than 500 (which is 50 coordinates)</em></td></tr><tr><td><code>access_token</code></td><td><code>string</code></td><td>A valid Mapbox <a href="/guides/#access-tokens-and-token-scopes">access token</a>.</td></tr></tbody></table>

You can further refine the results from this endpoint with the following optional parameters:

<table><thead><tr><th>Optional parameters</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>annotations</code></td><td><code>string</code></td><td>Return additional metadata along the route. You can include several annotations as a comma-separated list. <strong>Must be used in combination with <code>overview=full</code>.</strong><table class="mt12"><tbody><tr><th>Possible values</th><th>Description</th></tr><tr><td><code>distance</code></td><td>The distance between each pair of coordinates, in meters.</td></tr><tr><td><code>duration</code></td><td>The duration between each pair of coordinates, in seconds.</td></tr><tr><td><code>speed</code></td><td>The speed between each pair of coordinates, in meters per second.</td></tr><tr><td><code>congestion</code></td><td>The level of congestion between each entry in the array of coordinate pairs in the route leg. This annotation is only available for the <code>mapbox/driving-traffic</code> profile.</td></tr><tr><td><code>congestion_numeric</code></td><td>The numeric level of congestion between each entry in the array of coordinate pairs in the route leg. This annotation is only available for the <code>mapbox/driving-traffic</code> profile.</td></tr><tr><td><code>maxspeed</code><div style="padding-top:1px;letter-spacing:0.07em" class="txt-fancy-medium round inline-block cursor-default color-gray-dark bg-blue-lighter color-blue-dark txt-s px6" data-state="closed">BETA</div></td><td>The maximum speed limit between the coordinates of a segment. This annotation is only available for the <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code> profiles.</td></tr></tbody></table>See the <a href="/navigation/directions/#route-leg-object">route leg object</a> for more details on what is included with annotations.</td></tr><tr><td><code>approaches</code></td><td><code>string</code></td><td>A semicolon-separated list indicating the side of the road from which to approach waypoints in a requested route. Accepts <code>unrestricted</code> (default, route can arrive at the waypoint from either side of the road) or <code>curb</code> (route will arrive at the waypoint on the <code>driving_side</code> of the region). If provided, the number of approaches must be the same as the number of waypoints. But, you can skip a coordinate and show its position in the list with the <code>;</code> separator. If <code>waypoints</code> is not specified (so all coordinates are treated as waypoints), the list of approaches must be the same length as the list of coordinates. Must be used in combination with <code>steps=true</code>.</td></tr><tr><td><code>geometries</code></td><td><code>string</code></td><td>The format of the returned geometry. Allowed values are: <code>geojson</code> (as <a href="https://tools.ietf.org/html/rfc7946#appendix-A.2">LineString</a>), <a href="https://developers.google.com/maps/documentation/utilities/polylinealgorithm"><code>polyline</code></a> (default, a polyline with precision 5), and <a href="https://developers.google.com/maps/documentation/utilities/polylinealgorithm"><code>polyline6</code></a> (a polyline with precision 6).</td></tr><tr><td><code>overview</code></td><td><code>string</code></td><td>The type of returned overview geometry. Can be <code>full</code> (the most detailed geometry available), <code>simplified</code> (default, a simplified version of the full geometry), or <code>false</code> (no overview geometry).</td></tr><tr><td><code>radiuses</code></td><td><code>number</code></td><td>A semicolon-separated list indicating the maximum distance a coordinate can be moved to snap to the road network, in meters. If provided, the number of radiuses must be the same as the number of coordinates. But, you can skip a coordinate and show its position in the list with the <code>;</code> separator. Values can be a number between <code>0.0</code> and <code>50.00</code>. Use higher numbers (<code>20</code>-<code>50</code>) for noisy traces and lower numbers (<code>1</code>-<code>10</code>) for clean traces. The default value is <code>5</code>. A <code>NoSegment</code> error is returned if no routable road is located within the radius.</td></tr><tr><td><code>steps</code></td><td><code>boolean</code></td><td>Whether to return steps and turn-by-turn instructions (<code>true</code>) or not (<code>false</code>, default).<br><br>Setting <code>steps</code> to true will make the following guidance-related parameters available: <code>banner_instructions</code>, <code>language</code>, <code>roundabout_exits</code>, <code>voice_instructions</code>, <code>voice_units</code>, <code>waypoint_names</code>, and <code>waypoints</code>.</td></tr><tr><td><code>banner_instructions</code></td><td><code>boolean</code></td><td>Whether to return banner objects associated with the route steps (<code>true</code>) or not (<code>false</code>, default). <strong>Must be used in conjunction with <code>steps=true</code>.</strong></td></tr><tr><td><code>language</code></td><td><code>string</code></td><td>The language of returned turn-by-turn text instructions. See <a href="/navigation/directions/#instructions-languages">supported languages</a>. The default is <code>en</code> (English). <strong>Must be used in conjunction with <code>steps=true</code>.</strong></td></tr><tr><td><code>roundabout_exits</code></td><td><code>boolean</code></td><td>Whether to emit instructions at roundabout exits (<code>true</code>) or not (<code>false</code>, default). Without this parameter, roundabout maneuvers are a single instruction that includes both entering and exiting the roundabout. With <code>roundabout_exits=true</code>, this maneuver becomes two instructions, one for entering the roundabout and one for exiting it. <strong>Must be used in conjunction with <code>steps=true</code>.</strong></td></tr><tr><td><code>voice_instructions</code></td><td><code>boolean</code></td><td>Whether to return <a href="https://developer.amazon.com/docs/custom-skills/speech-synthesis-markup-language-ssml-reference.html">SSML</a> marked-up text for voice guidance along the route (<code>true</code>) or not (<code>false</code>, default). <strong>Must be used in conjunction with <code>steps=true</code>.</strong></td></tr><tr><td><code>voice_units</code></td><td><code>string</code></td><td>Specify which type of units to return in the text for voice instructions. Can be <code>imperial</code> (default), <code>british_imperial</code> or <code>metric</code>. <strong>Must be used in conjunction with <code>steps=true</code> and <code>voice_instructions=true</code>.</strong></td></tr><tr><td><code>tidy</code></td><td><code>boolean</code></td><td>Whether to remove clusters and re-samples traces for improved map matching results (<code>true</code>) or not (<code>false</code>, default).</td></tr><tr><td><code>timestamps</code></td><td><code>number</code></td><td>A semicolon-separated list of numbers in <a href="https://en.wikipedia.org/wiki/Unix_time">Unix time</a> (in other words, seconds since 1/1/1970 UTC) that correspond to each input coordinate. If provided, the number of timestamps must be the same as the number of coordinates, no coordinates can be skipped, and the timestamps must occur in ascending order. For best results, timestamps should have a sample rate of about 5 seconds.</td></tr><tr><td><code>waypoint_names</code></td><td><code>string</code></td><td>A semicolon-separated list of custom names for waypoints. These names will be used for the arrival instruction in banners and voice instructions. Values can be any string, and the total number of all characters (including semicolons) cannot exceed <code>500</code>. The list of <code>waypoint_names</code> must be the same length as the list of waypoints, but you can skip a waypoint and show its position with the <code>;</code> separator. If <code>waypoints</code> is not specified (so all coordinates are treated as waypoints), the list of <code>waypoint_names</code> must be the same length as the list of coordinates.</td></tr><tr><td><code>waypoints</code></td><td><code>integer</code></td><td>A semicolon-separated list indicating which input coordinates should be treated as waypoints. If a coordinate is treated as a waypoint, it receives arrival and departure events in the <a href="#match-object">match object's</a> route. If a list of waypoints is not provided, all coordinates are treated as waypoints. Each item in the list must be the zero-based index of an input coordinate, and the list must include <code>0</code> (the index of the first coordinate) and the index of the last coordinate. Waypoints are most useful in combination with <code>steps=true</code> and requests based on traces with high sample rates.</td></tr><tr><td><code>ignore</code></td><td><code>string</code></td><td>Ignore certain routing restrictions when map matching. You can include several <code>ignore</code> options as a comma-separated list (for example, <code>ignore=access,oneways,restrictions</code>).<table class="mt12"><tbody><tr><th>Possible values</th><th>Description</th></tr><tr><td><code>access</code></td><td>Ignore access restrictions related to mode of travel.</td></tr><tr><td><code>oneways</code></td><td>Ignore one-way restrictions.</td></tr><tr><td><code>restrictions</code></td><td>Ignore other restrictions, such as time-based or turn restrictions.</td></tr></tbody></table>This option is only available for the <code>mapbox/driving</code> profile.</td></tr><tr><td><code>linear_references</code></td><td><code>boolean</code></td><td>Returns map-agnostic location identifiers of the roads along a route. When <code>true</code>, a successful response will include a key <code>linear_references</code>, the value of which is an array of base64-encoded <a href="https://download.tomtom.com/open/banners/openlr-whitepaper_v1.5.pdf">OpenLR location references</a>, matched by the input trace. This option is only available for <code>driving</code> and <code>driving-traffic</code> profile.</td></tr><tr><td><code>openlr_spec</code></td><td><code>string</code></td><td>The logical format for OpenLR encoded <code>coordinates</code> translates to how OpenLR attributes are being interpreted. Some data providers may use a different logical data format, as in the same attributes (FRC, FOW) may be interpreted differently.<table class="mt12"><tbody><tr><th>Possible values</th><th>Description</th></tr><tr><td><code>tomtom</code></td><td>Based on <a href="https://download.tomtom.com/open/banners/openlr-whitepaper_v1.5.pdf"><code>TomTom</code> OpenLR location references,</a>. Read p. 31-40 to learn more about the specification.</td></tr><tr><td><code>here</code></td><td>Based on <a href="https://www.iso.org/obp/ui/#iso:std:iso:ts:21219:-22:ed-1:v1:en">HERE/TPEG2 specification</a></td></tr></tbody></table>The default is <code>tomtom</code>.<br>This option is only available if <code>coordinates</code> are provided as OpenLR encoded string.</td></tr><tr><td><code>openlr_format</code></td><td><code>string</code></td><td>Binary format for OpenLR encoded <code>coordinates</code> translates to how an OpenLR input is encoded.<table class="mt12"><tbody><tr><th>Possible values</th><th>Description</th></tr><tr><td><code>tomtom</code></td><td>Based on <a href="https://download.tomtom.com/open/banners/openlr-whitepaper_v1.5.pdf"><code>TomTom</code> OpenLR location references,</a>. Read p. 43-52 to learn more about the binary format.</td></tr></tbody></table>The default is <code>tomtom</code>.<br>This option is only available if <code>coordinates</code> are provided as OpenLR encoded string.</td></tr><tr><td><code>depart_at</code></td><td><code>string</code></td><td>The departure time from the first coordinates, formatted in one of three <a href="https://en.wikipedia.org/wiki/ISO_8601">ISO 8601</a> formats: <code>YYYY-MM-DDThh:mm:ssZ</code>, <code>YYYY-MM-DDThh:mmss±hh:mm</code>, or <code>YYYY-MM-DDThh:mm</code>. In the last format, the timezone is calculated from the first coordinates. If not provided then <code>depart_at</code> is considered to be the present time in the local timezone of the first coordinates. The map-matched route and duration will reflect traffic conditions based on the <code>depart_at</code> time.</td></tr></tbody></table>

Some processing tips to achieve the best results:

-   Timestamps improve the quality of the matching and are highly recommended.
-   The Map Matching API is limited to processing traces with up to 100 coordinates. If you need to process longer traces, you can split the trace and make multiple requests.
-   Clusters of points (like a person waiting at a train crossing for a few minutes) often don't add more information to a trace and can negatively impact map-matching quality. We recommend that you tidy the trace (remove clusters and provide a uniform sample rate). You can use the `tidy=true` query parameter or process your traces with external tools like [geojson-tidy](https://github.com/mapbox/geojson-tidy).
-   Map matching works best with a sample rate of 5 seconds between points. If your trace has a higher sample rate, you may want to downsample your trace.
-   With the `waypoints` parameter specified, traces that would normally return with sub-matches will error. We recommend tidying traces before using them with the `waypoints` parameter.

### Example request: Retrieve a match

```bash
# Basic request that returns a match object with route legs between each waypoint

$ curl "https://api.mapbox.com/matching/v5/mapbox/driving/-117.17282,32.71204;-117.17288,32.71225;-117.17293,32.71244;-117.17292,32.71256;-117.17298,32.712603;-117.17314,32.71259;-117.17334,32.71254?access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request to access speed limit information using the maxspeed annotation

$ curl "https://api.mapbox.com/matching/v5/mapbox/driving/-122.39636,37.79129;-122.39732,37.79283;-122.39606,37.79349?annotations=maxspeed&overview=full&geometries=geojson&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request with the approaches parameter set to 'curb' for each waypoint

$ curl "https://api.mapbox.com/matching/v5/mapbox/driving/-117.17282,32.71204;-117.17288,32.71225;-117.17293,32.71244;-117.17292,32.71256?approaches=curb;curb;curb;curb&access_token=YOUR_MAPBOX_ACCESS_TOKEN"


# Request with various parameters, returns a match object with one route leg between the first and last waypoints

$ curl "https://api.mapbox.com/matching/v5/mapbox/driving/2.344003,48.85805;2.34675,48.85727;2.34868,48.85936;2.34955,48.86084;2.34955,48.86088;2.34962,48.86102;2.34982,48.86125?steps=true&tidy=true&waypoints=0;6&waypoint_names=Home;Work&banner_instructions=true&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request with openlr encoded string as coordinates used `tomtom` specification, return the same response the same as for regular coordinates

$ curl "https://api.mapbox.com/matching/v5/mapbox/driving/CwOiYCUMoBNWAv9P%2F%2BMSBg%3D%3D?openlr_spec=tomtom&openlr_format=tomtom&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request with openlr encoded string as coordinates used `here` specification

$ curl "https://api.mapbox.com/matching/v5/mapbox/driving/Cwe2%2BiJmURNhMPlvCBAbbAAA?openlr_spec=here&access_token=YOUR_MAPBOX_ACCESS_TOKEN"


```

> **Note: URL encoding of OpenLR references**
> 
> When submitting API requests containing OpenLR references, you must URL encode the reference before passing it in as a parameter. OpenLR references contain unsafe ASCII characters like `/`, `+`, and `=` which must be encoded to produce a valid URL. See [W3Schools' page on URL encoding](https://www.w3schools.com/tags/ref_urlencode.ASP) for help with encoding unsafe ASCII characters.

### Supported libraries: Retrieve a match

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Directions for Swift](https://github.com/mapbox/mapbox-directions-swift/#matching-a-trace-to-the-road-network)
-   [Mapbox Java SDK](https://docs.mapbox.com/android/java/api/libjava-services/5.8.0/com/mapbox/api/matching/v5/package-frame.html)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#getmatch)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

### Response: Retrieve a match

The **match response object** contains one or more [match objects](#match-object), as well as one or more [tracepoint objects](#tracepoint-object).

| Property | Type | Description |
| --- | --- | --- |
| `code` | `string` | A string indicating the state of the response. The potential values are listed in the [Map Matching status codes section](#map-matching-api-errors). |
| `matchings` | `array` | An array of [match objects](#match-object). |
| `tracepoints` | `array` | An array of [tracepoint objects](#tracepoint-object) that represent the location an input point was matched with, in the order in which they were matched. If a trace point is omitted by the Map Matching API because it is an outlier, the entry will be `null`. |

With clean matches, only one match object is returned. When the algorithm cannot decide the correct match between two points, it will omit that line and return several sub-matches as match objects. The higher the number of sub-match match objects, the more likely it is that the input traces are poorly aligned to the road network.

#### Example response: Retrieve a match

```json
{
  "matchings": [
    {
      "confidence": 4.615758886217236e-10,
      "geometry": {
        "coordinates": [
          [-122.397484, 37.792809],
          [-122.39746, 37.792693],
          [-122.39745, 37.792645],
          [-122.397437, 37.792586],
          [-122.397431, 37.792558],
          [-122.39741, 37.792466],
          [-122.397404, 37.79244],
          [-122.397225, 37.792579],
          [-122.396623, 37.793057],
          [-122.39636, 37.793276],
          [-122.396075, 37.793502]
        ],
        "type": "LineString"
      },
      "legs": [
        {
          "annotation": {
            "maxspeed": [
              {
                "speed": 48,
                "unit": "km/h"
              },
              {
                "speed": 48,
                "unit": "km/h"
              },
              {
                "speed": 48,
                "unit": "km/h"
              },
              {
                "speed": 48,
                "unit": "km/h"
              },
              {
                "speed": 48,
                "unit": "km/h"
              },
              {
                "speed": 48,
                "unit": "km/h"
              },
              {
                "unknown": true
              },
              {
                "unknown": true
              },
              {
                "unknown": true
              },
              {
                "unknown": true
              }
            ]
          },
          "summary": "",
          "weight": 153.6,
          "duration": 73.6,
          "steps": [],
          "distance": 207.8
        }
      ],
      "weight_name": "routability",
      "weight": 153.6,
      "duration": 73.6,
      "distance": 207.8
    }
  ],
  "tracepoints": [
    null,
    {
      "alternatives_count": 0,
      "waypoint_index": 0,
      "matchings_index": 0,
      "distance": 14.635568381812668,
      "name": "Davis Street",
      "location": [-122.397484, 37.792809],
      "maxspeed": {
        "speed": 48,
        "unit": "km/h"
      }
    },
    {
      "alternatives_count": 1,
      "waypoint_index": 1,
      "matchings_index": 0,
      "distance": 1.8762601659793365,
      "name": "Market Street",
      "location": [-122.396075, 37.793502],
      "maxspeed": {
        "unknown": true
      }
    }
  ],
  "code": "Ok"
}
```

## Use HTTP POST to retrieve a match

The Map Matching API also supports access using the HTTP `POST` method. HTTP `POST` should be used for large requests, since the Map Matching API has a size limit of approximately 8100 bytes on `GET` request URLs. `POST` requests are still subject to your account's request size limits.

Learn more about this process in the [Using HTTP POST](https://docs.mapbox.com/api/navigation/http-post/) section.

## Match object

A **match object** is a [route object](https://docs.mapbox.com/api/navigation/directions/#route-object) with an additional confidence field:

<table><thead><tr><th>Property</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>confidence</code></td><td><code>number</code></td><td>The level of confidence in the returned match, from <code>0</code> (low) to <code>1</code> (high).</td></tr><tr><td><code>distance</code></td><td><code>number</code></td><td>The distance traveled, in meters.</td></tr><tr><td><code>duration</code></td><td><code>number</code></td><td>The estimated travel time, in seconds.</td></tr><tr><td><code>weight</code></td><td><code>number</code></td><td>The weight in units described by <code>weight_name</code>.</td></tr><tr><td><code>weight_name</code></td><td><code>string</code></td><td>The weight used. The default is <code>routability</code>, which is duration-based, with additional penalties for less desirable maneuvers.</td></tr><tr><td><code>geometry</code></td><td><code>string</code></td><td>Depending on the <code>geometries</code> parameter in the request, this is a <a href="https://tools.ietf.org/html/rfc7946#appendix-A.2">GeoJSON LineString</a> or a <a href="https://developers.google.com/maps/documentation/utilities/polylinealgorithm">Polyline string</a>. Depending on the <code>overview</code> parameter in the request, this is the complete route geometry (<code>full</code>), a simplified geometry to the zoom level at which the route can be displayed in full (<code>simplified</code>), or is not included (<code>false</code>).</td></tr><tr><td><code>legs</code></td><td><code>array</code></td><td>An array of <a href="/navigation/directions/#route-leg-object">route leg objects</a>.</td></tr><tr><td><code>voice_locale</code><br><br>(Requires <code>steps=true</code>)</td><td><code>string</code></td><td>The locale used for voice instructions. Defaults to <code>en</code> (English). See <a href="/navigation/directions/#instructions-languages">supported languages</a>.</td></tr><tr><td><code>linear_references</code></td><td><code>array</code></td><td>An array of base64-encoded <a href="https://download.tomtom.com/open/banners/openlr-whitepaper_v1.5.pdf">OpenLR location references</a>, one for each graph edge of the road network matched by the input trace. This key is optional, and present only when <code>linear_references=true</code> in the request.</td></tr></tbody></table>

### Example match object

```json
{
  "confidence": 0.9548844020537051,
  "distance": 103.7,
  "duration": 16.4,
  "geometry": "gatfEfidjUi@Le@@Y?E??J?^Hf@",
  "legs": []
}
```

## Tracepoint object

A **tracepoint object** is a [waypoint object](https://docs.mapbox.com/api/navigation/directions/#waypoint-object) with three additional fields: `matchings_index`, `waypoint_index`, and `alternatives_count`.

| Property | Type | Description |
| --- | --- | --- |
| `matchings_index` | `integer` | The index of the match object in `matchings` that the sub-trace was matched to. |
| `waypoint_index` | `integer` or `null` | The index of the waypoint inside the matched route. May be null when the `waypoints` parameter is used and this tracepoint does not correspond with specified waypoints. |
| `alternatives_count` | `integer` | The number of probable alternative matchings for this trace point. A value of `0` indicates that this point was matched unambiguously. Split the trace at these points for incremental map matching. |
| `name` | `string` | The name of the road or path the coordinate snapped to. |
| `location` | `array` | An array that contains the location of the snapped coordinate, in the format `[longitude, latitude]`. |

### Example tracepoint object

```json
{
  "waypoint_index": 0,
  "location": [-117.172836, 32.71204],
  "name": "North Harbor Drive",
  "matchings_index": 0,
  "alternatives_count": 0
}
```

## Map Matching API errors

On error, the server responds with different HTTP status codes:

-   For responses with HTTP status codes lower than `500`, the JSON response body includes the code property, which may be used by client programs to manage control flow. The response body may also include a message property, with a human-readable explanation of the error.
-   If a server error occurs, the HTTP status code will be `500` or higher and the response will not include a `code` property.

<table><thead><tr><th>Response body <code>code</code></th><th>HTTP status code</th><th>Description</th></tr></thead><tbody><tr><td><code>Ok</code></td><td><code>200</code></td><td>Normal case</td></tr><tr><td><code>NoMatch</code></td><td><code>200</code></td><td>The input did not produce any matches, or the <code>waypoints</code> requested were not found in the resulting match. <code>features</code> will be an empty array.</td></tr><tr><td><code>NoSegment</code></td><td><code>200</code></td><td>No road segment could be matched for one or more coordinates within the supplied <code>radiuses</code>. Check for coordinates that are too far away from a road.</td></tr><tr><td><code>Not Authorized - No Token</code></td><td><code>401</code></td><td>No token was used in the query.</td></tr><tr><td><code>Not Authorized - Invalid Token</code></td><td><code>401</code></td><td>Check the access token you used in the query.</td></tr><tr><td><code>Forbidden</code></td><td><code>403</code></td><td>There may be an issue with your account. Check your <a href="https://console.mapbox.com/">Account page</a> for more details.<br><br>In some cases, using an access tokens with URL restrictions can also result in a <code>403</code> error. For more information, see our <a href="https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions">Token management guide</a>.</td></tr><tr><td><code>TooManyCoordinates</code></td><td><code>422</code></td><td>There are more than 100 points in the regular request, or more than 50 points for OpenLR input.</td></tr><tr><td><code>ProfileNotFound</code></td><td><code>404</code></td><td>Needs to be a valid profile (<code>mapbox/driving</code>, <code>mapbox/driving-traffic</code>, <code>mapbox/walking</code>, or <code>mapbox/cycling</code>).</td></tr><tr><td><code>InvalidInput</code></td><td><code>422</code></td><td><code>message</code> will hold an explanation of the invalid input.</td></tr></tbody></table>

## Map Matching API restrictions and limits

-   The Map Matching API is limited to 300 requests per minute.
-   Each regular request can have a maximum of 100 coordinates.
-   Each OpenLR request can have a maximum of 50 coordinates.
-   Results must be displayed on a Mapbox map using one of the Mapbox [libraries or SDKs](https://mapbox.com/documentation).

If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).

## Map Matching API pricing

-   Billed by **requests**
-   See rates and discounts per Map Matching API request in the pricing page's **[Navigation](https://www.mapbox.com/pricing/#matching)** section

Usage of the Map Matching API is measured in **API requests**. A request that contains multiple waypoints is billed as a single API request. Details about the number of Map Matching API requests included in the free tier and the cost per request beyond what is included in the free tier are available on the [pricing page](https://www.mapbox.com/pricing/#matching).