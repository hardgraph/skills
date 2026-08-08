# Static Images API

The **Mapbox Static Images API** serves standalone, static map images generated from Mapbox Studio styles. These images can be displayed on web and mobile devices without the aid of a mapping library or API. They look like an embedded map, but do not have interactivity or controls.

> **Related content (playground): [Static Images API playground](https://docs.mapbox.com/playground/static/)**
> 
> Build a Static Images API request by zooming and panning around an interactive map.

> **Note (warning): Projections other than Mercator in the Static Images API**
> 
> All map images returned by the Static Images API are projected in [Web Mercator](https://docs.mapbox.com/mapbox-gl-js/guides/projections/#mercator). Other projections are not supported.

The **Mapbox Static Images API** is not compatible with the [Mapbox Standard](https://docs.mapbox.com/map-styles/guides/standard-styles/) and Mapbox Standard Satellite styles. Custom styles that import either of these styles are not supported.

Support for the Mapbox Standard and Mapbox Standard Satellite styles is planned for a future release.

## Retrieve a static map from a style

**GET** : `https://api.mapbox.com/styles/v1/{username}/{style_id}/static/{overlay}/{lon},{lat},{zoom},{bearing},{pitch}|{bbox}|{auto}/{width}x{height}{@2x}{.format}` [Token scope: **styles:tiles**]

The position of the map is represented by either the word `auto`, a [bounding box](https://docs.mapbox.com/help/glossary/bounding-box/), or by five numbers: longitude, latitude, zoom, bearing, and pitch. The last two numbers, bearing and pitch, are optional. If you only specify bearing and not pitch, pitch will default to `0`. If you specify neither, they will both default to `0`. If you specify `auto` or `bbox`, you should not provide any of these numbers.

You can use the `format` parameter to specify output image format, otherwise it will be defined implicitly, [depending on the used sources](#response-retrieve-a-static-map-from-a-style).

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the style belongs. |
| `style_id` | `string` | The ID of the style from which to create a static map. |
| `overlay` | `string` | One or more comma-separated features that can be applied on top of the map at request time. The order of features in an overlay dictates their Z-order on the page. The last item in the list will have the highest Z-order (will overlap the other features in the list), and the first item in the list will have the lowest (will rest beneath the other features). Format can be a mix of `geojson`, `marker`, or `path`. For more details on each option, see the [Overlay options section](#overlay-options). |
| `lon` | `number` | Longitude for the center point of the static map; a number between `-180` and `180`. |
| `lat` | `number` | Latitude for the center point of the static map; a number between `-85.0511` and `85.0511`. |
| `zoom` | `number` | Zoom level; a number between `0` and `22`. Fractional zoom levels will be rounded to two decimal places. |
| `bbox` | `array` | Four coordinates representing upper longitude, upper latitude, lower longitude, lower latitude, enclosed in square brackets like `[lon(min),lat(min),lon(max),lat(max)]`. `bbox` is used in place of `lon,lat,zoom` or `auto`. The zoom level is calculated based on the most detailed zoom level that fits the bounding box within the request's specified width and height. Increasing the request's width and height will return a map at a higher zoom level. See the [`bbox` example request](#example-request-retrieve-a-static-map-using-a-bounding-box) for how to retrieve a static map using a bounding box. |
| `auto` | `string` | If `auto` is used, the viewport will fit the bounds of the overlay. If used, `auto` replaces `lon`, `lat`, `zoom`, `bearing`, `pitch`, and `bbox`. If `auto` is used without a specified `padding` value, padding will automatically be applied with a value that is 5% of the smallest side of the image, rounded up to the next integer value, up to a maximum of 12 pixels of padding per side. |
| `width` | `number` | Width of the image; a number between `1` and `1280` pixels. |
| `height` | `number` | Height of the image; a number between `1` and `1280` pixels. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `styles:tiles` scope. |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `bearing` | `number` | Bearing rotates the map around its center. A number between `0` and `360`, interpreted as decimal degrees. 90 rotates the map 90° clockwise, while 180 flips the map. Defaults to `0`. |
| `pitch` | `number` | Pitch tilts the map, producing a perspective effect. A number between `0` and `60`, measured in degrees. Defaults to `0` (looking straight down at the map). |
| `@2x` | `string` | Render the static map at a `@2x` scale factor for high-density displays. |
| `attribution` | `boolean` | Controls whether there is attribution on the image. Defaults to `true`. **Note:** If `attribution=false`, the watermarked attribution is removed from the image. You still have a legal responsibility to attribute maps that use OpenStreetMap data, which includes most maps from Mapbox. If you specify `attribution=false`, you are legally required to [include proper attribution elsewhere on the webpage or document](https://docs.mapbox.com/help/dive-deeper/attribution/#static--print). |
| `logo` | `boolean` | Controls whether there is a Mapbox logo on the image. Defaults to `true`. |
| `before_layer` | `string` | Controls where the `overlay` is inserted in the style. All overlays will be inserted before the specified layer. |
| `addlayer` | `object` | Adds a [Mapbox style layer](https://docs.mapbox.com/style-spec/reference/layers/) to the map's style at render time. Can be combined with `before_layer`. See [Style Parameters](#style-parameters) for more information. |
| `setfilter` | `array` | Applies a filter to an existing layer in a style using Mapbox's [expression](https://docs.mapbox.com/style-spec/reference/expressions/) syntax. Must be used with `layer_id`. See [Style Parameters](#style-parameters) for more information. |
| `layer_id` | `string` | Denotes the layer in the style that the filter specified in `setfilter` is applied to. |
| `padding` | `string` | Denotes the minimum padding per side of the image. This can only be used with `auto` or `bbox`. The value resembles the [CSS specification](https://developer.mozilla.org/en-US/docs/Web/CSS/padding) for padding and accepts 1-4 integers without units. For example, `padding=5` declares a minimum padding of 5 pixels for all sides, where as `padding=5,8,10,7` declares a minimum of 5 pixels of top padding, 8 pixels of right padding, 10 pixels of bottom padding, and 7 pixels of left padding. If `auto` is used but no value is specified in `padding`, the default padding will be used (a value that is 5% of the smallest side of the image, rounded up to the next integer value, up to a maximum of 12 pixels of padding per side). |
| `format` | `string` | Format of the output image. `png`, `jpeg` and `webp` values are supported. If not specified the image format will be defined implicitly [depending on the used sources](#response-retrieve-a-static-map-from-a-style) |

> **Related content (playground): [Location Helper](https://labs.mapbox.com/location-helper)**
> 
> To experiment with camera pitch, bearing, tilt, and zoom and get values to use in your code, try our *Location Helper* tool.

### Example requests

#### Example requests: Retrieve a static map from a style

```bash
# Retrieve a map at -122.4241 longitude, 37.78 latitude,
# zoom 15.25, bearing 0, and pitch 60. The map
# will be 400 pixels wide and 400 pixels high
# and save the output as a PNG image.
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/-122.4241,37.78,15.25,0,60/400x400?access_token=YOUR_MAPBOX_ACCESS_TOKEN" --output example-mapbox-static-1.png

# Retrieve a map at 0 longitude, 10 latitude, zoom 3,
# and bearing 20. Pitch will default to 0
# and save the output as a PNG image.
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/0,10,3,20/600x600?access_token=YOUR_MAPBOX_ACCESS_TOKEN" --output example-mapbox-static-2.png

# Retrieve a WebP map at 0 longitude, 0 latitude, zoom 2.
# Bearing and pitch default to 0
# and save the output as a WebP image.
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/0,0,2/600x600.webp?access_token=YOUR_MAPBOX_ACCESS_TOKEN" --output example-mapbox-static-3.webp

# Querying a style with raster layers returns a JPEG
# and save the output as a JPEG image
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/satellite-v9/static/0,0,2/600x600?access_token=YOUR_MAPBOX_ACCESS_TOKEN" --output example-mapbox-static-4.jpg
```

#### Example request: Retrieve a static map using a bounding box

Retrieve a map that fits the bounding box `[-77.043686,38.892035,-77.028923,38.904192]` and save the output as a PNG image that is 400 pixels wide and 400 pixels high.

```bash
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/[-77.043686,38.892035,-77.028923,38.904192]/400x400?access_token=YOUR_MAPBOX_ACCESS_TOKEN" --output example-mapbox-static-bbox-1.png
```

#### Example response: Retrieve a static map using a bounding box

![Static map image of the White House on Pennsylvania Avenue in Washington, D.C.](https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/[-77.043686,38.892035,-77.028923,38.904192]/400x400?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)[View static image](https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/[-77.043686,38.892035,-77.028923,38.904192]/400x400?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)

#### Example request: Retrieve a static map using a bounding box with padding

The `bbox` parameter can be combined with padding as well. Using the same bounding box from the previous example, this request adds 50 pixels of top padding, 10 pixels of side padding, and 20 pixels of bottom padding.

```bash
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/[-77.043686,38.892035,-77.028923,38.904192]/400x400?padding=50,10,20&access_token=YOUR_MAPBOX_ACCESS_TOKEN" --output example-mapbox-static-bbox-2.png
```

#### Example response: Retrieve a static map using a bounding box with padding

![Static map image of the White House on Pennsylvania Avenue, and surrounding streets, in Washington, D.C.](https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/[-77.043686,38.892035,-77.028923,38.904192]/400x400?padding=50,10,20&access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)[View static image](https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/[-77.043686,38.892035,-77.028923,38.904192]/400x400?padding=50,10,20&access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)

#### Example request: Retrieve a static map image in an HTML file

```html
{/* Retrieve a map at -87.0186 longitude, 32.4055 latitude, */} {/* zoom 14,
bearing 0. Pitch will default to 0. */} {/* The map will be 500 pixels wide and
300 pixels high. */}
<img
  src="https://api.mapbox.com/styles/v1/mapbox/light-v11/static/-87.0186,32.4055,14/500x300?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
  alt="Map of the Edmund Pettus Bridge in Selma, Alabama, with a black 'L' marker positioned in the middle of the bridge."
/>
```

#### Response: Retrieve a static map from a style

-   For styles that contain *vector layers*, the returned static map will be a PNG.
-   For styles that contain *only raster layers*, the returned static map will be a JPEG.

### Supported libraries: Retrieve a static map from a style

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Python CLI](https://github.com/mapbox/mapbox-cli-py#staticmap)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#getstaticimage)
-   [Mapbox Java SDK](https://docs.mapbox.com/android/java/api/libjava-services/5.8.0/com/mapbox/api/staticmap/v1/package-summary.html)
-   [MapboxStatic.swift](https://github.com/mapbox/MapboxStatic.swift) (Objective-C and Swift)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Overlay options

You can use overlay options to add markers, custom images, or other shapes on top of a static map image at the time of request.

For some parameters, the Static images API requires that values be encoded as URI components. You can encode those values using [`encodeURIComponent()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent#description) or other encoding tools.

### GeoJSON

```
geojson({geojson})
```

| Argument | Type | Description |
| --- | --- | --- |
| `geojson` | `object` | The `{geojson}` argument must be a valid GeoJSON object encoded as a URI component. [simplestyle-spec](https://github.com/mapbox/simplestyle-spec) styles for GeoJSON features will be respected and rendered. Note that `#` characters must be replaced in the URL with `%23` (for example, `"fill":"#FF0000"` would be `"fill":"%23FF0000"`). |

#### Troubleshooting: Shorten Static Images API requests

The Static Images API only accepts requests that are [**8,192 or fewer characters long**](https://docs.mapbox.com/api/guides/#url-length-limits). Using a large GeoJSON object as an argument to the `overlay` parameter will lengthen your request URL. If you find yourself brushing up against this limit, there are a several optimizations that can be used to reduce the length of your Static Images API requests:

**1. Use a third-party tool for simplification.** You can simplify your GeoJSON using a third party library, such as [simplify-geojson](https://www.npmjs.com/package/simplify-geojson), before passing it as an argument.

**2. Create a custom style in Studio.** Add a large GeoJSON polygon or other object to a custom [style](https://docs.mapbox.com/studio-manual/reference/styles/) in [Mapbox Studio](https://console.mapbox.com/studio/), then passing the [style ID](https://docs.mapbox.com/help/glossary/style-id/) to the Static Images API `style_id` parameter instead of passing the GeoJSON to the `overlay` parameter. See [Part 1 of our Make a choropleth map tutorial](https://docs.mapbox.com/help/tutorials/choropleth-studio-gl-pt-1/) for guidance.

**3. Remove redundant coordinates in encoded path overlays.** On web mercator maps, straight line features require only two coordinates to be rendered correctly. If your request includes path overlays that were encoded with a large amount of points along a straight line, you can reduce the length of your encoded path by removing intermediary coordinates. For example, the line `[[0,52], [0,51], [0,50],[0,49]]` can be simplified to `[[0,52], [0,49]]` and still achieve the same visual result.

**4. Use GeoJSON for multiple marker overlays.** If your request includes several marker overlays that all use the same icon, you can remove the redundancy of specifying each `pin-*` or `url-*` overlay by using a GeoJSON overlay that leverages a `MultiPoint` feature and the [`simplestyle-spec` GeoJSON styling convention](https://github.com/mapbox/simplestyle-spec) instead.

For example, if you have the following path overlay:

```bash
pin-s-airport+0000FF(1,2),pin-s-airport+0000FF(2,1),pin-s-airport+0000FF(3,2),pin-s-airport+0000FF(1,3)
```

You can instead add a GeoJSON overlay with the following data:

```json
{
  "type": "Feature",
  "properties": {
    "marker-size": "small",
    "marker-symbol": "airport",
    "marker-color": "#0000FF"
  },
  "geometry": {
    "type": "MultiPoint",
    "coordinates": [
      [1, 2],
      [2, 1],
      [3, 2],
      [1, 3]
    ]
  }
}
```

And still achieve the same visual result.

If you are reusing a custom marker for multiple points, you can include the custom URL as the `"marker-url"` property in your GeoJSON overlay.

**5. Reduce coordinate precision.** In web applications it is common to use `latitude` and `longitude` values with high amounts of decimal precision. But it is rarely necessary to use more than six decimals of precision for Static Images API requests. Rounding any coordinate values in your request to the sixth decimal will help trim down long URLs.

**6. Remove feature properties, if relying on defaults.** For overlays, the Static Images API will set a default value for the feature if no value is provided for certain `simplestyle` properties. If you are already using the default value or are comfortable relying on the default values, removing the explicit declaration of these style properties in your overlays can help reduce URL length:

| Property | Default value |
| --- | --- |
| `marker-color` | `#7e7e7e` |
| `stroke` | `#555555` |
| `stroke-width` | `2` |
| `stroke-opacity` | `1.0` |
| `fill` | `#555555` |
| `fill-opacity` | `0.6` |
| `marker-size` | `medium` |

#### Example request: Retrieve a static map with a GeoJSON `Point` overlay

```bash
# Retrieve a static map with a GeoJSON overlay
# and save the output as a PNG image
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/geojson(%7B%22type%22%3A%22Point%22%2C%22coordinates%22%3A%5B-73.99%2C40.7%5D%7D)/-73.99,40.70,12/500x300?access_token=YOUR_MAPBOX_ACCESS_TOKEN" --output example-mapbox-static-overlay-GeoJSON.png
```

#### Example response: Retrieve a static map with a GeoJSON `Point` overlay

![Static image of a map using the Streets style, with one default marker overlay.](https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/geojson(%7B%22type%22%3A%22Point%22%2C%22coordinates%22%3A%5B-73.99%2C40.7%5D%7D)/-73.99,40.70,12/500x300?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)[View static image](https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/geojson(%7B%22type%22%3A%22Point%22%2C%22coordinates%22%3A%5B-73.99%2C40.7%5D%7D)/-73.99,40.70,12/500x300?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)

#### Example request: Retrieve a static map with a GeoJSON `FeatureCollection` overlay

```bash
# Retrieve a 500×300px static map image with a GeoJSON
# overlay including three colored markers with
# Maki icons and save the output as a PNG image.
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/geojson(%7B%22type%22%3A%22FeatureCollection%22%2C%22features%22%3A%5B%7B%22type%22%3A%22Feature%22%2C%22properties%22%3A%7B%22marker-color%22%3A%22%23462eff%22%2C%22marker-size%22%3A%22medium%22%2C%22marker-symbol%22%3A%22bus%22%7D%2C%22geometry%22%3A%7B%22type%22%3A%22Point%22%2C%22coordinates%22%3A%5B-122.25993633270264,37.80988566878777%5D%7D%7D%2C%7B%22type%22%3A%22Feature%22%2C%22properties%22%3A%7B%22marker-color%22%3A%22%23e99401%22%2C%22marker-size%22%3A%22medium%22%2C%22marker-symbol%22%3A%22park%22%7D%2C%22geometry%22%3A%7B%22type%22%3A%22Point%22%2C%22coordinates%22%3A%5B-122.25916385650635,37.80629162635318%5D%7D%7D%2C%7B%22type%22%3A%22Feature%22%2C%22properties%22%3A%7B%22marker-color%22%3A%22%23d505ff%22%2C%22marker-size%22%3A%22medium%22%2C%22marker-symbol%22%3A%22music%22%7D%2C%22geometry%22%3A%7B%22type%22%3A%22Point%22%2C%22coordinates%22%3A%5B-122.25650310516359,37.8063933469406%5D%7D%7D%5D%7D)/-122.256654,37.804077,13/500x300?access_token=YOUR_MAPBOX_ACCESS_TOKEN" --output example-mapbox-static-overlay-GeoJSON3.png
```

#### Example response: Retrieve a static map with a GeoJSON `FeatureCollection` overlay

![Static image of a map using the Streets style, with 3 multicolored markers with unique Maki icons in different locations in a park.](https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/geojson(%7B%22type%22%3A%22FeatureCollection%22%2C%22features%22%3A%5B%7B%22type%22%3A%22Feature%22%2C%22properties%22%3A%7B%22marker-color%22%3A%22%23462eff%22%2C%22marker-size%22%3A%22medium%22%2C%22marker-symbol%22%3A%22bus%22%7D%2C%22geometry%22%3A%7B%22type%22%3A%22Point%22%2C%22coordinates%22%3A%5B-122.25993633270264,37.80988566878777%5D%7D%7D%2C%7B%22type%22%3A%22Feature%22%2C%22properties%22%3A%7B%22marker-color%22%3A%22%23e99401%22%2C%22marker-size%22%3A%22medium%22%2C%22marker-symbol%22%3A%22park%22%7D%2C%22geometry%22%3A%7B%22type%22%3A%22Point%22%2C%22coordinates%22%3A%5B-122.25916385650635,37.80629162635318%5D%7D%7D%2C%7B%22type%22%3A%22Feature%22%2C%22properties%22%3A%7B%22marker-color%22%3A%22%23d505ff%22%2C%22marker-size%22%3A%22medium%22%2C%22marker-symbol%22%3A%22music%22%7D%2C%22geometry%22%3A%7B%22type%22%3A%22Point%22%2C%22coordinates%22%3A%5B-122.25650310516359,37.8063933469406%5D%7D%7D%5D%7D)/-122.256654,37.804077,13/500x300?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)[View static image](https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/geojson(%7B%22type%22%3A%22FeatureCollection%22%2C%22features%22%3A%5B%7B%22type%22%3A%22Feature%22%2C%22properties%22%3A%7B%22marker-color%22%3A%22%23462eff%22%2C%22marker-size%22%3A%22medium%22%2C%22marker-symbol%22%3A%22bus%22%7D%2C%22geometry%22%3A%7B%22type%22%3A%22Point%22%2C%22coordinates%22%3A%5B-122.25993633270264,37.80988566878777%5D%7D%7D%2C%7B%22type%22%3A%22Feature%22%2C%22properties%22%3A%7B%22marker-color%22%3A%22%23e99401%22%2C%22marker-size%22%3A%22medium%22%2C%22marker-symbol%22%3A%22park%22%7D%2C%22geometry%22%3A%7B%22type%22%3A%22Point%22%2C%22coordinates%22%3A%5B-122.25916385650635,37.80629162635318%5D%7D%7D%2C%7B%22type%22%3A%22Feature%22%2C%22properties%22%3A%7B%22marker-color%22%3A%22%23d505ff%22%2C%22marker-size%22%3A%22medium%22%2C%22marker-symbol%22%3A%22music%22%7D%2C%22geometry%22%3A%7B%22type%22%3A%22Point%22%2C%22coordinates%22%3A%5B-122.25650310516359,37.8063933469406%5D%7D%7D%5D%7D)/-122.256654,37.804077,13/500x300?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)

### Marker

```
{name}-{label}+{color}({lon},{lat})
```

| Argument | Type | Description |
| --- | --- | --- |
| `name` | `string` | Marker shape and size. Options are `pin-s` and `pin-l`. |
| `label` | `string` | *Optional.* Marker symbol. Options are a lowercase alphanumeric label `a` through `z`, `0` through `99`, or a valid [Maki](https://www.mapbox.com/maki-icons/) icon. If a letter is requested, it will be rendered in uppercase only. |
| `color` | `string` | *Optional.* A 3- or 6-digit hexadecimal color code. |
| `lon, lat` | `number` | The location at which to center the marker. When using an asymmetric marker, make sure that the tip of the pin is at the center of the image. |

#### Example request: Retrieve a static map with a marker overlay

```bash
# Retrieve a static map with a marker overlay
# and save the output as a PNG image
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/light-v11/static/pin-s-l+000(-87.0186,32.4055)/-87.0186,32.4055,14/500x300?access_token=YOUR_MAPBOX_ACCESS_TOKEN" --output example-mapbox-static-overlay-marker.png
```

#### Example response: Retrieve a static map with a marker overlay

![Static map image of Edmund Pettus Bridge in Selma, Alabama, using the Light map style, with a black marker with the letter 'L' positioned in the middle of the bridge.](https://api.mapbox.com/styles/v1/mapbox/light-v11/static/pin-s-l+000(-87.0186,32.4055)/-87.0186,32.4055,14/500x300?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)[View static image](https://api.mapbox.com/styles/v1/mapbox/light-v11/static/pin-s-l+000(-87.0186,32.4055)/-87.0186,32.4055,14/500x300?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)

#### Example request: Retrieve a static map with a Maki icon overlay

```bash
# Retrieve a static map with a large marker overlay
# Marker has a Maki icon label and background color `#f74e4e`
# Output is saved as a PNG image
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/dark-v11/static/pin-l-embassy+f74e4e(-74.0021,40.7338)/-74.0021,40.7338,16/500x300?access_token=YOUR_MAPBOX_ACCESS_TOKEN" --output example-mapbox-static-overlay-marker-pin-maki-icon.png
```

#### Example response: Retrieve a static map with a Maki icon overlay

![Static map image of Edmund Pettus Bridge in Selma, Alabama, using the Dark map style, with a black marker with the letter 'L' positioned in the middle of the bridge.](https://api.mapbox.com/styles/v1/mapbox/dark-v11/static/pin-l-embassy+f74e4e(-74.0021,40.7338)/-74.0021,40.7338,16/500x300?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)[View static image](https://api.mapbox.com/styles/v1/mapbox/dark-v11/static/pin-l-embassy+f74e4e(-74.0021,40.7338)/-74.0021,40.7338,16/500x300?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)

#### Example request: Retrieve a static map with a marker overlay in an HTML file

```html
{/* Retrieve a static map with marker overlay that is */} {/* a small pin with a
letter 'L' label and black color */}
<img
  src="https://api.mapbox.com/styles/v1/mapbox/light-v11/static/pin-s-l+000(-87.0186,32.4055)/-87.0186,32.4055,14/500x300?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
  alt="Static map image of Edmund Pettus Bridge in Selma, Alabama, using the Dark map style, with a black marker with the letter 'L' positioned in the middle of the bridge."
/>
```

### Custom marker

```bash
url-{url}({lon},{lat})
```

| Argument | Type | Description |
| --- | --- | --- |
| `url` | `string` | A percent-encoded URL for the image. Type can be `PNG` or `JPG`. |
| `lon, lat` | `number` | The location at which to center the marker. When creating an asymmetric marker like a pin, make sure that the tip of the pin is at the center of the image. |

Custom marker overlays are not automatically scaled or modified if used simultaneously with `@2x`, so you must make sure that the image used as a custom marker is the appropriate size before making the request. The height and width of images used as custom marker overlays cannot exceed 1,024 pixels.

Custom markers are cached according to the `Expires` and `Cache-Control` headers. Set at least one of these headers to a proper value to prevent repeated requests for the custom marker image.

You must declare the type value for custom markers using the `Content-Type` key in the response header of the marker URL request. Acceptable values are `image/png` and `image/jpeg`.

#### Example request: Retrieve a static map with a custom marker overlay

```bash
# Retrieve a map with a custom marker overlay
# and save the output as a PNG image
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/light-v11/static/url-https%3A%2F%2Fdocs.mapbox.com%2Fapi%2Fimg%2Fcustom-marker.png(-76.9,38.9)/-76.9,38.9,15/500x300?access_token=YOUR_MAPBOX_ACCESS_TOKEN" --output example-mapbox-static-overlay-marker-custom.png
```

#### Example response: Retrieve a static map with a custom marker overlay

![Static image of a map using the Light map style with a blue teardrop-shaped custom marker image inside a polygon labeled Seat Pleasant Park.](https://api.mapbox.com/styles/v1/mapbox/light-v11/static/url-https%3A%2F%2Fdocs.mapbox.com%2Fapi%2Fimg%2Fcustom-marker.png(-76.9,38.9)/-76.9,38.9,15/500x300?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)[View static image](https://api.mapbox.com/styles/v1/mapbox/light-v11/static/url-https%3A%2F%2Fdocs.mapbox.com%2Fapi%2Fimg%2Fcustom-marker.png(-76.9,38.9)/-76.9,38.9,15/500x300?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)

### Path

```bash
path-{strokeWidth}+{strokeColor}-{strokeOpacity}+{fillColor}-{fillOpacity}({polyline})
```

[Encoded polylines](https://developers.google.com/maps/documentation/utilities/polylinealgorithm) with a precision of 5 decimal places can be used with the Static API via the `path` parameter.

| Argument | Type | Description |
| --- | --- | --- |
| `strokeWidth` | `number` | *Optional.* A positive number for the line stroke width |
| `strokeColor` | `string` | *Optional.* A 3- or 6-digit hexadecimal color code for the line stroke |
| `strokeOpacity` | `number` | *Optional.* A number between `0` (transparent) and `1` (opaque) for line stroke opacity |
| `fillColor` | `string` | *Optional.* A 3- or 6-digit hexadecimal color code for the fill |
| `fillOpacity` | `number` | *Optional.* A number between `0` (transparent) and `1` (opaque) for fill opacity |
| `polyline` | `string` | A valid encoded polyline encoded as a URI component |

#### Example request: Retrieve a static map with a path overlay

```bash
# Retrieve a map with two points and a polyline overlay,
# with its center point automatically determined with `auto`
# and save the output as a PNG image
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/pin-s-a+9ed4bd(-122.46589,37.77343),pin-s-b+000(-122.42816,37.75965),path-5+f44-0.5(%7DrpeFxbnjVsFwdAvr@cHgFor@jEmAlFmEMwM_FuItCkOi@wc@bg@wBSgM)/auto/500x300?access_token=YOUR_MAPBOX_ACCESS_TOKEN" --output example-mapbox-static-overlay-polyline.png
```

#### Example response: Retrieve a static map with a path overlay

![Static map image of a red path overlay between two points labeled 'A' and 'B'.](https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/pin-s-a+9ed4bd(-122.46589,37.77343),pin-s-b+000(-122.42816,37.75965),path-5+f44-0.5(%7DrpeFxbnjVsFwdAvr@cHgFor@jEmAlFmEMwM_FuItCkOi@wc@bg@wBSgM)/auto/500x300?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)[View static image](https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/pin-s-a+9ed4bd(-122.46589,37.77343),pin-s-b+000(-122.42816,37.75965),path-5+f44-0.5(%7DrpeFxbnjVsFwdAvr@cHgFor@jEmAlFmEMwM_FuItCkOi@wc@bg@wBSgM)/auto/500x300?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)

## Style parameters

Style parameters allow you to change the requested map style at render time.

Style parameter requests must specify a `lon`, `lat`, and `zoom` in the request. Using the `auto` extent will result in an error if the request does not also include an overlay.

### Add a style layer

You can add a style layer to your static image request by using the `addlayer` query parameter. The `addlayer` query parameter accepts a [Mapbox style layer](https://docs.mapbox.com/style-spec/reference/layers/) to add a new layer to the rendered map. The layer you add must be from a Mapbox source or an existing source in your account.

To use the `addlayer` parameter, format your request like:

```bash
# Add a road overlay layer from the existing composite source
# The hex code for "line-color" is URI encoded
https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/-122,37,9/600x600?access_token=YOUR_MAPBOX_ACCESS_TOKEN&addlayer={"id":"road-overlay","type":"line","source":"composite","source-layer":"road","filter":["==",["get","class"],"motorway"],"paint":{"line-color":"%23ff0000","line-width":5}}
```

To specify the z-index of the new layer by using the `before_layer` query parameter. For example, the following request will place the new layer `road-overlay` before `road-label`:

```bash
# Add a road overlay layer from the existing composite source
# The hex code for "line-color" is URI encoded
https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/-122,37,9/600x600?access_token=YOUR_MAPBOX_ACCESS_TOKEN&addlayer={"id":"road-overlay","type":"line","source":"composite","source-layer":"road","filter":["==",["get","class"],"motorway"],"paint":{"line-color":"%23ff0000","line-width":5}}&before_layer=road-label
```

> **Note: Overlays and before_layer priority**
> 
> When `addlayer` is used in conjunction with an overlay *and* a `before_layer` is specified, the layer defined in the `addlayer` parameter will always be inserted beneath the indicated layer and the overlay will be added on top of the map.

To add a style layer from a source that is not already part of the style, you must specify `source.url` and `source.type` in the new style layer, like:

```bash
"source": {
  "type": "vector",
  "url": "mapbox://{username}.{tileset}"
}
```

`source.url` must be a `mapbox://` tileset URL, and must be accessible to the access token used in the request. `source.type` must be either `raster` or `vector`. If you want to add an `image` or `geojson` as a new layer, you should use their respective [overlay options](#overlay-options).

#### Example request: Retrieve a static map with an added style layer

```bash
# Add a layer called "better-boundary" using data from the source layer
# `admin`, filtered to include only state boundaries, matching the U.S.
# worldview, and style the resulting lines red (#DB6936) with a dashed line.
# Special characters in the request are encoded.
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/light-v11/static/-96.561208,38.790325,3/800x400@2x?addlayer={%22id%22:%22better-boundary%22,%22type%22:%22line%22,%22source%22:%22composite%22,%22source-layer%22:%22admin%22,%22filter%22:[%22all%22,[%22==%22,[%22get%22,%22admin_level%22],1],[%22==%22,[%22get%22,%22maritime%22],%22false%22],[%22match%22,[%22get%22,%22worldview%22],[%22all%22,%22US%22],true,false],[%22==%22,[%22get%22,%22iso_3166_1%22],%22US%22]],%22layout%22:{%22line-join%22:%22bevel%22},%22paint%22:{%22line-color%22:%22%23DB6936%22,%22line-width%22:1.5,%22line-dasharray%22:[1.5,1]}}&before_layer=road-label&access_token=YOUR_MAPBOX_ACCESS_TOKEN" --output example-mapbox-static-style-parameters-addlayer.png
```

#### Example response: Retrieve a static map with an added style layer

![Light gray map of the United States with state boundaries drawn in red dashed lines.](https://api.mapbox.com/styles/v1/mapbox/light-v11/static/-96.561208,38.790325,3/800x400@2x?addlayer=%7B%22id%22:%22better-boundary%22,%22type%22:%22line%22,%22source%22:%22composite%22,%22source-layer%22:%22admin%22,%22filter%22:[%22all%22,[%22==%22,[%22get%22,%22admin_level%22],1],[%22==%22,[%22get%22,%22maritime%22],%22false%22],[%22match%22,[%22get%22,%22worldview%22],[%22all%22,%22US%22],true,false],[%22==%22,[%22get%22,%22iso_3166_1%22],%22US%22]],%22layout%22:%7B%22line-join%22:%22bevel%22%7D,%22paint%22:%7B%22line-color%22:%22%23DB6936%22,%22line-width%22:1.5,%22line-dasharray%22:[1.5,1]%7D%7D&before_layer=road-label&access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)[View static image](https://api.mapbox.com/styles/v1/mapbox/light-v11/static/-96.561208,38.790325,3/800x400@2x?addlayer=%7B%22id%22:%22better-boundary%22,%22type%22:%22line%22,%22source%22:%22composite%22,%22source-layer%22:%22admin%22,%22filter%22:[%22all%22,[%22==%22,[%22get%22,%22admin_level%22],1],[%22==%22,[%22get%22,%22maritime%22],%22false%22],[%22match%22,[%22get%22,%22worldview%22],[%22all%22,%22US%22],true,false],[%22==%22,[%22get%22,%22iso_3166_1%22],%22US%22]],%22layout%22:%7B%22line-join%22:%22bevel%22%7D,%22paint%22:%7B%22line-color%22:%22%23DB6936%22,%22line-width%22:1.5,%22line-dasharray%22:[1.5,1]%7D%7D&before_layer=road-label&access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw)

### Filter existing features

To apply a filter expression to an existing style layer, you can use the `setfilter` and `layer_id` query parameters. `setfilter` accepts a valid filter [expression](https://docs.mapbox.com/style-spec/reference/expressions/), while `layer_id` specifies the layer in the style to add the filter to.

For example, to filter out country labels to only display "Canada", you could do the following:

```bash
https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/-91,60,2/800x600?access_token=YOUR_MAPBOX_ACCESS_TOKEN&setfilter=["==","name_en","Canada"]&layer_id=country-label
```

If you are using `setfilter`, you must specify a layer with `layer_id`. The `layer_id` must be a valid layer in the style.

#### Example request: Retrieve a static map with filtered layer

```bash
# Filter data in the `country-label` layer to
# display only data matching the string "Canada".
# Special characters in the request are encoded.
$ curl -g "https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/-96.8,47.3,1.8/800x400?access_token=YOUR_MAPBOX_ACCESS_TOKEN&setfilter=[%22==%22,%22name_en%22,%22Canada%22]&layer_id=country-label" --output example-mapbox-static-style-parameters-setfilter.png
```

#### Example response: Retrieve a static map with filtered layer

![Static map image of a red path overlay between two points labeled 'A' and 'B'.](https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/-96.8,47.3,1.8/800x400?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw&setfilter=[%22==%22,%22name_en%22,%22Canada%22]&layer_id=country-label)[View static image](https://api.mapbox.com/styles/v1/mapbox/streets-v12/static/-96.8,47.3,1.8/800x400?access_token=pk.EXAMPLE_PUBLIC_TOKEN.a-vxW4UaxOoUMWUTGnEArw&setfilter=[%22==%22,%22name_en%22,%22Canada%22]&layer_id=country-label)

## Static Images API errors

<table><thead><tr><th>Response body <code>message</code></th><th>HTTP status code</th><th>Description</th></tr></thead><tbody><tr><td><code>Not Authorized - Invalid Token</code></td><td><code>401</code></td><td>Check the access token you used in the query.</td></tr><tr><td><code>Forbidden</code></td><td><code>403</code></td><td>There may be an issue with your account. Check your <a href="https://console.mapbox.com/">Account page</a> for more details.<br><br>In some cases, using an access tokens with URL restrictions can also result in a <code>403</code> error. For more information, see our <a href="https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions">Token management guide</a>.</td></tr><tr><td><code>Style not found</code></td><td><code>404</code></td><td>Check the style ID used in the query.</td></tr><tr><td><code>Classic styles are no longer supported</code></td><td><code>410</code></td><td>Classic styles are no longer supported. See this <a href="https://docs.mapbox.com/map-styles/guides/classic-styles/">Classic Styles</a> blog post for additional deprecation details.</td></tr><tr><td><code>Style may not composite raster sources with vector sources</code></td><td><code>400</code></td><td>The style the request uses contains a source reference that incorrectly composites sources of two different types.</td></tr><tr><td><code>Invalid style source</code></td><td><code>422</code></td><td>The <code>sources</code> key within the style your request references contains an invalid value.</td></tr><tr><td><code>Zoom level must be between 0-22.</code></td><td><code>422</code></td><td>The zoom level specified in the query is larger than 22 or contains non-numeric characters.</td></tr><tr><td><code>Pitch must be between 0-60.</code></td><td><code>422</code></td><td>The pitch specified in the query is larger than 60 or contains non-numeric characters.</td></tr><tr><td><code>{Width}/{Height} must be between 1-1280.</code></td><td><code>422</code></td><td>The width or the height specified in the query is larger than 1280 or contains non-numeric characters.</td></tr><tr><td><code>Auto extent cannot be used with style parameters and no overlay</code></td><td><code>422</code></td><td><code>/auto/</code> is used for the extent in a request with style parameters that does not include an overlay. You should instead specify a longitude, latitude, and zoom for the request.</td></tr><tr><td><code>Auto extent cannot be determined when GeoJSON has no features</code></td><td><code>422</code></td><td><code>/auto/</code> is used for the extent in a request with a GeoJSON overlay that has no features. For example, <code>geojson({"type":"FeatureCollection","features":[]})</code>. You should instead specify a longitude, latitude, and zoom for the request.</td></tr><tr><td><code>The auto parameter cannot be used with additional location parameters, bearing, or pitch.</code></td><td><code>422</code></td><td>Additional location parameters (<code>lon</code>, <code>lat</code>, <code>zoom</code>), bearing, and pitch cannot be used with the <code>auto</code> parameter.</td></tr><tr><td><code>The bounding box parameter cannot be used with additional location parameters, bearing, or pitch.</code></td><td><code>422</code></td><td>Additional location parameters (<code>lon</code>, <code>lat</code>, <code>zoom</code>), bearing, and pitch cannot be used with the <code>bbox</code> parameter.</td></tr><tr><td><code>The GeoJSON argument has an unknown or unsupported geometry type</code></td><td><code>422</code></td><td>The GeoJSON geometry type in the overlay is not a <a href="https://geojson.org/">supported geometry type</a>.</td></tr><tr><td><code>You can only use {addlayer}/{setfilter} once per request</code></td><td><code>422</code></td><td>Only one style parameter can be used in a single request.</td></tr><tr><td><code>addlayer and setfilter cannot be used in the same request</code></td><td><code>422</code></td><td>Only one style parameter can be used in a single request. If you want to apply a filter to a new style layer, you should add a <code>filter</code> to the style layer object in the <code>addlayer</code> request.</td></tr><tr><td><code>The new layer's source reference key does not match any source keys in the requested style. Specify url and type in the source for the new layer.</code></td><td><code>422</code></td><td>The <code>source</code> specified in the <code>addlayer</code> request does not match the requested style. You should reformat the new layer source so that it's an object with a <code>type</code> and <code>url</code> (for example <code>source: { url: 'mapbox://mapbox.mapbox-streets-v8', type: 'vector'}</code>).</td></tr><tr><td><code>The new layer source url must be a properly formatted mapbox:// url (for example mapbox://mapbox.mapbox-streets-v7)</code></td><td><code>422</code></td><td>The new layer's source URL is not formatted correctly.</td></tr><tr><td><code>Layer is missing required attributes (id, type, source)</code></td><td><code>422</code></td><td>The layer is missing an <code>id</code>, <code>type</code>, or <code>source</code> value. The new layer should follow <a href="https://docs.mapbox.com/style-spec/reference/layers/">Mapbox style layer</a> syntax.</td></tr><tr><td><code>New layer sources must contain a source type and url</code></td><td><code>422</code></td><td>The new layer source is missing the source <code>type</code> or <code>url</code>.</td></tr><tr><td><code>New layers must be added with unique ids</code></td><td><code>422</code></td><td>The newly added layer has the same name as an existing layer in the style. You should rename the layer id to something else.</td></tr><tr><td><code>New layer type must be one of the following types: background, fill, line, symbol, circle, fill-extrusion, heatmap, hillshade, raster</code></td><td><code>422</code></td><td>The new layer type must be a valid <a href="https://docs.mapbox.com/style-spec/reference/layers/#type">Mapbox GL JS layer type</a></td></tr><tr><td><code>Must include layer_id in setfilter request</code></td><td><code>422</code></td><td><code>setfilter</code> must be used with <code>layer_id</code> to know what layer to apply the filter to.</td></tr><tr><td><code>layer_id must be an existing layer in the requested style</code></td><td><code>422</code></td><td>You can only apply the <code>setfilter</code> parameter to an existing layer in the style.</td></tr><tr><td><code>Invalid filter syntax</code></td><td><code>422</code></td><td>The expression passed to <code>setfilter</code> is not a valid <a href="https://docs.mapbox.com/style-spec/reference/expressions/">expression</a>.</td></tr><tr><td><code>Invalid layer passed to addlayer. Unable to parse JSON.</code></td><td><code>422</code></td><td>The JSON passed to <code>addlayer</code> is invalid.</td></tr><tr><td><code>Padding must be 1-4 numbers, comma-delimited.</code></td><td><code>422</code></td><td>Padding must be 1-4 numbers (no units, since pixels are implied) that are comma-delimited, resembling the <a href="https://developer.mozilla.org/en-US/docs/Web/CSS/padding">CSS specification</a> for padding, like <code>padding=5</code> or <code>padding=5,8,10,7</code>.</td></tr><tr><td><code>The padding cannot exceed the height or width of the requested image.</code></td><td><code>422</code></td><td>The sum of the top and bottom padding cannot exceed the image height, and the sum of the left and right padding cannot exceed the image width.</td></tr><tr><td><code>Padding cannot be used without the auto parameter or without a provided bounding box.</code></td><td><code>422</code></td><td>Padding can only be used with the <code>auto</code> parameter or with location specified by a <code>bbox</code>. Without the <code>auto</code> parameter or the <code>bbox</code> parameter, requests expect <code>lon</code>, <code>lat</code>, and <code>zoom</code>, and these values would change if asking for <code>padding</code> with a fixed image size.</td></tr><tr><td><code>Invalid bbox: *</code></td><td><code>422</code></td><td>The provided bbox is invalid. Make sure the bbox contains 4 valid coordinates enclosed in square brackets and are ordered like <code>[lon(min),lat(min),lon(max),lat(max)]</code>.</td></tr><tr><td><code>Overlay bounds are out of range.</code></td><td><code>422</code></td><td>The overlay coordinates are out of map range. Make sure the overlay coordinates are within range, meaning longitude between <code>-180</code> and <code>180</code> and latitude between <code>-85.0511</code> and <code>85.0511</code>.</td></tr><tr><td><code>Height and width must not exceed 1024px</code></td><td><code>422</code></td><td>The width and height of an image used as a <a href="#custom-marker">custom marker overlay</a> must be smaller than 1,024 pixels.</td></tr></tbody></table>

## Static Images API restrictions and limits

-   The default rate limit for the Mapbox Static Images API endpoint is 1,250 requests per minute. If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).
-   If you exceed the rate limit, you will receive an `HTTP 429 Too Many Requests` response. For information on rate limit headers, see the [Rate limit headers](https://docs.mapbox.com/api/guides#rate-limit-headers) section.
-   The caching behavior of the Static Images API is different than that of other Mapbox services. The longest amount of time you could potentially wait until a change is propagated to a static map is 12 hours. The Static Images API endpoint sets the following caching headers in the response: `max-age=43200, s-maxage=604800` if the style uses `mapbox.satellite`, and `max-age=43200, s-maxage=43200` otherwise. These `Cache-Control` headers show how long a source is considered valid for either the client or any request handled by our CDN. So, it is expected caching behavior that your static map might take up to 12 hours to update after making changes.
-   For general information on caching, see the [Maps APIs caching dive deeper guide](https://docs.mapbox.com/help/dive-deeper/api-caching/).

## Static Images API pricing

-   Billed by **requests**
-   See rates and discounts per Static Images API request in the pricing page's **[Maps](https://www.mapbox.com/pricing/#glstatic)** section

Usage of the Static Images API is measured in **API requests**. Details about the number of requests included in the free tier and the cost per request beyond what is included in the free tier are available on the [pricing page](https://www.mapbox.com/pricing/#glstatic).