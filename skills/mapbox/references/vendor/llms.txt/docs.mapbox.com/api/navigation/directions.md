# Directions API

> **Note: Directions API version**
> 
> This documentation is for `v5` of the Directions API. For information about the earlier version, see the [`v4` documentation](https://docs.mapbox.com/api/legacy/directions-v4/) or [view the changelog](https://docs.mapbox.com/api/navigation/changelog/#directions-api).

The **Mapbox Directions API** will show you how to get where you're going. With the Directions API, you can:

-   Calculate optimal driving, walking, and cycling routes using traffic- and incident-aware routing
-   Produce turn-by-turn instructions
-   Produce routes with up to 25 coordinates for the `driving`, `driving-traffic`, `walking`, and `cycling` profiles
-   Calculate routes for electric vehicles to reach destinations with optimal charging stops as well as battery prediction

> **Related content (playground): [Directions API playground](https://docs.mapbox.com/playground/directions/)**
> 
> Retrieve directions between waypoints and see the results on a map.

> **Related content (tutorial): [Directions API tutorials](https://docs.mapbox.com/help/tutorials/?product=Directions+API)**
> 
> To see all tutorials related to the Directions API, use the *Products* filter on our Tutorials page.

## Routing profiles

The Directions API supports 4 different routing profiles:

| Profile | Description |
| --- | --- |
| `mapbox/driving-traffic` | For automotive routing. This profile factors in current and historic traffic conditions to avoid slowdowns. Traffic information is available for the supported geographies listed in our [Traffic Data page](https://docs.mapbox.com/help/getting-started/directions/#traffic-data). Requests using this profile accept up to 25 coordinates. |
| `mapbox/driving` | For automotive routing. This profile shows the fastest routes by preferring high-speed roads like highways. Requests using this profile accept up to 25 coordinates. |
| `mapbox/walking` | For pedestrian and hiking routing. This profile shows the optimal path by using sidewalks and trails. Requests using this profile accept up to 25 coordinates. |
| `mapbox/cycling` | For bicycle routing. This profile shows routes that are short and safer for cyclists by avoiding highways and preferring streets with bike lanes. Requests using this profile accept up to 25 coordinates. |

## Retrieve directions

**GET** : `https://api.mapbox.com/directions/v5/{profile}/{coordinates}`

### Required parameters

Retrieve directions between waypoints. Directions requests must specify at least two waypoints as starting and ending points.

| Required parameters | Type | Description |
| --- | --- | --- |
| `profile` | `string` | The [routing profile](https://docs.mapbox.com/api/navigation/directions/#routing-profiles) to use. Possible values are `mapbox/driving-traffic`, `mapbox/driving`, `mapbox/walking`, or `mapbox/cycling`. |
| `coordinates` | `number` | A semicolon-separated list of between two and 25 `{longitude},{latitude}` coordinate pairs to visit in order. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |

### Optional parameters

You can further refine the results from this endpoint with the following optional parameters:

<table><thead><tr><th>Optional parameters</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>alternatives</code></td><td><code>boolean</code></td><td>Whether to try to return alternative routes (<code>true</code>) or not (<code>false</code>, default). An alternative route is a route that is significantly different from the fastest route, but still close in time. Such a route does not exist in all circumstances. Up to two alternatives may be returned. This is available for <code>mapbox/driving-traffic</code>, <code>mapbox/driving</code>, <code>mapbox/cycling</code> and <code>mapbox/walking</code>.</td></tr><tr><td><code>annotations</code></td><td><code>string</code></td><td>Return additional metadata along the route. You can include several annotations as a comma-separated list. <strong>Must be used in conjunction with <code>overview=full</code>.</strong><table class="mt12"><tbody><tr><th>Possible values</th><th>Description</th></tr><tr><td><code>distance</code></td><td>The distance between each pair of coordinates, in meters.</td></tr><tr><td><code>duration</code></td><td>The duration between each pair of coordinates, in seconds.</td></tr><tr><td><code>speed</code></td><td>The speed between each pair of coordinates, in meters per second.</td></tr><tr><td><code>congestion</code></td><td>The level of congestion between each entry in the array of coordinate pairs in the route leg. This annotation is only available for the <code>mapbox/driving-traffic</code> profile.</td></tr><tr><td><code>congestion_numeric</code></td><td>The numeric level of congestion between each entry in the array of coordinate pairs in the route leg. This annotation is available only for the <code>mapbox/driving-traffic</code> profile.</td></tr><tr><td><code>maxspeed</code><div style="padding-top:1px;letter-spacing:0.07em" class="txt-fancy-medium round inline-block cursor-default color-gray-dark bg-blue-lighter color-blue-dark txt-s px6" data-state="closed">BETA</div></td><td>The maximum speed limit between the coordinates of a segment. This annotation is only available for the <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code> profiles.</td></tr><tr><td><code>closure</code></td><td>An array of closure objects describing live-traffic related closures that occur along the route. This annotation is only available for the <code>mapbox/driving-traffic</code> profile.</td></tr><tr><td><code>state_of_charge</code><div style="padding-top:1px;letter-spacing:0.07em" class="txt-fancy-medium round inline-block cursor-default color-gray-dark bg-blue-lighter color-blue-dark txt-s px6" data-state="closed">BETA</div></td><td>The battery's current state of charge as a percentage of the maximum capacity. The value can be negative, signifying a charge deficit. Positive values show the battery's charge level.</td></tr></tbody></table>See the <a href="#route-leg-object">route leg object</a> for more details on what is included with annotations.</td></tr><tr><td><code>avoid_maneuver_radius</code></td><td><code>number</code></td><td>Add a radius around the starting point in which the API will avoid returning any significant maneuvers. Possible values are in the range from <code>1</code> to <code>1000</code> meters. Use this option when the vehicle is traveling at a significant speed to avoid dangerous maneuvers when re-routing. If a route is not found using the specified value, it will be ignored. Note that if a large radius is used, the API may ignore an important turn and return a long straight path before the first maneuver. The <code>avoid_maneuver_radius</code> parameter cannot be used with the <code>arrive_by=&lt;time&gt;</code> or <code>depart_at=&lt;time&gt;</code> options, since these modes are used for reference requests, not for real-time routing. But you can use the <code>depart_at=now</code> option. This is available for <code>mapbox/driving-traffic</code> and <code>mapbox/driving</code>.</td></tr><tr><td><code>bearings</code></td><td><code>integer</code></td><td>Influences the direction in which a route <em>starts</em> from a waypoint. Used to filter the road segment the waypoint will be placed on by direction. This is useful for making sure the new routes of rerouted vehicles continue traveling in their current direction. A request that does this would provide bearing and radius values for the first waypoint and leave the remaining values empty. Takes two comma-separated values per waypoint: an angle clockwise from true north between <code>0</code> and <code>360</code>, and the range of degrees by which the angle can deviate (recommended value is 45° or 90°), formatted as <code>{angle, degrees}</code>. If provided, the list of bearings must be the same length as the list of coordinates. But, you can skip a coordinate and show its position in the list with the <code>;</code> separator.</td></tr><tr><td><code>layers</code></td><td><code>integer</code></td><td>Influences the layer of road from which a route <em>starts</em> from a waypoint. Used to filter the road segment the waypoint will be placed on, in Z-order. This is useful to avoid ambiguity in the case of multi-level roads (for example, a tunnel under a road). Takes a single signed integer per waypoint. If provided, the list of layers must be the same length as the list of coordinates. But, you can skip a coordinate and show its position in the list with the <code>;</code> separator. Corresponds to the <a href="https://docs.mapbox.com/data/tilesets/reference/mapbox-streets-v8/#--road---layer-number"><code>layer</code> field in Mapbox Streets v8</a>. Note that if a matching layer is not found, the API will choose a suitable layer according to other provided parameters.</td></tr><tr><td><code>continue_straight</code></td><td><code>boolean</code></td><td>Sets the allowed direction of travel when departing intermediate waypoints. If <code>true</code>, the route will continue in the same direction of travel. If <code>false</code>, the route may continue in the opposite direction of travel. Defaults to <code>true</code> for <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code>, and <code>false</code> for <code>mapbox/walking</code> and <code>mapbox/cycling</code>.</td></tr><tr><td><code>exclude</code></td><td><code>string</code></td><td>Exclude certain road types and custom locations from routing. Default is to not exclude anything from the list below. You can specify multiple values as a comma-separated list. The following exclude values are available:<table class="mt12"><tbody><tr><th>Possible values</th><th>Description</th></tr><tr><td><code>motorway</code></td><td>Exclude highways or motorways. Available in <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code> profiles.</td></tr><tr><td><code>toll</code></td><td>Exclude roads with tolls. Available in <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code> profiles.</td></tr><tr><td><code>ferry</code></td><td>Exclude ferry rides across waterways. Available in <code>mapbox/driving</code>, <code>mapbox/driving-traffic</code>, <code>mapbox/cycling</code> and <code>mapbox/walking</code> profiles.</td></tr><tr><td><code>unpaved</code></td><td>Exclude dirt roads or track. Available in <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code> profiles.</td></tr><tr><td><code>cash_only_tolls</code></td><td>Exclude roads with tolls that accept only cash. Available in <code>mapbox/driving</code>, <code>mapbox/driving-traffic</code>, <code>mapbox/walking</code> and <code>mapbox/cycling</code> profiles.</td></tr><tr><td><code>point(longitude latitude)</code><div style="padding-top:1px;letter-spacing:0.07em" class="txt-fancy-medium round inline-block cursor-default color-gray-dark bg-blue-lighter color-blue-dark txt-s px6" data-state="closed">BETA</div></td><td>Exclude custom locations such as dangerous entry/exit points, bridges, tunnels, low quality roads and cross border roads. Coordinate specified in WKT format will be snapped to the nearest road and the snapped coordinate will be excluded from routing. You can specify multiple coordinates with a comma-separated list. For example <code>point(lon1 lat1), point(lon2 lat2)</code>. Note that at most 50 points are allowed per API request. Available in <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code> profiles.</td></tr><tr><td><code>country_border</code><div style="padding-top:1px;letter-spacing:0.07em" class="txt-fancy-medium round inline-block cursor-default color-gray-dark bg-blue-lighter color-blue-dark txt-s px6" data-state="closed">BETA</div></td><td>Exclude crossing of any controlled country border. Note that borders within the <a href="https://en.wikipedia.org/wiki/Schengen_Area">Schengen Area</a> will not be excluded. Available in <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code> profiles.</td></tr><tr><td><code>state_border</code><div style="padding-top:1px;letter-spacing:0.07em" class="txt-fancy-medium round inline-block cursor-default color-gray-dark bg-blue-lighter color-blue-dark txt-s px6" data-state="closed">BETA</div></td><td>Exclude crossing of any state border. Available in <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code> profiles.</td></tr><tr><td><code>tunnel</code><div style="padding-top:1px;letter-spacing:0.07em" class="txt-fancy-medium round inline-block cursor-default color-gray-dark bg-blue-lighter color-blue-dark txt-s px6" data-state="closed">BETA</div></td><td>Exclude roads with tunnels. Available in <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code> profiles.</td></tr></tbody></table><p class="mt12">Exclusions are best-effort. When an excluded road type cannot be avoided (for example, it is the only way to reach a waypoint), the route may still use it. These cases are reported in the <a href="#notification-object">notification object</a> as a <code>violation</code>, which you can check to detect when an exclusion was not honored.</p></td></tr><tr><td><code>geometries</code></td><td><code>string</code></td><td>The format of the returned geometry. Allowed values are: <code>geojson</code> (as <a href="https://tools.ietf.org/html/rfc7946#appendix-A.2">LineString</a>), <a href="https://developers.google.com/maps/documentation/utilities/polylinealgorithm"><code>polyline</code></a> (default, a polyline with a precision of five decimal places), <a href="https://developers.google.com/maps/documentation/utilities/polylinealgorithm"><code>polyline6</code></a> (a polyline with a precision of six decimal places).</td></tr><tr><td><code>include</code></td><td><code>string</code></td><td>Include certain additional road types for routing. The default is to not include these road types. You can specify multiple values as a comma-separated list. The following include values are available:<table class="mt12"><tbody><tr><th>Possible values</th><th>Description</th></tr><tr><td><code>hov2</code></td><td>A road type that requires a minimum of two vehicle occupants. Available in <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code> profiles.</td></tr><tr><td><code>hov3</code></td><td>A road type that requires a minimum of three vehicle occupants. Available in <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code> profiles.</td></tr><tr><td><code>hot</code></td><td>A high occupancy toll road that is tolled if your vehicle doesn't meet the minimum occupant requirement. Available in <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code> profiles.</td></tr></tbody></table></td></tr><tr><td><code>overview</code></td><td><code>string</code></td><td>Displays the requested type of overview geometry. Can be <code>full</code> (the most detailed geometry available), <code>simplified</code> (default, a simplified version of the full geometry), or <code>false</code> (no overview geometry).</td></tr><tr><td><code>radiuses</code></td><td><code>number</code> or <code>string</code></td><td>The maximum distance a coordinate can be moved to snap to the road network, in meters. There must be as many radiuses as there are coordinates in the request, each separated by <code>;</code>. Values can be any number greater than <code>0</code> or the string <code>unlimited</code>. A <code>NoSegment</code> error is returned if no routable road is located within the radius.</td></tr><tr><td><code>approaches</code></td><td><code>string</code></td><td>A semicolon-separated list indicating the side of the road from which to approach waypoints in a requested route. Accepts <code>unrestricted</code> (default, route can arrive at the waypoint from either side of the road) or <code>curb</code> (route will arrive at the waypoint on the <code>driving_side</code> of the region). If provided, the number of approaches must be the same as the number of waypoints. But, you can skip a coordinate and show its position in the list with the <code>;</code> separator. Since the first value will not be evaluated, begin the list with a semicolon. If the waypoint is within 1 meter of the road, this parameter is ignored.</td></tr><tr><td><code>steps</code></td><td><code>boolean</code></td><td>Whether to return steps and turn-by-turn instructions (<code>true</code>) or not (<code>false</code>, default).<br><br>Setting <code>steps</code> to <code>true</code> will make the following guidance-related parameters available: <code>banner_instructions</code>, <code>language</code>, <code>roundabout_exits</code>, <code>voice_instructions</code>, <code>voice_units</code>, <code>waypoint_names</code>, <code>waypoint_targets</code>, and <code>waypoints</code>.</td></tr><tr><td><code>banner_instructions</code></td><td><code>boolean</code></td><td>Whether to return banner objects associated with the route steps (<code>true</code>) or not (<code>false</code>, default). <strong>Must be used in conjunction with <code>steps=true</code>.</strong></td></tr><tr><td><code>language</code></td><td><code>string</code></td><td>The language of returned turn-by-turn text instructions. See <a href="#instructions-languages">supported languages</a>. The default is <code>en</code> (English). <strong>Must be used in conjunction with <code>steps=true</code>.</strong></td></tr><tr><td><code>roundabout_exits</code></td><td><code>boolean</code></td><td>Whether to emit instructions at roundabout exits (<code>true</code>) or not (<code>false</code>, default). Without this parameter, roundabout maneuvers are returned as a single instruction that includes both entering and exiting the roundabout. With <code>roundabout_exits=true</code>, this maneuver becomes two instructions, one for entering the roundabout and one for exiting it. <strong>Must be used in conjunction with <code>steps=true</code>.</strong></td></tr><tr><td><code>voice_instructions</code></td><td><code>boolean</code></td><td>Whether to return <a href="https://developer.amazon.com/docs/custom-skills/speech-synthesis-markup-language-ssml-reference.html">SSML</a> marked-up text for voice guidance along the route (<code>true</code>) or not (<code>false</code>, default). <strong>Must be used in conjunction with <code>steps=true</code>.</strong></td></tr><tr><td><code>voice_units</code></td><td><code>string</code></td><td>Specify which type of units to return in the text for voice instructions. Can be <code>imperial</code> (default), <code>british_imperial</code> or <code>metric</code>. <strong>Must be used in conjunction with <code>steps=true</code> and <code>voice_instructions=true</code>.</strong></td></tr><tr><td><code>waypoints</code></td><td><code>integer</code></td><td>A semicolon-separated list indicating which input coordinates should be treated as waypoints. Waypoints form the beginning and end of each <a href="#route-leg-object"><code>leg</code></a> in the returned route, and when the <code>steps=true</code> parameter is used, correspond to the <code>depart</code> and <code>arrive</code> <a href="#route-step-object">steps</a>. If a list of waypoints is not provided, all coordinates are treated as waypoints. Each item in the list must be the zero-based index of an input coordinate, and the list must include <code>0</code> (the index of the first coordinate) and the index of the last coordinate. The <code>waypoints</code> parameter can be used to guide the path of the route without introducing additional legs and <code>arrive</code>/<code>depart</code> instructions. <strong>Must be used in conjunction with <code>steps=true</code>.</strong></td></tr><tr><td><code>waypoints_per_route</code></td><td><code>boolean</code></td><td>If <code>true</code>, the waypoints array is returned within the route object, else its returned at the root of the response body. Defaults to <code>false</code> if unspecified. Available in <code>mapbox/driving</code>, <code>mapbox/driving-traffic</code> and <code>mapbox/walking</code> profiles.<br>Setting <code>waypoints_per_route</code> to <code>true</code> is necessary when asking for an EV-optimized route with alternatives, since each alternative route may produce separate sets of waypoints (charging stations).</td></tr><tr><td><code>waypoint_names</code></td><td><code>string</code></td><td>A semicolon-separated list of custom names for entries in the list of <code>coordinates</code>, used for the arrival instruction in banners and voice instructions. Values can be any string, and the total number of all characters cannot exceed 500. If provided, the list of <code>waypoint_names</code> must be the same length as the list of <code>waypoints</code>, but you can skip a waypoint coordinate and show its position in the list with the <code>;</code> separator. The first value in the list corresponds to the route origin, not the first destination. To leave the origin unnamed, begin the list with a semicolon. <strong>Must be used in conjunction with <code>steps=true</code>.</strong></td></tr><tr><td><code>waypoint_targets</code></td><td>number</td><td>A semicolon-separated list of coordinate pairs used to specify drop-off locations that are distinct from the locations specified in <code>coordinates</code>. If this parameter is provided, the Directions API will compute the side of the street, left or right, for each target based on the <code>waypoint_targets</code> and the driving direction. The <code>maneuver.modifier</code>, banner and voice instructions will be updated with the computed side of street. The number of <code>waypoint_targets</code> must be the same as the number of <code>waypoints</code>, but you can skip a waypoint coordinate and show its position in the list with the <code>;</code> separator. Since the first value will not be evaluated, begin the list with a semicolon. <strong>Must be used in conjunction with <code>steps=true</code>.</strong></td></tr><tr><td><code>notifications</code></td><td><code>string</code></td><td>Returns notification metadata associated with the route leg of the route object. Notifications allow the exposure of warnings or safety messages within your application, or important insights about your route. Available in <code>mapbox/driving</code> and <code>mapbox/driving-traffic</code> profiles.<table class="mt12"><tbody><tr><th>Possible values</th><th>Description</th></tr><tr><td><code>all</code></td><td>Include all notification metadata.</td></tr><tr><td><code>none</code></td><td>Do not provide notifications in route metadata.</td></tr></tbody></table>The default is <code>all</code>. See the <a href="#notification-object">notification object</a> for more details on what is included with notifications.</td></tr></tbody></table>

### Optional parameters for the `mapbox/walking` profile

| Optional `v5/mapbox/walking` parameters | Type | Description |
| --- | --- | --- |
| `walking_speed` | `number` | The walking speed, in meters per second, with a minimum of `0.14` m/s (or `0.5` km/h) and a maximum of `6.94` m/s (or `25.0` km/h). Defaults to `1.42` m/s (`5.1` km/h). |
| `walkway_bias` | `number` | A scale from `-1` to `1`, where `-1` biases the route against walkways and `1` biases the route toward walkways. The default is `0`, which is neutral. |

Unrecognized options in the query string result in an `InvalidInput` error.

### Optional parameters for the `mapbox/driving` profile

| Optional `v5/mapbox/driving` parameters | Type | Description |
| --- | --- | --- |
| `alley_bias` | `number` | A scale from `-1` to `1`, where `-1` biases the route against alleys and `1` biases the route toward alleys. The default is `0`, which is neutral. |
| `arrive_by` | `string` | The desired arrival time, formatted in one of three [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) formats: `YYYY-MM-DDThh:mm:ssZ`, `YYYY-MM-DDThh:mmss±hh:mm`, or `YYYY-MM-DDThh:mm`. In the last format, the timezone is calculated from the route destination. The travel time, returned in `duration`, is a prediction for travel time based on historical travel data. The route is calculated in a time-dependent manner. For example, a trip that takes two hours will consider changing historic traffic conditions across the two-hour window. The route takes timed turn restrictions and conditional access restrictions into account based on the requested arrival time. |
| `depart_at` | `string` | The departure time, formatted in one of three [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) formats: `YYYY-MM-DDThh:mm:ssZ`, `YYYY-MM-DDThh:mmss±hh:mm`, or `YYYY-MM-DDThh:mm`. In the last format, the timezone is calculated from the route origin. The travel time, returned in `duration`, is a prediction for travel time based on historical travel data. The route is calculated in a time-dependent manner. For example, a trip that takes two hours will consider changing historic traffic conditions across the two-hour window, instead of only at the specified `depart_at` time. The route takes timed turn restrictions and conditional access restrictions into account based on the requested departure time. |
| `max_height` | `number` | The max vehicle height, in meters. If this parameter is provided, the Directions API will compute a route that includes only roads with a height limit greater than or equal to the max vehicle height. `max_height` must be between 0 and 10 meters. The default value is 1.6 meters. *Coverage for road height restriction may vary by region.* |
| `max_width` | `number` | The max vehicle width, in meters. If this parameter is provided, the Directions API will compute a route that includes only roads with a width limit greater than or equal to the max vehicle width. `max_width` must be between 0 and 10 meters. The default value is 1.9 meters. *Coverage for road width restriction may vary by region.* |
| `max_weight` | `number` | The max vehicle weight, in metric tons (1000 kg). If this parameter is provided, the Directions API will compute a route that includes only roads with a weight limit greater than or equal to the max vehicle weight. `max_weight` must be between 0 and 100 metric tons. The default value is 2.5 metric tons. *Coverage for road weight restriction may vary by region.* |

### Optional parameters for the `mapbox/driving-traffic` profile

| Optional `v5/mapbox/driving-traffic` parameters | Type | Description |
| --- | --- | --- |
| `depart_at` | `string` | The departure time, formatted in one of three [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) formats: `YYYY-MM-DDThh:mm:ssZ`, `YYYY-MM-DDThh:mmss±hh:mm`, or `YYYY-MM-DDThh:mm`. In the last format, the timezone is calculated from the route origin. The travel time, returned in `duration`, is a prediction for travel time based on historical travel data and live traffic. Live traffic is gently mixed with historical data when the value for `depart_at` is close to current time. The route takes timed turn restrictions and conditional access restrictions into account based on the requested arrival time. |
| `snapping_include_closures` | `boolean` | A semicolon-separated list of boolean values affecting snapping of coordinate locations to road segments. If `true`, road segments closed due to live-traffic closures will be considered for snapping. If `false`, they will not be considered for snapping. If provided, the number of `snapping_include_closures` must be the same as the number of coordinates. But, you can skip a coordinate and show its position in the list with the `;` separator. If unspecified, this parameter defaults to `false`. |
| `snapping_include_static_closures` | `boolean` | A semicolon-separated list of boolean values affecting snapping of coordinate locations to road segments. If `true`, road segments statically closed, that is long-term, will be considered for snapping (for example, road under construction). If `false`, they will not be considered for snapping. If provided, the number of `snapping_include_static_closures` must be the same as the number of coordinates. But, you can skip a coordinate and show its position in the list with the `;` separator. If unspecified, this parameter defaults to `false`. |
| `max_height` | `number` | The max vehicle height, in meters. If this parameter is provided, the Directions API will compute a route that includes only roads with a height limit greater than or equal to the max vehicle height. `max_height` must be between 0 and 10 meters. The default value is 1.6 meters. *Coverage for road height restriction may vary by region.* |
| `max_width` | `number` | The max vehicle width, in meters. If this parameter is provided, the Directions API will compute a route that includes only roads with a width limit greater than or equal to the max vehicle width. `max_width` must be between 0 and 10 meters. The default value is 1.9 meters. *Coverage for road width restriction may vary by region.* |
| `max_weight` | `number` | The max vehicle weight, in metric tons (1000 kg). If this parameter is provided, the Directions API will compute a route that includes only roads with a weight limit greater than or equal to the max vehicle weight. `max_weight` must be between 0 and 100 metric tons. The default value is 2.5 metric tons. *Coverage for road weight restriction may vary by region.* |

### Example requests: Retrieve directions

```bash
# Request directions with no additional options

$ curl "https://api.mapbox.com/directions/v5/mapbox/cycling/-122.42,37.78;-77.03,38.91?access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request to access speed limit information using the maxspeed annotation
$ curl "https://api.mapbox.com/directions/v5/mapbox/driving/-122.39636,37.79129;-122.39732,37.79283;-122.39606,37.79349?annotations=maxspeed&overview=full&geometries=geojson&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request directions with radiuses and a polyline6 response through multiple waypoints

$ curl "https://api.mapbox.com/directions/v5/mapbox/driving/13.43,52.51;13.42,52.5;13.41,52.5?radiuses=40;;100&geometries=polyline6&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Specify bearings and radiuses

$ curl "https://api.mapbox.com/directions/v5/mapbox/driving/13.43,52.51;13.42,52.5;13.43,52.5?bearings=60,45;;45,45&radiuses=100;100;100&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Assign layers to waypoints, to handle ambiguities in the case of multi-level roads

$ curl "https://api.mapbox.com/directions/v5/mapbox/driving/13.43,52.51;13.42,52.5;13.43,52.5?layers=-1;;3&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request a route that approaches the destination on the curb of the driving side

$ curl "https://api.mapbox.com/directions/v5/mapbox/driving/13.43,52.51;13.43,52.5?approaches=unrestricted;curb&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request directions with voice and banner instructions specifying waypoint_names

$ curl "https://api.mapbox.com/directions/v5/mapbox/cycling/-122.42,37.78;-77.03,38.91?steps=true&voice_instructions=true&banner_instructions=true&voice_units=imperial&waypoint_names=Home;Work&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Specify waypoints

$ curl "https://api.mapbox.com/directions/v5/mapbox/driving/13.43,52.51;13.42,52.5;13.43,52.5?waypoints=0;2&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Specify a departure time

$ curl "https://api.mapbox.com/directions/v5/mapbox/driving/-122.396112,37.791399;-122.433904,37.757812?access_token=YOUR_MAPBOX_ACCESS_TOKEN&depart_at=2019-05-02T15:00"
```

### Supported libraries: Retrieve directions

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Directions for Swift](https://github.com/mapbox/mapbox-directions-swift#calculating-directions-between-locations/)
-   [Mapbox Java SDK](https://docs.mapbox.com/android/java/api/libjava-services/5.8.0/com/mapbox/api/directions/v5/package-frame.html)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#getdirections)
-   [Mapbox Ruby SDK](https://github.com/mapbox/mapbox-sdk-rb/blob/master/docs/directions.md)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

### Response: Retrieve directions

The response to a Directions API request is a JSON object that contains the following properties:

<table><thead><tr><th>Property</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>code</code></td><td><code>string</code></td><td>A code indicating the state of the response. This is a different code than the HTTP status code. On normal valid responses, the value will be <code>Ok</code>. For other responses, see the <a href="#directions-api-errors">Directions API errors table</a>.</td></tr><tr><td><code>waypoints</code><div style="padding-top:1px;letter-spacing:0.07em" class="txt-fancy-medium round inline-block cursor-default color-gray-dark bg-orange-lighter color-orange-dark txt-s px6" data-state="closed">LEGACY</div></td><td><code>array</code></td><td>An array of <a href="#waypoint-object">waypoint</a> objects. Each waypoint is an input coordinate snapped to the road and path network. The waypoints appear in the array in the order of the input coordinates.<br>📌 <strong>Deprecation Notice</strong><br>The location of the waypoints property at the root of the response is now deprecated. You can continue to use this and it will work as-is. It remains the default structure (<code>waypoints_per_route=false</code>). We have added a <code>waypoints_per_route=true</code> parameter to request the new waypoints response moving forward. This is necessary when asking for an EV-optimized route with alternatives. See the new <code>waypoints_per_route</code> parameter in the <a href="#optional-parameters">optional parameters</a> and the new waypoints per route object <code>waypoints</code> in the <a href="#route-object">route object</a>.</td></tr><tr><td><code>routes</code></td><td><code>array</code></td><td>An array of <a href="#route-object">route</a> objects ordered by descending recommendation rank. The response object may contain at most two routes.</td></tr><tr><td><code>uuid</code></td><td><code>string</code></td><td>A unique identifier, you can use to reference the response for successfully built route(s). Combination of <code>uuid</code> and the index in <code>routes</code> array uniquely identifies the route.</td></tr></tbody></table>

### Example response: Retrieve directions

#### *NEW* waypoints_per_route=true

```json
{
  "routes": [
    {
      "geometry": "mnn_Ick}pAfBiF`CzA",
      "waypoints": [ // A waypoint array specific to this route
        { // A regular, user-submitted waypoint (the origin).
          "name": "Köpenicker Straße",
          "location": [ 13.426579, 52.508068 ]
        },
        { // A regular, user-submitted waypoint (the destination).
          "name": "Engeldamm",
          "location": [ 13.427292, 52.506902 ]
        }
      ],
      "legs": [
        {
          "summary": "Köpenicker Straße, Engeldamm",
          "weight": 44.4,
          "duration": 26.2,
          "steps": [
            {
              "intersections": [
                {
                  "out": 0,
                  "entry": [ true ],
                  "bearings": [ 125 ],
                  "location": [ 13.426579, 52.508068 ]
                },
                {
                  "out": 1,
                  "in": 2,
                  "entry": [ true, true, false ],
                  "bearings": [ 30, 120, 300 ],
                  "location": [ 13.426688, 52.508022 ]
                }
              ],
              "driving_side": "right",
              "geometry": "mnn_Ick}pAHUlAqDNa@",
              "mode": "driving",
              "maneuver": {
                "bearing_after": 125,
                "bearing_before": 0,
                "location": [ 13.426579, 52.508068 ],
                "modifier": "right",
                "type": "depart",
                "instruction": "Head southeast on Köpenicker Straße (L 1066)"
              },
              "ref": "L 1066",
              "weight": 35.9,
              "duration": 17.7,
              "name": "Köpenicker Straße (L 1066)",
              "distance": 98.1,
              "voiceInstructions": [
                {
                  "distanceAlongGeometry": 98.1,
                  "announcement": "Head southeast on Köpenicker Straße (L 1066), then turn right onto Engeldamm",
                  "ssmlAnnouncement": "<speak><amazon:effect name=\"drc\"><prosody rate=\"1.08\">Head southeast on Köpenicker Straße (L <say-as interpret-as=\"address\">1066</say-as>), then turn right onto Engeldamm</prosody></amazon:effect></speak>"
                },
                {
                  "distanceAlongGeometry": 83.1,
                  "announcement": "Turn right onto Engeldamm, then you will arrive at your destination",
                  "ssmlAnnouncement": "<speak><amazon:effect name=\"drc\"><prosody rate=\"1.08\">Turn right onto Engeldamm, then you will arrive at your destination</prosody></amazon:effect></speak>"
                }
              ],
              "bannerInstructions": [
                {
                  "distanceAlongGeometry": 98.1,
                  "primary": {
                    "text": "Engeldamm",
                    "components": [ { "text": "Engeldamm" } ],
                    "type": "turn",
                    "modifier": "right"
                  },
                  "secondary": null,
                  "sub": null
                }
              ]
            },
            {
              "intersections": [
                {
                  "out": 2,
                  "in": 3,
                  "entry": [ false, true, true, false ],
                  "bearings": [ 30, 120, 210, 300 ],
                  "location": [ 13.427752, 52.50755 ]
                }
              ],
              "driving_side": "right",
              "geometry": "ekn_Imr}pARL\\T^RHDd@\\",
              "mode": "driving",
              "maneuver": {
                "bearing_after": 202,
                "bearing_before": 125,
                "location": [ 13.427752, 52.50755 ],
                "modifier": "right",
                "type": "turn",
                "instruction": "Turn right onto Engeldamm"
              },
              "weight": 8.5,
              "duration": 8.5,
              "name": "Engeldamm",
              "distance": 78.6,
              "voiceInstructions": [
                {
                  "distanceAlongGeometry": 27.7,
                  "announcement": "You have arrived at your destination",
                  "ssmlAnnouncement": "<speak><amazon:effect name=\"drc\"><prosody rate=\"1.08\">You have arrived at your destination</prosody></amazon:effect></speak>"
                }
              ],
              "bannerInstructions": [
                {
                  "distanceAlongGeometry": 78.6,
                  "primary": {
                    "text": "You will arrive at your destination",
                    "components": [ { "text": "You will arrive at your destination" } ],
                    "type": "arrive",
                    "modifier": "straight"
                  },
                  "secondary": {
                    "text": "Engeldamm",
                    "components": [ { "text": "Engeldamm" } ],
                    "type": "arrive",
                    "modifier": "straight"
                  }
                },
                {
                  "distanceAlongGeometry": 15,
                  "primary": {
                    "text": "You have arrived at your destination",
                    "components": [ { "text": "You have arrived at your destination" } ],
                    "type": "arrive",
                    "modifier": "straight"
                  },
                  "secondary": {
                    "text": "Engeldamm",
                    "components": [ { "text": "Engeldamm" } ]
                  }
                }
              ]
            },
            {
              "intersections": [
                {
                  "in": 0,
                  "entry": [ true ],
                  "bearings": [ 25 ],
                  "location": [ 13.427292, 52.506902 ]
                }
              ],
              "driving_side": "right",
              "geometry": "cgn_Iqo}pA",
              "mode": "driving",
              "maneuver": {
                "bearing_after": 0,
                "bearing_before": 205,
                "location": [ 13.427292, 52.506902 ],
                "type": "arrive",
                "instruction": "You have arrived at your destination"
              },
              "weight": 0,
              "duration": 0,
              "name": "Engeldamm",
              "distance": 0,
              "voiceInstructions": [ ],
              "bannerInstructions": [ ]
            }
          ],
          "distance": 176.7
        }
      ],
      "weight_name": "auto",
      "weight": 44.4,
      "duration": 26.2,
      "distance": 176.7
    },
    {  // A second route object or alternative route with potentially another set of waypoints
      "geometry": "mnn_Ick}pAfBiF`CzA",
      "waypoints": [ // A waypoint array specific to this route
        { // A regular, user-submitted waypoint (the origin).
          "name": "alt1_origin",
          "location": [ 13.426579, 52.508068 ]
        },
        { // A regular, user-submitted waypoint (the destination).
          "name": "alt1_destination",
          "location": [ 13.427292, 52.506902 ]
        }
      ],
      "legs": [
        { ... }
      ],
      "weight_name": "auto",
      "weight": 44.4,
      "duration": 26.2,
      "distance": 176.7
    }
  ],
  "code": "Ok",
  "uuid": "cjd51uqn5005447p8nte2zc4w"
}
```

#### *OLD* waypoints_per_route=false / default

```json
{
  "routes": [
    {
      ...
    }
  ],
  "waypoints": [
    {
      "name": "Köpenicker Straße",
      "location": [ 13.426579, 52.508068 ]
    },
    {
      "name": "Engeldamm",
      "location": [ 13.427292, 52.506902 ]
    }
  ],
  "code": "Ok",
  "uuid": "cjd51uqn5005447p8nte2zc4w"
}
```

### Use HTTP POST to retrieve directions

The Directions API also supports access using the HTTP `POST` method. HTTP `POST` should be used for large requests, since the Directions API has a size limit of approximately 8100 bytes on `GET` request URLs. `POST` requests are still subject to your account's request size limits.

Learn more about this process in the [Using HTTP POST](https://docs.mapbox.com/api/navigation/http-post/) section.

## Electric Vehicle Routing

> **Note (beta): Electric Vehicle Routing Private Preview**
> 
> The Directions API now supports electric vehicle (EV) routing that includes EV trip planning and battery prediction along the route, making smarter EVs that are easier to use. For pricing and to sign up for the EV Routing Private Preview, [contact EV Sales](http://www.mapbox.com/contact/electric-vehicle)

EV routing predicts state of charge on the route ahead and automatically identifies if charging is needed. If required, charging stops are added along the journey with optimal charge time and charging level for each stop. Mapbox EV Routing API calculates predicted state of charge on the route ahead using 3 sets of information:

-   **Live sensor data** is the live state of charge of the vehicle and fixed energy consumption due to auxiliary consumption.
-   **Vehicle energy model** is the description of EV energy model in simplified parameters. This can be static or dynamically computed along the trip.
-   **Dynamic Map data** includes the live speed data, road topology data like elevation on the route. This is coming from Mapbox cloud. 

Based on predicted state of charge on the route ahead, charging stations are automatically added to the route. The charging stops are returned as [waypoints with additional metadata](#charging-waypoint-metadata) in the route response. In some cases such as when range is too low to reach nearby charging stations, EV route may contain no charging stops with state of charge annotations going below 0%.

Live availability of charging stations is considered in the station selection with occupied stations penalized and out-of-order stations ignored. Customize the EV route by defining the battery characteristics, vehicle dynamics and user preferences. See [Parameters for Electric Vehicle Routing.](#parameters-for-electric-vehicle-routing).

### Parameters for Electric Vehicle Routing

EV routing is available in `driving` and `driving-traffic` profiles. Request an EV route by setting the parameter `engine=electric`; the other parameters apply only when the former is true. EV routing requests can specify up to 12 total waypoints. If ev_add_charging_stops is `false`, EV routing requests can specify up to 25 total waypoints

#### Required EV Parameters

| Parameters | Type | Description |
| --- | --- | --- |
| `engine` | `string` | Set to `electric` to enable electric vehicle routing. If this parameter is not specified, routing will default to non-electric vehicles. |
| `ev_max_charge` | `integer` | The maximum possible charge of vehicle in Wh (watt-hours). Example value of `80000` denotes a battery capacity of 80 kWh. |
| `ev_connector_types` | `string` | The compatible connector-types for the vehicle. The router will select the charging stations compatible with connector-types. Following connector-types are supported:
| Possible values | Description |
| --- | --- |
| `ccs_combo_type1` | Combined Charging System J-plug with fast charging DC |
| `ccs_combo_type2` | Combined Charging System Mennekes with fast charging DC |
| `tesla` | Proprietary connector for tesla vehicles |
| `chademo` | CHAdeMO fast-charging system |

 |
| `energy_consumption_curve` | `string` | The energy consumption in watt-hours per kilometer at a various speeds in kph. Example value of `0,300;20,160;80,140;120,180` defines for instance 160 watt-hours per kilometer at 20 km/h. |
| `ev_charging_curve` | `string` | The maximum battery charging rate (W) at a given charge level (Wh) in a list of pairs. Example value of `0,100000; 40000,70000; 60000,30000; 80000,10000` defines for instance 70 kW charging when the battery has 40 kWh charge. The charging curve's charging levels should span from 0 to maximum charge. The first element's charging rate is assumed to be constant between the 0 and the first element's charging level. The last element's charging rate is assumed to be constant between the last element's charging level and `ev_max_charge`. |

#### Optional EV Parameters

| Parameters | Type | Description |
| --- | --- | --- |
| `ev_initial_charge` | `integer` | Initial charge of the vehicle in Wh (watt-hours) at the beginning of the route. Example value of `10000` denotes initial battery of 10 kWh. The default value is equal to `ev_max_charge`. |
| `ev_unconditioned_charging_curve` | string | The maximum battery charging rate (W) at a specified charge level (Wh) in a list of pairs when the battery is in an unconditioned state (for example, cold). This is used in conjunction with the `ev_pre_conditioning_time parameter`, as a result both must be supplied when using either optional field. If only one of these parameters is supplied, this is considered an invalid query and will be rejected. An unconditioned battery may affect the charge time, and may impact the ETA as a result. By accounting for an unconditioned battery with `ev_unconditioned_charging_curve` and `ev_pre_conditioning_time` a more exact ETA can be calculated. Example value of `0,50000; 42000,35000; 60000,15000; 80000,5000` defines for instance 35 kW charging when the battery has 42 kWh charge. If the battery conditioning parameters are not provided, the standard `ev_charging_curve` is used for calculating charge times. The unconditioned charging curve's charging levels should span from 0 to maximum charge. The first element's charging rate is assumed to be constant between the 0 and the first element's charging level. The last element's charging rate is assumed to be constant between the last element's charging level and `ev_max_charge`. |
| `ev_pre_conditioning_time` | `integer` | The time in minutes it would take for the vehicle's battery to condition. This is used in conjunction with the `ev_unconditioned_charging_curve parameter`, as a result both must be supplied when using either optional field. If only one of these parameters is supplied, this is considered an invalid query and will be rejected. An unconditioned battery may affect the charge time, and may impact the ETA as a result. By accounting for an unconditioned battery with `ev_unconditioned_charging_curve` and `ev_pre_conditioning_time` a more exact ETA can be calculated. Example value of `10` denotes it will take 10 minutes for the battery to reach a conditioned state. If the next EV charging stop occurs within 10 minutes of the estimated drive, the `ev_unconditioned_battery_charging_curve` is used to estimate charging times at the EV stop. If an EV charging stop occurs after `ev_pre_conditioning_time` of driving time has elapsed, it is assumed the battery is conditioned and the standard `ev_charging_curve` is used to estimate charging times. The minimum `ev_pre_conditioning_time` value is 0 minutes, the maximum value is 240 minutes. If the battery conditioning parameters are not provided, the standard `ev_charging_curve` is used for calculating charge times. |
| `ev_max_ac_charging_power` | `integer` | The maximum AC charging power(W) that can be delivered by the onboard vehicle charger. This parameter along with the `ev_charging_curve` dictates the battery charging rate. For example, when charging at a 16kW AC power outlet, with a vehicle onboard AC converter of 10kW (`ev_max_ac_charging_power`), router will use max charging power of 10kW. The default value is 14400W. |
| `ev_min_charge_at_destination` | `integer` | The minimum battery charge required at the final route destination (Wh). Example value of `20000` denotes that vehicle will reach destination with at least 20 kWh charge. `ev_min_charge_at_destination` must be between 0 and `ev_max_charge`. The default value is 10% of `ev_max_charge`. |
| `ev_min_charge_at_charging_station` | `integer` | The minimum charge when arriving at the charging station (Wh). Example value of `20000` denotes that the vehicle will reach each charging stop with at least 20 kWh charge. `ev_min_charge_at_charging_station` must be between 0 and `ev_max_charge`. The default value is 10% of `ev_max_charge`. |
| `ev_ascent` | `number` | The energy consumption rate for each meter of elevation gain, measured in watt-hours per meter (Wh/m). This parameter is required if `ev_descent` is specified. |
| `ev_descent` | `number` | The potential energy recuperation rate for each meter of elevation loss, measured in watt-hours per meter (Wh/m). This is the potential energy restored to the battery during descent. This parameter is required if `ev_ascent` is specified. |
| `ev_regeneration_efficiency` | `number` | Energy recuperation efficiency measures how effectively potential energy released during descent is converted back into battery charge, after accounting for energy losses. Allowed range is [0.0, 1.0]. If not provided 0.7 is used by default. |
| `auxiliary_consumption` | `integer` | The measure of the continuous power draw of the auxiliary systems (e.g heating, radio, air conditioning) in watts. Auxiliary consumption will affect the estimated power consumed during the trip, and may affect routing as a result. Example value of `300` denotes the vehicle's auxiliary systems are drawing 300 watts of power. The minimum value is 0 watts, the maximum value is 10000 watts. |
| `ev_add_charging_stops` | `boolean` | Whether EV route should add charging stops or not. When set to false, EV route will not add charging stops even if vehicle does not have enough range to complete the trip. This is useful for manually planned EV trips with user or client selecting appropriate charging stops. When set to true (default), EV route will add charging stops along the trip with optimal charge time and charging level for each stop, if needed to complete the trip with enough range. |
| `ev_prefer_amenities` | `string` | A comma separated list of preferred amenities. Supported values: `light,restaurant,restroom,recreation_center`. The routing algorithm will apply a soft preference to charging stations that offer the specified amenities. |
| `ev_exclude_station_ids` | `string` | Stations that should be excluded at the planning stage. A comma separated list of station IDs with a limit of 40 IDs in a request. An input error is generated if any station in this list is used as user-provided charging station (field `waypoints.charging_station_id`). [EV Charge Finder API](https://docs.mapbox.com/api/navigation/ev-charge-finder/) can be used to find station IDs. Example: `ev_exclude_station_ids=dX...tz,dX...bs` |
| `ev_exclude_charge_point_provider_ids` | `string` | A comma separated list of charge point provider IDs to exclude in the charge point search. These providers will not be used in planning and if route is only possible with these providers a route without charging points or with constraints violations can be returned. [EV Charge Finder API](https://docs.mapbox.com/api/navigation/ev-charge-finder/) can be used to find charge point providers IDs and to find provider ID for a particular station. The maximum number of IDs in a query is 40. |
| `ev_prefer_charge_point_provider_ids` | `string` | A comma separated list of charge point provider IDs to prefer in the charge point search. Planner will try to use these stations as long as route including them does not violate any other constraints. [EV Charge Finder API](https://docs.mapbox.com/api/navigation/ev-charge-finder/) can be used to find charge point providers IDs and to find provider ID for a particular station. The maximum number of IDs in a query is 30. |
| `ev_exclude` | `string` | A comma-delimited list of attributes to use to exclude stations. Stations having one or more of the specified attributes will not be used in planning. If a route is only possible using these stations, a route without charging stations or with constraint violations may be returned.
| Possible values | Description |
| --- | --- |
| `tesla_exclusive` | Charge point that can only be used by Tesla cars. |

 |
| `ev_max_charging_voltage` | `integer` | Required if you set `ev_max_charging_power_at_lower_voltage`. Maximum DC charging voltage supported by the vehicle's battery (in volts). Used to determine charging power when the station voltage is lower. If the station voltage is higher, this value does nothing since charging is limited by the vehicle's charging curve. |
| `ev_max_charging_power_at_lower_voltage` | `integer` | Required if you set `ev_max_charging_voltage`. Maximum DC charging power (in watts) when charging from a station that supplies voltage below `ev_max_charging_voltage`. |
| `ev_min_energy_increment_at_charging_station` | `integer` | The minimum amount of energy (Wh) that must be added when stopping at a charging station. When the route planner determines a charging stop is needed, but the calculated charge increment would be less than this value, the planner will increase the charge to at least this minimum increment (capped at `ev_max_charge`). This parameter also applies to user-provided charging stations, the minimum increment is always enforced even if the vehicle arrives with enough charge and can be used to prevent short charging stops. For example, a value of 5000 ensures that every charging stop adds at least 5 kWh to the battery. The value must be between 0 and `ev_max_charge`. |

#### Waypoint EV Parameters

The `waypoint` parameters are semicolon-separated lists that align one-to-one with the `coordinates` parameter to signify that a coordinate is an EV charging station. If EV charging stations are specified, the directions API will take them into account when calculating the route.

If a coordinate is not associated with a charging station, its position should still be represented with an empty entry (for example, consecutive semicolons). It is important to note that origin waypoints do not support charging station attributes, so all three parameters must begin with a leading semicolon. This structure ensures that the planner correctly associates charging details with the appropriate waypoints.

| Parameters | Type | Description |
| --- | --- | --- |
| `waypoints.charging_station_power` | `string` | A semicolon-separated list of integers representing power in watts for `coordinates` that represent EV charging stations. If charging is not needed at a particular charging waypoint, the corresponding `charge_time` value in the `waypoint.metadata` object of the route response will be `0`. Power levels must be greater than or equal to 1000 watts. |
| `waypoints.charging_station_current_type` | `string` | A semicolon-separated list of strings representing the current type of `coordinates` that represent EV charging stations. Supported values are `single_phase_ac`, `three_phase_ac`, and `dc`. If the current type is `single_phase_ac` or `three_phase_ac`, the maximum charging power at the station is limited to the `ev_max_ac_charging_power` parameter if provided. |
| `waypoints.charging_station_id` | `string` | A semicolon-separated list of unique IDs for `coordinates` that represent EV charging stations. The unique ID is used to look up charging station details in the EV charging station database. If the station with provided ID is present in the planner's database, the `waypoints.charging_station_power` and `waypoints.charging_station_current` values corresponding to the charging waypoint may be ignored in favor of the planner's station details. If the station ID is not found, the user specified power and current values are used. To get more details about a station refer to [EV Charge Finder API](https://docs.mapbox.com/api/navigation/ev-charge-finder/) and to [Get Charge Point details by ID](https://docs.mapbox.com/api/navigation/ev-charge-finder/#get-charge-point-details) call. |

### Charging station data

The Mapbox EV Routing processes charging stations and builds a digital graph of interconnected stations. When you make an EV route request, the router will pick up the nearest stations along the route and identify the fastest chargers that are compatible with the driver's model of EV.

### Supported geographies for EV Routing

Mapbox EV Router supports the following geographies for EV trip planning:

-   Australia
-   Austria
-   Belgium
-   Bulgaria
-   Canada
-   Croatia
-   Czech Republic
-   Denmark
-   Estonia
-   Finland
-   France
-   Germany
-   Greece

-   Hong Kong
-   Hungary
-   Iceland
-   Ireland
-   Israel
-   Italy
-   Japan
-   Jersey
-   Latvia
-   Liechtenstein
-   Lithuania
-   Luxembourg
-   Macao

-   Malaysia
-   Monaco
-   Netherlands
-   New Zealand
-   Norway
-   Poland
-   Portugal
-   Romania
-   Serbia
-   Singapore
-   Slovakia
-   Slovenia
-   South Africa

-   South Korea
-   Spain
-   Sweden
-   Switzerland
-   Taiwan
-   Thailand
-   Turkey
-   United Arab Emirates
-   United Kingdom
-   United States

### Example EV route request

```bash
$ curl "https://api.mapbox.com/directions/v5/mapbox/driving/11.59838875578771,48.1498484882143;11.646071564986642,48.157168731123306.json?overview=false&alternatives=true&waypoints_per_route=true&engine=electric&ev_initial_charge=600&ev_max_charge=50000&ev_connector_types=ccs_combo_type1,ccs_combo_type2&energy_consumption_curve=0,300;20,160;80,140;120,180&ev_charging_curve=0,100000;40000,70000;60000,30000;80000,10000&ev_min_charge_at_charging_station=1&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Example EV Alternative route response with EV charging stops

#### *NEW* waypoints_per_route=true, alternatives=true

```json
{
  "routes": [
    {
      "weight_name": "auto",
      "weight": 729.764,
      "duration": 1371.657958984375,
      "distance": 4575.367,
      "waypoints": [  // A waypoint array specific to this route
        { // A regular, user-submitted waypoint (the origin).
          "name": "Ifflandstraße",
          "location": [
            11.59804,
            48.149988
          ],
          "distance": 30.207000732421875,
          "metadata": null
        },
        { // A silent waypoint, as in it does not create a new leg.
          // It only describes where it matched to the route.
          "metadata": {
            "type": "silent",
            "distance_from_route_start": 2088,
            "geometry_index": 11
          },
          ...
        },
        { // A charging-station waypoint inserted by the routing engine during
          // an EV route request
          "name": "Arabellastraße",
          "location": [
            11.617788,
            48.153125
          ],
          "distance": 18.554000854492188,
          "metadata": {
            "type": "charging-station",
            "name": "BayWa Zentrale",
            "charge_time": 788,
            "charge_to": 5369,
            "charge_at_arrival": 250,
            "plug_type": "ccs_combo_type2",
            "power_kw": 150,
            "station_id": "ocm-128831",
            "provider_names": ["Tesla", "Shell Recharge Solutions"],
          }
        },
        { // A regular, user-submitted waypoint (the destination).
          "name": "Barlowstraße",
          "location": [
            11.646591,
            48.156744
          ],
          "distance": 1.1330000162124634,
          "metadata": null
        }
      ],
      "legs": [
        {
          "via_waypoints": [],
          "summary": "Ifflandstraße, B 2R",
          "admins": [
            {
              "iso_3166_1_alpha3": "DEU",
              "iso_3166_1": "DE"
            }
          ],
          "weight": 333.797,
          "duration": 255.738,
          "steps": [],
          "distance": 2262.614
        },
        {
          "via_waypoints": [],
          "summary": "Arabellastraße, Englschalkinger Straße",
          "admins": [
            {
              "iso_3166_1_alpha3": "DEU",
              "iso_3166_1": "DE"
            }
          ],
          "weight": 395.967,
          "duration": 327.92,
          "steps": [],
          "distance": 2312.754
        }
      ]
    },
    {  // A second Route object, our alternative route with potentially another set of waypoints
      "geometry": "mnn_Ick}pAfBiF`CzA",
      "waypoints": [ // A waypoint array specific to this route
        { // A regular, user-submitted waypoint (the origin).
          "metadata": {
            "type": "regular"
          },
          "name": "alt1_origin",
          "distance": 1.1330000162124634
        },
        { // A charging-station waypoint inserted by the routing engine during an EV route request
          "metadata": {
            "type": "charging-station",
            "name": "charge1",
            ...
          },
          ...
        },
        { // A regular, user-submitted waypoint (the destination).
          "name": "alt1_destination",
          "location": [
            11.646591,
            48.156744
          ],
          "distance": 1.1330000162124634,
          "metadata": {
            "type": "regular"
          }
        }
        ],
          "legs": [
            { ... }
          ],
          "weight_name": "auto",
          "weight": 44.4,
          "duration": 26.2,
          "distance": 176.7
        }
  ],
  "code": "Ok",
  "uuid": "QJuXPALlclx4ciVHn-UPxhqlUD0W7jjiv1tXFH_haUpp_VkdD0sqGQ=="
}
```

#### *OLD* waypoints_per_route=false / default, no alternatives

```json
{
  "routes": [
    {
      "weight_name": "auto",
      "weight": 729.764,
      "duration": 1371.657958984375,
      "distance": 4575.367,
      "legs": [
        {
          "via_waypoints": [],
          "summary": "Ifflandstraße, B 2R",
          "admins": [
            {
              "iso_3166_1_alpha3": "DEU",
              "iso_3166_1": "DE"
            }
          ],
          "weight": 333.797,
          "duration": 255.738,
          "steps": [],
          "distance": 2262.614
        },
        {
          "via_waypoints": [],
          "summary": "Arabellastraße, Englschalkinger Straße",
          "admins": [
            {
              "iso_3166_1_alpha3": "DEU",
              "iso_3166_1": "DE"
            }
          ],
          "weight": 395.967,
          "duration": 327.92,
          "steps": [],
          "distance": 2312.754
        }
      ]
    }
  ],
  "waypoints": [
    {
      "name": "Ifflandstraße",
      "location": [11.59804, 48.149988],
      "distance": 30.207000732421875,
      "metadata": null
    },
    {
      "name": "Arabellastraße",
      "location": [11.617788, 48.153125],
      "distance": 18.554000854492188,
      "metadata": {
        "type": "charging-station",
        "name": "BayWa Zentrale",
        "charge_time": 788,
        "charge_to": 5369,
        "charge_at_arrival": 250,
        "plug_type": "ccs_combo_type2",
        "power_kw": 150,
        "station_id": "ocm-128831",
        "provider_names": ["Tesla", "Shell Recharge Solutions"]
      }
    },
    {
      "name": "Barlowstraße",
      "location": [11.646591, 48.156744],
      "distance": 1.1330000162124634,
      "metadata": null
    }
  ],
  "code": "Ok",
  "uuid": "QJuXPALlclx4ciVHn-UPxhqlUD0W7jjiv1tXFH_haUpp_VkdD0sqGQ=="
}
```

### Charging waypoint metadata

The Directions API returns charging stops as [waypoints](#waypoint-object) along the route. Each charging waypoint has additional `metadata` describing the charging stop. The following attributes are present in the `metadata` object:

| Property | Type | Description |
| --- | --- | --- |
| `type` | `string` | The type of waypoint. Set to `charging-station` for charging stops. |
| `name` | `string` | The name of the EV charging station. |
| `charge_time` | `number` | Time in seconds the EV needs to charge for at the station. |
| `charge_to` | `number` | Watt-hours the EV needs to charge to at the station. |
| `charge_at_arrival` | `number` | Battery charge, in Watt-hours, available on arrival at the station. |
| `plug_type` | `string` | The connector type to use for charging at the station. The current non-exhaustive list of possible values are:-   `ccs_combo_type1`
-   `ccs_combo_type2`
-   `chademo`
-   `tesla`
-   `mennekes_type2` |
| `current_type` | `string` | The current type of the connector used for charging at the station. The current non-exhaustive list of possible values are:-   `dc`
-   `single_phase_ac`
-   `three_phase_ac` |
| `power_kw` | `number` | The maximum power available for the connector type to use for charging at the station. |
| `station_id` | `string` | Unique ID of the charging station. |
| `provider_names` | `array` | Optional. The charge point providers of the EV charging station. There are no guarantees provider name strings are stable. |

## Waypoint object

The response body of a Directions API query contains a **waypoint object**, the input coordinates snapped to the roads network. A waypoint object contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `name` | `string` | The name of the road or path to which the input coordinate has been snapped. |
| `location` | `array` | The snapped coordinate as `[longitude, latitude]`. |
| `time_zone`
BETA

 | `object` | An [object describing time zone relevant information](#time-zone-information). This is only available for `driving-traffic`, `driving` and `walking` profiles. If using `walking` or `driving` profile, then `depart_at` or `arrive_by` must be used to receive time zone data. |
| `distance` | `number` | *Optional.* The straight-line distance from the coordinate specified in the query to the location it was snapped to. |
| `metadata` | `object` | *Optional.* An [object describing charging stops](#charging-waypoint-metadata) inserted by EV Routing for routes requiring charging along the way. Its set to `null` for user provided waypoints including start & end locations. |

### Example waypoint object

```json
{
  "name": "Kirkjubøarvegur",
  "location": [-6.80897, 62.000075],
  "distance": 25,
  "time_zone": {
    "abbreviation": "WET",
    "identifier": "Atlantic/Faroe",
    "offset": "+00:00"
  },
  "metadata": {
    // metadata only for EV-routing
    "type": "charging-station",
    "name": "BayWa Zentrale",
    "charge_time": 788,
    "charge_to": 5369,
    "charge_at_arrival": 250,
    "plug_type": "ccs_combo_type2",
    "power_kw": 150,
    "station_id": "ocm-128831",
    "provider_names": ["Tesla", "Shell Recharge Solutions"]
  }
}
```

### Time Zone Information

Each [`waypoint`](#waypoint-object) contains a `time_zone` object with the following fields:

| Property | Type | Description |
| --- | --- | --- |
| `identifier` | `string` | A unique string that specifies a time zone in the format of Region/Location, for example `America/New_York` or `Europe/Paris`, as defined by the IANA Time Zone Database. |
| `offset` | `string` | The difference in hours and minutes between a specific time zone and Coordinated Universal Time (UTC), for example `-05:00` for Eastern Standard Time or `+01:00` for Central European Time. |
| `abbreviation` | `string` | *Optional.* A short, commonly recognized abbreviation for a time zone, often used for display purposes, for example `EST` for Eastern Standard Time or `CET` for Central European Time. Note that this field may not always be available. |

## Route object

The response body of a Directions API query also contains an array of **route objects**. A route object describes a route through multiple waypoints. A route object contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `duration` | `number` | The estimated travel time through the waypoints, in seconds. |
| `distance` | `number` | The distance traveled through the waypoints, in meters. |
| `weight_name` | `string` | Specifies the weight used, which by default is duration-based and includes additional penalties for less desirable maneuvers. For `mapbox/driving` & `mapbox/driving-traffic` its set to `auto`, while for `mapbox/walking` its set to `pedestrian`. |
| `weight` | `number` | A numeric value representing desirability of a route, where a lower weight indicates a more favorable route when comparing two routes with the same set of waypoints. In case multiple routes are returned, they are sorted in ascending order based on their weights. |
| `duration_typical` | `number` | When using the `driving-traffic` profile, this will be returned as a float indicating the duration of the route under typical conditions (not taking into account live traffic). |
| `weight_typical` | `number` | When using the `driving-traffic` profile, this will be returned as a float indicating the weight of the selected route under typical conditions (not taking into account live traffic). |
| `geometry` | `string` | Depending on the `geometries` query parameter, this is either a [GeoJSON LineString](https://tools.ietf.org/html/rfc7946#appendix-A.2) or a [Polyline string](https://developers.google.com/maps/documentation/utilities/polylinealgorithm). Depending on the `overview` query parameter, this is the complete route geometry (`full`), a simplified geometry to the zoom level at which the route can be displayed in full (`simplified`), or is not included (`false`). |
| `legs` | `array` | An array of [route leg](#route-leg-object) objects. |
| `voiceLocale` | `string` | The locale used for voice instructions. Defaults to `en` (English). Can be any [accepted instruction language](#instructions-languages). `voiceLocale` is only present in the response when `voice_instructions=true`. |
| `waypoints` | `array` | When the input parameter `waypoints_per_route=true` is used, the waypoints will appear in the route object and display an array of [waypoint](#waypoint-object) objects specific to that route. Each waypoint is an input coordinate snapped to the road and path network. The waypoints appear in the order traversed by the route. These waypoints can now contain additional waypoints like charging-stations, if used in EV-routing. |

### Example route object

```json
{
  "waypoints": [], // only with waypoints_per_route=true
  "duration": 88.4,
  "distance": 830.4,
  "weight": 88.4,
  "weight_name": "routability",
  "geometry": "oklyJ`{ph@yBuY_F{^_FxJoBrBs@d@mAT",
  "legs": [],
  "voiceLocale": "en"
}
```

## Route leg object

Route leg objects are nested within the route object. A **route leg** is the journey from an origin point to a destination point, so a route will usually contain one fewer route legs than it has number of input coordinates. Using the `waypoints=` parameter will override this. For example:

-   A route request with two input coordinates defines one leg with no waypoints.
-   A route request with three input coordinates defines two legs with one waypoint.
-   A route request with four input coordinates will have two legs if it includes the `waypoints=` parameter specifying three waypoints.

Each route leg object contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `distance` | `number` | The distance traveled between waypoints, in meters. |
| `duration` | `number` | The estimated travel time between waypoints, in seconds. |
| `weight` | `number` | The weight in units described by `weight_name`. |
| `duration_typical` | `number` | When using the driving-traffic profile, this will be returned as sign of duration of the leg under typical conditions (not taking into account live traffic). |
| `weight_typical` | `number` | When using the driving-traffic profile, this will be returned as sign of the weight of the leg under typical conditions (not taking into account live traffic). |
| `steps` | `array` | Depending on the optional `steps` parameter, either an array of [route step](#route-step-object) objects (`steps=true`) or an empty array (`steps=false`, default). |
| `summary` | `string` | A summary of major roads traversed in this leg of the route. |
| `admins` | `array` | An array of objects describing the administrative boundaries the route leg travels through. Use `admin_index` on the intersection object to look up the administrative boundaries for each intersection in this array. |
| `incidents` | `array` | An array of [incident objects](#incident-object) describing temporary events that occur along the roadway. This is only provided if incidents exist and you are using the `mapbox/driving-traffic` profile. |
| `closures` | `array` | Included in the route leg object when making a `mapbox/driving-traffic` request with `annotations=closure,...` and there are live-traffic closures along the route. This is an array of `closure` objects which contain the following properties:
| Property | Description |
| --- | --- |
| `geometry_index_start` | The position in the coordinate list where the closure began, relative to the start of the leg it's on. |
| `geometry_index_end` | The position in the coordinate list where the closure ended, relative to the start of the leg it's on. |

 |
| `admins.iso_3166_1` | `string` | Contains the two-letter [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) code that applies to a country boundary. Example: `"US"`. |
| `admins.iso_3166_1_alpha3` | `string` | Contains the three-letter [ISO 3166-1 alpha-3](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) code that applies to a country boundary. Example: `"USA"`. |
| `annotation` | `object` | An annotations object that contains additional details about each line segment along the route geometry. Each entry in an annotations field corresponds to a coordinate along the route geometry. |
| `annotation.congestion` | `string` | The level of congestion, described as `severe`, `heavy`, `moderate`, `low` or `unknown`, between each entry in the array of coordinate pairs in the route leg. For any profile other than `mapbox/driving-traffic` a list of `unknown`s will be returned. |
| `annotation.congestion_numeric` | `number` or `null` | The level of congestion in numeric form, from 0-100. A value of 0 indicates no congestion, a value of 100 indicates maximum congestion. Entries may be `null` if congestion on that segment is not known. |
| `annotation.distance` | `number` | The distance between each pair of coordinates, in meters. |
| `annotation.duration` | `number` | The duration between each pair of coordinates, in seconds. |
| `annotation.maxspeed` | `object` | The local posted speed limit between each pair of coordinates. A `maxspeed` object contains the following properties:

| Property | Description |
| --- | --- |
| `speed` (optional) | The maximum speed limit between the coordinates of a segment. The value can be either the legal speed limit or an advisory speed limit where posted |
| `unit` (optional) | The unit of measurement that `speed` is expressed in, either `km/h` or `mph`. Returned in combination with `speed`. |
| `unknown` (optional) | Returns `true` if the speed limit is not known. |
| `none` (optional) | Set to `true` if the speed limit is unlimited (for instance, a German Autobahn). |

Note that `none` or `unknown` will not be present if `speed` and `unit` are present in an individual `maxspeed` object, and that `speed` and `unit` will not be present if `unknown` is present. |
| `annotation.state_of_charge` | `number` | The battery's current state of charge as a percentage of the maximum capacity. The value can be negative, signifying a charge deficit. Positive values show the battery's charge level. The range of values extends from negative −231 to 100, with 100 representing a fully charged battery. The state-of-charge can become negative if there are no valid routes with charging-stops. In that case, a route without additional injected charging stops will be returned, where the state-of-charge will reach and fall below 0. It is up to the client application to take appropriate measures (if any) when this happens. |
| `annotation.speed` | `number` | The average speed used in the calculation between the two points in each pair of coordinates, in meters per second. |
| `via_waypoints` | `array` | When the semicolon-separated list `waypoints` parameter is used in the request, an array per leg is returned that describes where a particular waypoint from the root-level array matches to the route. This provides a way for tracking when a waypoint is passed along the route. Also see the optional [waypoints](https://docs.mapbox.com/api/navigation/directions/#retrieve-directions) parameter description. |
| `via_waypoints.waypoint_index` | `number` | The associated waypoint index, excluding the origin (index 0) and destination. |
| `via_waypoints.distance_from_start` | `double` | The calculated distance, in meters, from the leg origin. |
| `via_waypoints.geometry_index` | `number` | The associated leg shape index of the via waypoint location. |
| `notifications` | `array` | An optional array of [notification objects](#notification-object) describing notifications about the route. |

### Example route leg object

```json
{
  "via_waypoints": [
    {
      "waypoint_index": 1,
      "distance_from_start": 127.951,
      "geometry_index": 6
    },
    {
      "waypoint_index": 2,
      "distance_from_start": 236.822,
      "geometry_index": 10
    }
  ],
  "annotation": {
    "distance": [
      4.294596842089401, 5.051172053200946, 5.533254065167979,
      6.576513793849532, 7.4449640160938015, 8.468757534990829,
      15.202780313562714, 7.056346577326572
    ],
    "duration": [1, 1.2, 2, 1.6, 1.8, 2, 3.6, 1.7],
    "speed": [4.3, 4.2, 2.8, 4.1, 4.1, 4.2, 4.2, 4.2],
    "congestion": [
      "low",
      "moderate",
      "moderate",
      "moderate",
      "heavy",
      "heavy",
      "heavy",
      "heavy"
    ],
    "maxspeed": [
      {
        "speed": 56,
        "unit": "km/h"
      },
      {
        "speed": 56,
        "unit": "km/h"
      },
      {
        "speed": 56,
        "unit": "km/h"
      },
      {
        "speed": 56,
        "unit": "km/h"
      },
      {
        "speed": 56,
        "unit": "km/h"
      },
      {
        "speed": 56,
        "unit": "km/h"
      },
      {
        "speed": 56,
        "unit": "km/h"
      },
      {
        "speed": 56,
        "unit": "km/h"
      }
    ]
  },
  "duration": 99.824,
  "weight": 131.349,
  "distance": 580.039,
  "steps": [],
  "summary": ""
}
```

## Route step object

In a route leg object, a nested **route step** object includes one [step maneuver](#step-maneuver-object) object as well as information about travel to the following route step:

<table><thead><tr><th>Property</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>maneuver</code></td><td><code>object</code></td><td>One <a href="#step-maneuver-object">step maneuver</a> object.</td></tr><tr><td><code>distance</code></td><td><code>number</code></td><td>The distance traveled, in meters, from the maneuver to the next route step.</td></tr><tr><td><code>duration</code></td><td><code>number</code></td><td>The estimated time traveled, in seconds, from the maneuver to the next route step.</td></tr><tr><td><code>weight</code></td><td><code>number</code></td><td>The weight in units described by <code>weight_name</code>.</td></tr><tr><td><code>duration_typical</code></td><td><code>number</code></td><td>When using the driving-traffic profile, this will be returned as sign of duration of the step under typical conditions (not taking into account live traffic).</td></tr><tr><td><code>weight_typical</code></td><td><code>number</code></td><td>When using the driving-traffic profile, this will be returned as sign of the weight of the step under typical conditions (not taking into account live traffic).</td></tr><tr><td><code>geometry</code></td><td><code>string</code></td><td>Depending on the <code>geometries</code> parameter, this is a <a href="https://tools.ietf.org/html/rfc7946#appendix-A.2">GeoJSON LineString</a> or a <a href="https://developers.google.com/maps/documentation/utilities/polylinealgorithm">Polyline string</a> representing the full route geometry from this route step to the next route step.</td></tr><tr><td><code>name</code></td><td><code>string</code></td><td>The name of the road or path that forms part of the route step.</td></tr><tr><td><code>ref</code></td><td><code>string</code></td><td>Any <a href="https://en.wikipedia.org/wiki/Road_designation_or_abbreviation">road designations</a> associated with the road or path leading from this step’s maneuver to the next step’s maneuver. If <a href="https://en.wikipedia.org/wiki/Concurrency_%28road%29">multiple road designations</a> are associated with the road, they are separated by semicolons. Typically consists of an alphabetic network code (identifying the road type or numbering system), a space or hyphen, and a <a href="https://en.wikipedia.org/wiki/Route_number">route number</a>. Optionally included, if data is available.<br><strong>Note:</strong> A network code is not necessarily globally unique, and should not be treated as though it is. A route number may not uniquely identify a road within a given network.</td></tr><tr><td><code>destinations</code></td><td><code>string</code></td><td>The destinations of the road or path along which the travel proceeds. Optionally included, if data is available.</td></tr><tr><td><code>exits</code></td><td><code>string</code></td><td>The exit numbers or names of the road or path. Optionally included, if data is available.</td></tr><tr><td><code>driving_side</code></td><td><code>string</code></td><td>The legal driving side at the location for this step. Either <code>left</code> or <code>right</code>.</td></tr><tr><td><code>mode</code></td><td><code>string</code></td><td>The mode of transportation.<table class="mt12"><tbody><tr><th>Profile</th><th>Possible values</th></tr><tr><td><code>mapbox/driving</code></td><td><code>driving</code>, <code>ferry</code>, <code>unaccessible</code></td></tr><tr><td><code>mapbox/walking</code></td><td><code>walking</code>, <code>ferry</code>, <code>unaccessible</code></td></tr><tr><td><code>mapbox/cycling</code></td><td><code>cycling</code>, <code>walking</code>, <code>ferry</code>, <code>train</code>, <code>unaccessible</code></td></tr></tbody></table></td></tr><tr><td><code>pronunciation</code></td><td><code>string</code></td><td>An <a href="https://en.wikipedia.org/wiki/International_Phonetic_Alphabet">IPA</a> phonetic transcription indicating how to pronounce the name in the <code>name</code> property. Omitted if pronunciation data is not available for the step.</td></tr><tr><td><code>intersections</code></td><td><code>array</code></td><td>An array of objects representing all the intersections along the step:</td></tr><tr><td><code>intersections.location</code></td><td><code>array</code></td><td>A <code>[longitude, latitude]</code> pair describing the location of the turn.</td></tr><tr><td><code>intersections.bearings</code></td><td><code>array</code></td><td>A list of bearing values that are available at the intersection. The bearings describe all available roads at the intersection.</td></tr><tr><td><code>intersections.classes</code></td><td><code>array</code></td><td>An array of strings signifying the classes of the road exiting the intersection.<table class="mt12"><tbody><tr><th>Possible values</th><th>Description</th></tr><tr><td><code>toll</code></td><td>Road continues on a toll road</td></tr><tr><td><code>ferry</code></td><td>Road continues on a ferry</td></tr><tr><td><code>restricted</code></td><td>Road continues on with access restrictions</td></tr><tr><td><code>motorway</code></td><td>Road continues on a motorway</td></tr><tr><td><code>tunnel</code></td><td>Road continues in a tunnel</td></tr></tbody></table></td></tr><tr><td><code>intersections.entry</code></td><td><code>array</code></td><td>A list of entry flags, corresponding with the entries in <code>bearings</code>. If the flag is <code>true</code>, indicates that the respective road could be entered on a valid route. If <code>false</code>, the turn onto the respective road would violate a restriction.</td></tr><tr><td><code>intersections.geometry_index</code></td><td><code>integer</code></td><td>The zero-based index into the geometry, relative to the start of the leg it's on. This value can be used to apply the duration annotation that corresponds with the intersection. Only available on the <code>driving</code>, <code>driving-traffic</code>, and <code>walking</code> profile.</td></tr><tr><td><code>intersections.in</code></td><td><code>integer</code></td><td>The index in the <code>bearings</code> and <code>entry</code> arrays. Used to calculate the bearing before the turn. Namely, the clockwise angle from true north to the direction of travel before the maneuver/passing the intersection. To get the bearing in the direction of driving, the bearing has to be rotated by a value of 180. The value is not supplied for departure maneuvers.</td></tr><tr><td><code>intersections.out</code></td><td><code>integer</code></td><td>The index in the <code>bearings</code> and <code>entry</code> arrays. Used to extract the bearing after the turn. Namely, the clockwise angle from true north to the direction of travel after the maneuver/passing the intersection. The value is not supplied for arrival maneuvers.</td></tr><tr><td><code>intersections.lanes</code></td><td><code>array</code></td><td>An array of <a href="#lane-object">lane</a> objects that represent the available turn lanes at the intersection. If no lane information is available for an intersection, the <code>lanes</code> property will not be present.</td></tr><tr><td><code>intersections.duration</code></td><td><code>number</code></td><td>The time required, in seconds, to traverse the intersection. Only available on the <code>driving</code> profile.</td></tr><tr><td><code>intersections.tunnel_name</code></td><td><code>string</code></td><td>The name of the tunnel when the road exiting the intersection continues in a tunnel. This property is only included when the tunnel has a specified name. Only available on the <code>driving</code> profile.</td></tr><tr><td><code>intersections.mapbox_streets_v8</code></td><td><code>object</code></td><td>Contains detailed information about the road exiting the intersection along the route. Properties in this object correspond to properties in the <a href="https://docs.mapbox.com/data/vector-tiles/reference/mapbox-streets-v8/#road">Mapbox Streets v8 road</a> specification. Only available on the <code>driving</code> profile.</td></tr><tr><td><code>intersections.mapbox_streets_v8.class</code></td><td><code>string</code></td><td>The road class of the road exiting the intersection as defined by the <a href="https://docs.mapbox.com/data/vector-tiles/reference/mapbox-streets-v8/#--road---class-text">Mapbox Streets v8 road class</a> specification. Valid values include, but are not limited to, those supported by Mapbox Streets v8. For example: <code>motorway</code>, <code>motorway_link</code>, <code>primary</code>, and <code>street</code>.</td></tr><tr><td><code>intersections.is_urban</code></td><td><code>boolean</code></td><td>Indicates whether the road exiting the intersection is considered to be in an urban area. This value is determined by the density of the surrounding road network. Only available on the <code>driving</code> profile.</td></tr><tr><td><code>intersections.admin_index</code></td><td><code>integer</code></td><td>The zero-based index into the administrative boundaries list on the <a href="#route-leg-object">route leg</a> for this intersection. Use this field to look up the ISO 3166-1 country code for this point on the route. Only available on the <code>driving</code> profile.</td></tr><tr><td><code>intersections.rest_stop</code></td><td><code>object</code></td><td>Contains information about passing <a href="https://wiki.openstreetmap.org/wiki/Tag:highway%3Drest_area">rest stops</a> along the route. Only available on the <code>driving</code> profile.</td></tr><tr><td><code>intersections.rest_stop.type</code></td><td><code>string</code></td><td>The type of rest stop, always included in the <code>rest_stop</code> object. Valid values include, but are not limited to, <code>rest_area</code> (includes parking only) and <code>service_area</code> (includes amenities such as gas or restaurants). In Japan, <code>rest_area</code> and <code>service_area</code> correspond to <code>ＰＡ</code> and <code>ＳＡ</code> in the rest stop name, regardless of their amenities.</td></tr><tr><td><code>intersections.rest_stop.name</code></td><td><code>string</code></td><td>The name of the rest stop. Optionally included if data is available.</td></tr><tr><td><code>intersections.toll_collection</code></td><td><code>object</code></td><td>Contains information about a toll collection point along the route. This is a <a href="https://wiki.openstreetmap.org/wiki/Tag:barrier%3Dtoll_booth">payment booth or overhead electronic gantry</a> where a toll charge may be collected. Only available on the <code>driving</code> profile.</td></tr><tr><td><code>intersections.toll_collection.type</code></td><td><code>string</code></td><td>The type of toll collection point, always included in the <code>toll_collection</code> object. Valid values include, but are not limited to, <code>toll_booth</code> and <code>toll_gantry</code>.</td></tr><tr><td><code>intersections.toll_collection.name</code></td><td><code>string</code></td><td>The name of toll collection point. Optionally included if data is available.</td></tr><tr><td><code>intersections.railway_crossing</code></td><td><code>boolean</code></td><td>Indicates whether there is a railway crossing at the intersection.</td></tr><tr><td><code>intersections.traffic_signal</code></td><td><code>boolean</code></td><td>Indicates whether there is a traffic signal at the intersection.</td></tr><tr><td><code>intersections.stop_sign</code></td><td><code>boolean</code></td><td>Indicates whether there is a stop sign at the intersection.</td></tr><tr><td><code>intersections.yield_sign</code></td><td><code>boolean</code></td><td>Indicates whether there is a yield sign at the intersection.</td></tr><tr><td><code>speedLimitSign</code></td><td><code>string</code></td><td>The design of speed limit signs along the route step, either <code>mutcd</code> (<a href="https://en.wikipedia.org/wiki/Manual_on_Uniform_Traffic_Control_Devices">Manual on Uniform Traffic Control Devices</a>) or <code>vienna</code> (<a href="https://en.wikipedia.org/wiki/Vienna_Convention_on_Road_Signs_and_Signals">Vienna Convention on Road Signs and Signals</a>). Only present in the response when the <code>annotations</code> query parameter contains <code>maxspeed</code>.</td></tr><tr><td><code>speedLimitUnit</code></td><td><code>string</code></td><td>The unit of measurement for speed that is used locally along the step, either <code>km/h</code> or <code>mph</code>. This unit is an appropriate default unit to display in the absence of a user preference; it may differ from the <code>unit</code> property of the annotation objects. Only present in the response when the <code>annotations</code> query parameter contains <code>maxspeed</code>.</td></tr></tbody></table>

### Example route step object

```json
{
  "intersections": [
    {
      "out": 1,
      "location": [13.424671, 52.508812],
      "bearings": [120, 210, 300],
      "entry": [false, true, true],
      "in": 0,
      "stop_sign": true,
      "lanes": [
        {
          "valid": true,
          "active": true,
          "valid_indication": "straight",
          "indications": ["left", "straight"]
        },
        {
          "valid": true,
          "active": false,
          "valid_indication": "straight",
          "indications": ["straight", "right"]
        },
        {
          "valid": false,
          "active": false,
          "indications": ["right"]
        }
      ]
    }
  ],
  "geometry": "asn_Ie_}pAdKxG",
  "maneuver": {
    "bearing_after": 202,
    "type": "turn",
    "modifier": "left",
    "bearing_before": 299,
    "location": [13.424671, 52.508812],
    "instruction": "Turn left onto Adalbertstraße"
  },
  "duration": 59.1,
  "distance": 236.9,
  "driving_side": "right",
  "weight": 59.1,
  "name": "Adalbertstraße",
  "mode": "driving"
}
```

## Step maneuver object

A route step object contains a nested **step maneuver** object, which contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `bearing_before` | `integer` | A number between `0` and `360` indicating the clockwise angle from true north to the direction of travel *before* the maneuver. |
| `bearing_after` | `integer` | A number between `0` and `360` indicating the clockwise angle from true north to the direction of travel *after* the maneuver. |
| `instruction` | `string` | A human-readable instruction of how to execute the returned maneuver. |
| `location` | `array` | The coordinates of the maneuver as `[longitude, latitude]`. |
| `modifier` | `string` | *Optional.* The direction change of the maneuver. The meaning of each `modifier` depends on the `type` property.
| Possible values | Description |
| --- | --- |
| `uturn` | Indicates a reversal of direction. `type` can be `turn` or `continue` when staying on the same road. |
| `sharp right` | A sharp right turn. |
| `right` | A normal turn to the right. |
| `slight right` | A slight turn to the right. |
| `straight` | No relevant change in direction. |
| `slight left` | A slight turn to the left. |
| `left` | A normal turn to the left. |
| `sharp left` | A sharp turn to the left. |

If the source or target location is close enough to the `depart` or `arrive` location, no `modifier` will be supplied. |
| `type` | `string` | Indicates the type of maneuver. See the full list of maneuver types in the [maneuver types table](#maneuver-types). If no `modifier` is provided, the `type` of maneuvers is limited to `depart` and `arrive`. |

### Maneuver types

| `type` | Description |
| --- | --- |
| `turn` | A turn in the direction of the modifier. |
| `new name` | The road name changes (after a mandatory turn). |
| `depart` | Indicates departure from a leg. The `modifier` value indicates the position of the departure point compared to the current direction of travel. |
| `arrive` | Indicates arrival to a destination of a leg. The `modifier` value indicates the position of the arrival point compared to the current direction of travel. |
| `merge` | Merge onto a street. |
| `on ramp` | Take a ramp to enter a highway. |
| `off ramp` | Take a ramp to exit a highway. |
| `fork` | Keep left or right side at a bifurcation, or left/right/straight at a trifurcation. |
| `end of road` | Road ends in a T intersection. |
| `continue` | Continue on a street after a turn. |
| `roundabout` | Traverse roundabout. Has an additional property `exit` in the [route step](#route-step-object) that contains the exit number. The `modifier` specifies the direction of entering the roundabout. |
| `rotary` | A traffic circle. While like a larger version of a roundabout, it does not necessarily follow roundabout rules for right of way. It can offer `rotary_name` parameters, `rotary_pronunciation` parameters, or both, located in the [route step](#route-step-object) object. It also contains the `exit` property. |
| `roundabout turn` | A small roundabout that is treated as an intersection. |
| `notification` | Indicates a change of driving conditions, for example changing the `mode` from `driving` to `ferry`. |
| `exit roundabout` | Indicates the exit maneuver from a roundabout. Will not appear in results unless you supply the `roundabout_exits=true` query parameter in the request. |
| `exit rotary` | Indicates the exit maneuver from a rotary. Will not appear in results unless you supply the `roundabout_exits=true` query parameter in the request. |

> **Note**
> 
> New properties, potentially depending on `type`, may be introduced in the future without an API version change.

### Example step maneuver object

```json
{
  "bearing_before": 299,
  "bearing_after": 202,
  "type": "turn",
  "modifier": "left",
  "location": [13.424671, 52.508812],
  "instruction": "Turn left onto Adalbertstraße"
}
```

## Lane object

A route step object contains a nested **lane object**. The lane object describes the available turn lanes at an intersection. Lanes are provided in their order on the street, from left to right.

| Property | Type | Description |
| --- | --- | --- |
| `valid` | `boolean` | Indicates whether a lane can be used to complete the maneuver (`true`) or not (`false`). For example, if a lane array has four objects and the first two are valid, the driver can take either of the left lanes and stay on the route. |
| `active` | `boolean` | Indicates whether a lane is a *preferred* lane (`true`) or not (`false`). A preferred lane is a lane that is recommended if there are multiple lanes available. For example, if guidance indicates that the driver must turn left at an intersection and there are multiple left turn lanes, the left turn lane that will better prepare the driver for the next maneuver will be marked as `active`. Only available on the `mapbox/driving` profile. |
| `valid_indication` | `string` | When either the `valid` or `active` value is `true`, this property shows which of the lane `indications` applies to the current route, when there is more than one. For example, if a lane allows you to go left or straight but your current route is guiding you to the left, then this value will be `left`. See `indications` for possible values. When both `active` and `valid` are false, this property will not be included in the response. Only available on the `mapbox/driving` profile. |
| `indications` | `array` | The indications (based on signs, road markings, or both) for a lane. A road can have multiple indications. For example, a turn lane can have a sign with an arrow pointing left and another arrow pointing straight.
| Possible values | Description |
| --- | --- |
| `none` | No dedicated sign is displayed |
| `straight` | A sign signaling to continue straight |
| `sharp left` | A sign signaling a sharp left turn |
| `left` | A sign signaling a left turn |
| `slight left` | A sign signaling a slight left turn |
| `slight right` | A sign signaling a slight right turn |
| `right` | A sign signaling a right turn |
| `sharp right` | A sign signaling a sharp right turn |
| `uturn` | A sign signaling the possibility to reverse direction by making a u-turn |

 |
| `access.designated` | `array` | Indicates whether a lane is designated to specified vehicle types. A vehicle type can be any of (but not limited to) `bicycle`, `bus`, `hov`, `moped`, `motorcycle`, or `taxi`. For example, when a lane is designated to buses and taxis, this field should have `["bus", "taxi"]` as its value. |

### Example lane object

```json
{
  "valid": true,
  "active": true,
  "valid_indication": "left",
  "indications": ["left"],
  "access": {
    "designated": ["bicycle", "bus"]
  }
}
```

## Voice instruction object

A route step object contains a nested **voice instruction object** if the optional `voice_instructions=true` query parameter is present. The voice instruction object contains the text that should be announced, along with how far from the maneuver it should be emitted. The system will announce the instructions during the route step in which the voice instruction object is nested, but the instructions refer to the maneuver in the *following* step.

| Property | Type | Description |
| --- | --- | --- |
| `distanceAlongGeometry` | `number` | A float indicating how far from the upcoming maneuver the voice instruction should begin, in meters. |
| `announcement` | `string` | The text of the verbal instruction. |
| `ssmlAnnouncement` | `string` | Contains SSML markup for proper text and pronunciation. This property is designed for use with [Amazon Polly](https://aws.amazon.com/polly/). The SSML tags may not work with other text-to-speech engines. |

### Example voice instruction object

```json
{
  "distanceAlongGeometry": 375.7,
  "announcement": "In a quarter mile, take the ramp on the right towards I 495: Baltimore",
  "ssmlAnnouncement": "<speak><amazon:effect name=\"drc\"><prosody rate=\"1.08\">In a quarter mile, take the ramp on the right towards <say-as interpret-as=\"address\">I-495</say-as>: Baltimore</prosody></amazon:effect></speak>"
}
```

## Banner instruction object

A route step object contains a nested **banner instruction object** if the optional `banner_instructions=true` query parameter is present. The banner instruction object contains the contents of a banner that should be displayed as added visual guidance for a route. The banner instructions are children of the route steps during which they should be displayed, but they refer to the maneuver in the *following* step.

| Property | Type | Description |
| --- | --- | --- |
| `distanceAlongGeometry` | `number` | A float indicating how far from the upcoming maneuver the banner instruction should begin being displayed, in meters. Only one banner should be displayed at a time. |
| `primary` | `object` | The most important content to display to the user. This text is larger and at the top. |
| `secondary` | `object` or `null` | *Optional.* Additional content useful for visual guidance. This text is slightly smaller and below `primary`. Can be `null`. |
| `sub` | `object` | *Optional.* Additional information that is included if the driver needs to be notified about something. Can include information about the *next* maneuver (the one after the upcoming one) if the step is short. If lane information is available, that takes precedence over information about the *next* maneuver. |

Each of the different banner types (`primary`, `secondary`, and `sub`) contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `text` | `string` | All the text that should be displayed. |
| `type` | `string` | *Optional.* The type of maneuver. May be used in combination with the `modifier` (and, if it is a roundabout, the `degrees`) for an icon to display. Possible values: `turn`, `merge`, `depart`, `arrive`, `fork`, `off ramp`, and `roundabout`. |
| `modifier` | `string` | *Optional.* The modifier for the maneuver. Can be used in combination with the `type` (and, if it is a roundabout, the `degrees`) for an icon to display.
| Possible values | Description |
| --- | --- |
| `uturn` | Indicates a reversal of direction |
| `sharp right` | A sharp right turn |
| `right` | A normal turn to the right |
| `slight right` | A slight turn to the right |
| `straight` | No relevant change in direction |
| `slight left` | A slight turn to the left |
| `left` | A normal turn to the left |
| `sharp left` | A sharp turn to the left |

 |
| `degrees` | `integer` | *Optional.* The degrees at which you will be exiting a roundabout, assuming `180` indicates going straight through the roundabout. |
| `driving_side` | `string` | *Optional.* The side of the street on which people drive in that location. Can be `left` or `right`. |
| `components` | `array` | An array of objects that, together, make up what should be displayed in the banner. Includes additional information intended to be used to aid in visual layout. A component can contain the following properties: |
| `components.type` | `string` | Contains more context about the component that may help in visual markup and display choices. If the type of the component is unknown, it should be treated as text.

| Possible values | Description |
| --- | --- |
| `text` | Default. Indicates the text is part of the instructions and no other type. |
| `icon` | This is text that can be replaced by an `imageBaseURL` icon. |
| `delimiter` | This is text that can be dropped, and should be dropped if you are rendering icons. |
| `exit-number` | Indicates the exit number for the maneuver. |
| `exit` | Provides the word for *exit* in the local language. |
| `lane` | Indicates which lanes can be used to complete the maneuver. |

The introduction of new types is not considered a breaking change. |
| `components.text` | `string` | The sub-string of the `text` of the parent objects that may have additional context associated with it |
| `components.abbr` | `string` | *Optional.* The abbreviated form of `text`. If this is present, there will also be an `abbr_priority` value. For an example of using `abbr` and `abbr_priority`, see the [abbreviation examples](#abbreviation-examples) table. |
| `components.abbr_priority` | `integer` | *Optional.* Indicates the order in which the abbreviation `abbr` should be used in place of `text`. The highest priority is `0`, while a higher integer value means it should have a lower priority. There are no gaps in integer values. Multiple components can have the same `abbr_priority`. When this happens, all `components` with the same `abbr_priority` should be abbreviated at the same time. Finding no larger values of `abbr_priority` means that the string is fully abbreviated. |
| `components.imageBaseURL` | `string` | *Optional.* Points to a shield image to use instead of the text. The shield image can be retrieved as an SVG by appending `.svg` to the URL string, or it can be retrieved as a PNG at 1-3× pixel density by appending `@1x.png`, `@2x.png`, or `@3x.png` to the URL string. |
| `components.directions` | `array` | *Optional.* Indicates which directions you can go from a lane (left, right, or straight). If the value is ['left', 'straight'], the driver can go straight or left from that lane. Present if `components.type` is `lane`. |
| `components.active` | `boolean` | *Optional.* Indicates whether the lane is recommended for performing the upcoming maneuver. If multiple lanes are active, all are equally recommended. Present if `components.type` is `lane`. |
| `components.active_direction` | `string` | *Optional.* When `components.active` is `true`, this property shows which of the lane's `components.directions` applies to the current route, when there is more than one. For example, if a lane allows you to go left or straight but your current route is guiding you to the left, then this value will be `left`. See `component.directions` for possible values. When `components.active` is `false`, this property will not be included in the response. Only available on the `mapbox/driving` profile. |

### Abbreviation examples

| `text` | `abbr` | `abbr_priority` |
| --- | --- | --- |
| North | N | 1 |
| Franklin Drive | Franklin Dr | 0 |

Given the `components` in the table above, the possible abbreviations are, in order:

-   North Franklin Dr
-   N Franklin Dr

### Example banner instruction object

```json
{
  "distanceAlongGeometry": 100,
  "primary": {
    "type": "turn",
    "modifier": "left",
    "text": "I 495 North / I 95",
    "components": [
      {
        "text": "I 495",
        "imageBaseURL": "https://s3.amazonaws.com/mapbox/shields/v3/i-495",
        "type": "icon"
      },
      {
        "text": "North",
        "type": "text",
        "abbr": "N",
        "abbr_priority": 0
      },
      {
        "text": "/",
        "type": "delimiter"
      },
      {
        "text": "I 95",
        "imageBaseURL": "https://s3.amazonaws.com/mapbox/shields/v3/i-95",
        "type": "icon"
      }
    ]
  },
  "secondary": {
    "type": "turn",
    "modifier": "left",
    "text": "Baltimore / Northern Virginia",
    "components": [
      {
        "text": "Baltimore",
        "type": "text"
      },
      {
        "text": "/",
        "type": "text"
      },
      {
        "text": "Northern Virginia",
        "type": "text"
      }
    ]
  },
  "sub": {
    "text": "",
    "components": [
      {
        "text": "",
        "type": "lane",
        "directions": ["left"],
        "active": true
      },
      {
        "text": "",
        "type": "lane",
        "directions": ["left", "straight"],
        "active": true
      },
      {
        "text": "",
        "type": "lane",
        "directions": ["right"],
        "active": false
      }
    ]
  }
}
```

## Incident object

The incident object is included in the [route leg object](#route-leg-object) when making a `mapbox/driving-traffic` request in cases where there are incidents along the route.

| Property | Type | Description |
| --- | --- | --- |
| `id` | `string` | The unique ID of the incident. |
| `type` | `string` | The type of incident this is. It can be one of several possible incidents including: `accident`, `congestion`, `construction`, `disabled_vehicle`, `lane_restriction`, `mass_transit`, `miscellaneous`, `other_news`, `planned_event`, `road_closure`, `road_hazard`, `weather`. |
| `description` | `string` | A short description of the incident in a human-readable format. |
| `long_description` | `string` | A long description of the incident in a human-readable format. |
| `creation_time` | `string` | The time this incident was last created as an incident on the map in [ISO-8601 format](https://www.iso.org/iso-8601-date-and-time-format.html) (for example, `2020-12-18T15:56:53Z`). This value can change for the same incident between requests. |
| `start_time` | `string` | The time this incident was started or is expected to start in [ISO-8601 format](https://www.iso.org/iso-8601-date-and-time-format.html) (for example, `2020-12-18T15:56:53Z`). |
| `end_time` | `string` | The time this incident ended or is expected to end in [ISO-8601 format](https://www.iso.org/iso-8601-date-and-time-format.html) (for example, `2020-12-18T15:56:53Z`). |
| `impact` | `string` | The impact of the incident on local traffic. Can be one of: `unknown`, `critical`, `major`, `minor`, `low`. |
| `lanes_blocked` | `array` | Lanes that are blocked by the incident. Some examples of values include: `left`, `left center`, `left turn lane`, `center`, `right`, `right center`, `right turn lane`, `hov`. |
| `num_lanes_blocked` | `integer` | The number of items in the `lanes_blocked` array. |
| `congestion` | `object` | Contains information about the amount of congestion on the road around the incident. |
| `congestion.value` | `integer` | A number between `0` and `101` representing the level of congestion caused by the incident. The higher the number, the more congestion there is. A value of `0` means there is no congestion on the road. A value of `100` means that the road is closed. A value of `101` means that no congestion was calculated. |
| `closed` | `boolean` | If this is `true` then the road has been completely closed. |
| `geometry_index_start` | `integer` | The position in the coordinate list where the incident began, relative to the start of the leg it's on. |
| `geometry_index_end` | `integer` | The position in the coordinate list where the incident ended, relative to the start of the leg it's on. |
| `sub_type` | `string` | This could contain additional information about the type of incident. |
| `sub_type_description` | `string` | This could contain detail about the value of the `sub_type` field. |
| `iso_3166_1_alpha2` | `string` | The two-letter [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) code for the country the incident is located in. Example: `"US"`. |
| `iso_3166_1_alpha3` | `string` | The three-letter [ISO 3166-1 alpha-3](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) code for the country the incident is located in. Example: `"USA"`. |
| `affected_road_names` | `array` | List of roads names affected by the incident. Alternate road names are separated by a `/`. The list is ordered from the first affected road to the last one that the incident affects. |
| `south` | number | The incident bounding box south latitude coordinate as float. |
| `west` | number | The incident bounding box west longitude coordinate as float. |
| `north` | number | The incident bounding box north latitude coordinate as float. |
| `east` | number | The incident bounding box east longitude coordinate as float. |

## Instructions languages

The table below shows supported language codes used with the `language` parameter for turn-by-turn instructions. The language parameter will default to `en` (English) if an unsupported language code is used.

| Code | Language | Text | Voice |
| --- | --- | --- | --- |
| `ar` or `ar-AE` | Arabic | ✅ | ✅ |
| `id` or `id-ID` | Bahasa (Indonesia) | ✅ | — |
| `bs` or `bs-BA` | Bosnian | ✅ | * contact sales |
| `bg` or `bg-BG` | Bulgarian | ✅ | * contact sales |
| `yue` or `yue-CN` | Cantonese | ✅ | * contact sales |
| `zh`, `zh-CN` or `cmn-CN` | Chinese (Mandarin,Simplified) | ✅ | ✅ |
| `zh-TW` | Chinese (Mandarin,Traditional) | ✅ | * contact sales |
| `hr-HR` | Croatian | ✅ | — |
| `cs` or `cs-CZ` | Czech | ✅ | * contact sales |
| `da` or `da-DK` | Danish | ✅ | ✅ |
| `nl-BE` | Dutch (Belgium) | ✅ | ✅ |
| `nl` or `nl-NL` | Dutch (Holland) | ✅ | ✅ |
| `en-AU` | English (Australia) | `en-GB` | ✅ |
| `en-GB-WLS` | English (Welsh) | `en-GB` | `en-GB` |
| `en-GB` | English (UK) | ✅ | ✅ |
| `en-IN` | English (India) | `en-GB` | ✅ |
| `en-SG` | English (Singapore) | ✅ | — |
| `en` or `en-US` | English (United States) | ✅ | ✅ |
| `et` or `et-EE` | Estonian | ✅ | — |
| `fi` or `fi-FI` | Finnish | ✅ | * contact sales |
| `fr-CA` | French (Canada) | ✅ | ✅ |
| `fr` or `fr-FR` | French (France) | ✅ | ✅ |
| `de` or `de-DE` | German (Germany) | ✅ | ✅ |
| `de-CH` | German (Switzerland) | `de` or `de-DE` | `de` or `de-DE` |
| `el` or `el-GR` | Greek (Greece) | ✅ | * contact sales |
| `he` or `he-IL` | Hebrew | ✅ | — |
| `hu` or `hu-HU` | Hungarian | ✅ | * contact sales |
| `is` or `is_IS` | Icelandic | - | ✅ |
| `id` or `id-ID` | Indonesian | ✅ | * contact sales |
| `it` or `it-IT` | Italian | ✅ | ✅ |
| `ja` or `ja-JP` | Japanese | ✅ | ✅ |
| `ko` or `ko-KR` | Korean | ✅ | ✅ |
| `lv-LV` | Latvian | ✅ | * contact sales |
| `lt` or `lt-LT` | Lithuanian | ✅ | * contact sales |
| `ms` or `ms-MY` | Malay (Malaysia) | ✅ | * contact sales |
| `no-NO` | Norwegian | - | ✅ |
| `nb-NO` | Norwegian (Bokmål) | ✅ | ✅ |
| `pl` or `pl-PL` | Polish | ✅ | ✅ |
| `pt-BR` | Portuguese (Brazil) | ✅ | — |
| `pt` or `pt-PT` | Portuguese (Portugal) | ✅ | ✅ |
| `ro` or `ro-RO` | Romanian | ✅ | ✅ |
| `ru` or `ru-RU` | Russian | ✅ | ✅ |
| `sr` or `sr-RS` | Serbian (Cyrillic) | ✅ | * contact sales |
| `sk` or `sk-SK` | Slovak | ✅ | * contact sales |
| `sl` or `sl-SI` | Slovene | ✅ | * contact sales |
| `es-MX` | Spanish (Latin America) | ✅ | ✅ |
| `es` or `es-ES` | Spanish (Spain) | ✅ | ✅ |
| `es-US` | Spanish (United States) | `es-MX` | ✅ |
| `sv` or `sv-SE` | Swedish | ✅ | ✅ |
| `th-TH` | Thai | ✅ | — |
| `tr` or `tr-TR` | Turkish | ✅ | — |
| `uk` or `uk-UA` | Ukrainian | ✅ | — |
| `vi` or `vi-VN` | Vietnamese | ✅ | — |

-   For access, [contact us](https://www.mapbox.com/contact/sales/) for more information. Premium voices available for `de-de`, `nl-nl`, and `yue-cn`. To enable, [contact us](https://www.mapbox.com/contact/sales/)

Use of the underscore as a language code separator is supported too, like `nl_NL`. The `language` parameter is case-insensitive.

## Notification object

| Property | Type | Description |
| --- | --- | --- |
| `notifications.type` | `string` | The type of notifications. Notification types supported are `violation` and `alert`. `violation` is more severe in nature and are delivered when user-set parameters (ex: `exclude=unpaved`) are violated during route generation. The severity of `alert` is less, and is intended to inform users when implicit preferences cannot be satisfied (ex: `countryBorderCrossing`). Available values are in the tables below. |
| `notifications.subtype` | `string` | The optional subtype of notification. Available values are displayed in the tables below. |
| `notifications.refresh_type` | `string` | The refresh type of notification to distinguish between static and dynamic notifications.
| Possible values | Description |
| --- | --- |
| `static` | A notification that is received with the initial route request or during one of the route refreshes, but is then kept constant and not updated on route refreshes. It persists until arrival. An example is a violation alert, where the condition is unlikely to change. |
| `dynamic` | A notification that is updated and reset with each route refresh. Previous notifications of this type are cleared, and new ones are applied based on the current status. This type of notification is used for information that can change, such as the status of a busy EV station. |

 |
| `notifications.geometry_index` | `integer` | The optional position in the coordinate list where the notification occurred, relative to the start of the leg it's on. |
| `notifications.geometry_index_start` | `integer` | The optional position in the coordinate list where the notification began, relative to the start of the leg it's on. |
| `notifications.geometry_index_end` | `integer` | The optional position in the coordinate list where the notification ended, relative to the start of the leg it's on. |
| `notifications.station_id` | `string` | When notification concerns an EV charging station this field will contain the ID of the station. |
| `notifications.reason` | `string` | The reason why notification was issued. See the `subtype` tables below for a list of possible values. |
| `notifications.details` | `object` | The optional details specific to the notification type and subtype. |
| `notifications.details.requested_value` | `string` | The optional requested value in the request. For example, it is 3 (meters) if we specify `max_width=3` in the request. |
| `notifications.details.actual_value` | `string` | The optional actual value associated with the property of the road. For example, it is 2.5 (meters) if maximum road width is 2.5 meters. |
| `notifications.details.unit` | `string` | The optional unit of measure associated with `details.actual_value` and `details.requested_value`. |
| `notifications.details.message` | `string` | The optional message of the notification. |

If `notifications.geometry_index_start` and `notifications.geometry_index_end` is present, then the notification is related to a range on the leg geometry. If `notifications.geometry_index` is present, then the notification is related to a point on the leg geometry. Otherwise, the notification is not related to the leg geometry. Note that only the pair of `notifications.geometry_index_start` and `notifications.geometry_index_end` **or** `notifications.geometry_index` can be present in a single notification object.

The `notifications.details` object is specific to the notification type and subtype.

### Available violations (`notifications.type=violation`)

`violation` is more severe in nature and are delivered when user-set parameters (ex: `exclude=unpaved`) are violated during route generation.

<table class="mt12"><tbody><tr><th><p><code>Subtype</code></p></th><th>Description</th><th style="min-width:320px">Example</th></tr><tr><td><p><code>maxHeight</code></p></td><td>The height of the vehicle is greater than the allowed height of the road. It is provided when <code>max_height</code> is specified.</td><td><pre><code class="language-json">{ "type": "violation", "subtype": "maxHeight", "refresh_type": "static", "geometry_index_end": 5, "geometry_index_start": 3, "details": { "actual_value": "4.60", "requested_value": "4.70", "unit": "meters", "message": "The height of the vehicle (4.7 meters) is more than the permissible height of travel on the road (4.6 meters) by 0.10 meters." } } </code></pre></td></tr><tr><td><p><code>maxWidth</code></p></td><td>The width of the vehicle is greater than the allowed width of the road. It is provided when <code>max_width</code> is specified.</td><td><pre><code class="language-json">{ "type": "violation", "subtype": "maxWidth", "refresh_type": "static", "geometry_index_end": 2, "geometry_index_start": 1, "details": { "actual_value": "4.69", "requested_value": "5.10", "unit": "meters", "message": "The width of the vehicle (5.1 meters) is more than the permissible width of travel on the road (4.69 meters) by 0.41 meters." } } </code></pre></td></tr><tr><td><p><code>maxWeight</code></p></td><td>The weight of the vehicle is greater than the allowed weight of the road. It is provided when <code>max_weight</code> is specified.</td><td><pre><code class="language-json">{ "type": "violation", "subtype": "maxWeight", "refresh_type": "static", "geometry_index_end": 1, "geometry_index_start": 0, "details": { "actual_value": "4.60", "requested_value": "5.10", "unit": "metric tons", "message": "The weight of the vehicle (5.1 metric tons) is more than the permissible weight of travel on the road (4.6 metric tons) by 0.50 metric tons." } } </code></pre></td></tr><tr><td><p><code>unpaved</code></p></td><td>Unpaved road, while it was explicitly requested to exclude unpaved roads from the route. It is provided when <code>exclude=unpaved</code></td><td><pre><code class="language-json">{ "type": "violation", "subtype": "unpaved", "refresh_type": "static", "geometry_index_end": 3, "geometry_index_start": 2, "details": { "requested_value": "paved", "actual_value": "dirt", "message": "The road has an unpaved surface (dirt)." } } </code></pre></td></tr><tr><td><p><code>tunnel</code></p></td><td>Road with tunnel, while it was explicitly requested to exclude tunnel roads from the route. It is provided when <code>exclude=tunnel</code></td><td><pre><code class="language-json">{ "type": "violation", "subtype": "tunnel", "refresh_type": "static", "geometry_index_end": 3, "geometry_index_start": 2, "details": { "message": "Tunnel avoidance not possible - the road leads through a tunnel." } } </code></pre></td></tr><tr><td><p><code>toll</code></p></td><td>Toll road, while it was explicitly requested to exclude toll roads from the route. It is provided when <code>exclude=toll</code>.</td><td><pre><code class="language-json">{ "type": "violation", "subtype": "toll", "refresh_type": "static", "geometry_index_end": 3, "geometry_index_start": 2, "details": { "message": "Toll avoidance not possible - the route uses a toll road." } } </code></pre></td></tr><tr><td><p><code>motorway</code></p></td><td>Motorway, while it was explicitly requested to exclude motorways from the route. It is provided when <code>exclude=motorway</code>.</td><td><pre><code class="language-json">{ "type": "violation", "subtype": "motorway", "refresh_type": "static", "geometry_index_end": 44, "geometry_index_start": 0, "details": { "message": "Motorway avoidance not possible - the route uses a motorway." } } </code></pre></td></tr><tr><td><p><code>pointExclusion</code></p></td><td>Undesirable road, while it was explicitly requested to exclude a road from the route by point. It is provided when <code>exclude=point(longitude latitude)</code>.</td><td><pre><code class="language-json">{ "type": "violation", "subtype": "pointExclusion", "refresh_type": "static", "geometry_index_end": 3, "geometry_index_start": 2, "details": { "message": "The road was marked as undesirable for routing due to the exclusion by point[1.004941,0.996407]." } } </code></pre></td></tr><tr><td><p><code>countryBorderCrossing</code></p></td><td>Indicates a country border crossing. It is provided when <code>exclude=country_border</code>.</td><td><pre><code class="language-json">{ "type": "violation", "subtype": "countryBorderCrossing", "refresh_type": "static", "geometry_index_end": 1, "geometry_index_start": 0, "details": { "actual_value": "US,CA", "message": "Crossing the border of the countries of US and CA." } } </code></pre></td></tr><tr><td><p><code>stateBorderCrossing</code></p></td><td>Indicates a state border crossing. It is provided when <code>exclude=state_border</code>.</td><td><pre><code class="language-json">{ "type": "violation", "subtype": "stateBorderCrossing", "refresh_type": "static", "geometry_index_end": 1, "geometry_index_start": 0, "details": { "actual_value": "US-NV,US-CA", "message": "Crossing the border of the states of US-NV and US-CA." } } </code></pre></td></tr><tr><td><p><code>evMinChargeAtChargingStation</code></p></td><td>Only applicable with <code>engine=electric</code>. EV's battery charge level was less than requested at a charging station. See <a href="#parameters-for-electric-vehicle-routing"><code>ev_min_charge_at_charging_station</code> EV routing parameter.</a></td><td><pre><code class="language-json">{ "type": "violation", "subtype": "evMinChargeAtChargingStation", "refresh_type": "dynamic", "details": { "requested_value": 30000, "actual_value": 27000, "unit": "Wh" } } </code></pre></td></tr><tr><td><p><code>evMinChargeAtDestination</code></p></td><td>Only applicable with <code>engine=electric</code>. EV's battery charge level was less than requested at the destination. See <a href="#parameters-for-electric-vehicle-routing"><code>ev_min_charge_at_destination</code> EV routing parameter.</a></td><td><pre><code class="language-json">{ "type": "violation", "subtype": "evMinChargeAtDestination", "refresh_type": "dynamic", "details": { "requested_value": 20000, "actual_value": 13000, "unit": "Wh" } } </code></pre></td></tr></tbody></table>

### Available alerts (`notifications.type=alert`)

The severity of `alert` is less, and is intended to inform users when implicit preferences cannot be satisfied (ex: `countryBorderCrossing`).

<table class="mt12"><tbody><tr><th><p><code>Subtype</code></p></th><th>Description</th><th style="min-width:320px">Example</th></tr><tr><td><p><code>countryBorderCrossing</code></p></td><td>Indicates a country border crossing.</td><td><pre><code class="language-json">{ "type": "alert", "subtype": "countryBorderCrossing", "refresh_type": "static", "geometry_index_end": 1, "geometry_index_start": 0, "details": { "actual_value": "US,CA", "message": "Crossing the border of the countries of US and CA." } } </code></pre></td></tr><tr><td><p><code>stateBorderCrossing</code></p></td><td>Indicates a state border crossing.</td><td><pre><code class="language-json">{ "type": "alert", "subtype": "stateBorderCrossing", "refresh_type": "static", "geometry_index_end": 1, "geometry_index_start": 0, "details": { "actual_value": "US-NV,US-CA", "message": "Crossing the border of the states of NV and CA." } } </code></pre></td></tr><tr><td><p><code>tunnel</code></p></td><td>Indicates a tunnel.</td><td><pre><code class="language-json">{ "type": "alert", "subtype": "tunnel", "refresh_type": "static", "geometry_index_end": 1, "geometry_index_start": 0, "details": { "message": "The road leads through a tunnel." } } </code></pre></td></tr><tr><td><p><code>toll</code></p></td><td>Indicates a toll road.</td><td><pre><code class="language-json">{ "type": "alert", "subtype": "toll", "refresh_type": "static", "geometry_index_end": 1, "geometry_index_start": 0, "details": { "message": "The route uses a toll road." } } </code></pre></td></tr><tr><td><p><code>evInsufficientCharge</code></p></td><td>Only applicable with <code>engine=electric</code>. EV's battery charge level transitioned from a positive value to zero or less for the first time on a leg. Note that if a leg starts with zero charge level the notification <em>is not emitted</em>.</td><td><pre><code class="language-json">{ "type": "alert", "subtype": "evInsufficientCharge", "refresh_type": "dynamic", "geometry_index": 3 } </code></pre></td></tr><tr><td><p><code>stationUnavailable</code></p></td><td>Only applicable with <code>engine=electric</code>. The station offered during trip planning became unavailable. The reason for availability change is provided in field <code>reason</code> and can be either <code>outOfOrder</code> (when station broke down) or <code>occupied</code> (when there are no available chargers at the station).</td><td><pre><code class="language-json">{ "type": "alert", "subtype": "stationUnavailable", "refresh_type": "dynamic", "reason": "outOfOrder", "station_id": "station-1", } </code></pre></td></tr></tbody></table>

## Directions API errors

On error, the server responds with different HTTP status codes:

-   For responses with HTTP status codes lower than `500`, the JSON response body includes the `code` property, which may be used by client programs to manage control flow. The response body may also include a `message` property with a human-readable explanation of the error.
-   If a server error occurs, the HTTP status code will be `500` or higher and the response will not include a `code` property.

<table><thead><tr><th>Response body <code>code</code></th><th>HTTP status code</th><th>Description</th></tr></thead><tbody><tr><td><code>Ok</code></td><td><code>200</code></td><td>Normal success case.</td></tr><tr><td><code>NoRoute</code></td><td><code>200</code></td><td>There was no route found for the given coordinates. Check for impossible routes (for example, routes over oceans without ferry connections).</td></tr><tr><td><code>NoSegment</code></td><td><code>200</code></td><td>No road segment could be matched for one or more coordinates within the supplied <code>radiuses</code>. Check for coordinates that are too far away from a road.</td></tr><tr><td><code>Not Authorized - No Token</code></td><td><code>401</code></td><td>No token was used in the query.</td></tr><tr><td><code>Not Authorized - Invalid Token</code></td><td><code>401</code></td><td>Check the access token you used in the query.</td></tr><tr><td><code>Forbidden</code></td><td><code>403</code></td><td>There may be an issue with your account. Check your <a href="https://console.mapbox.com/">Account page</a> for more details.<br><br>In some cases, using an access tokens with URL restrictions can also result in a <code>403</code> error. For more information, see our <a href="https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions">Token management guide</a>.</td></tr><tr><td><code>ProfileNotFound</code></td><td><code>404</code></td><td>Use a valid profile as described in the <a href="/navigation/directions/#routing-profiles">list of routing profiles</a>.</td></tr><tr><td><code>InvalidInput</code></td><td><code>422</code></td><td>The given request was not valid. The <code>message</code> key of the response will hold an explanation of the invalid input.</td></tr></tbody></table>

## Directions API restrictions and limits

-   Requests using the `driving`, `driving-traffic`, `walking`, and `cycling` profiles can specify up to 25 total waypoints (input coordinates that are snapped to the roads network) along the route. But, if using `engine=electric`, you can specify up to a total of 12 waypoints.
    -   Traffic coverage for the `driving-traffic` profile is available in [supported geographies](https://docs.mapbox.com/help/getting-started/directions/#traffic-data). Requests to this profile revert to `driving` profile results for areas without traffic coverage.
-   Maximum 300 requests per minute.
-   Requests that contain multiple coordinates are counted as 1 request.
-   Maximum total distance between all waypoints:
    -   10,000 kilometers for the `driving` and `driving-traffic` profiles.
    -   1,500 kilometers for the `cycling` profile.
    -   1,000 kilometers for the `walking` profile.
-   Inter-continental route requests are not supported.
-   Route requests through water bodies are not supported.

If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).

## Directions API pricing

-   Billed by **requests**
-   See rates and discounts per Directions API request in the pricing page's **[Navigation](https://www.mapbox.com/pricing/#directions)** section

Usage of the Directions API is measured in **API requests**. A request that contains multiple waypoints is billed as a single API request. Details about the number of Directions API requests included in the free tier and the cost per request beyond what is included in the free tier are available on the [pricing page](https://www.mapbox.com/pricing/#directions).