# Geocoding API

The **Mapbox Geocoding API** does two things: *forward geocoding* and *reverse geocoding*:

-   Forward geocoding converts location text into geographic coordinates, turning `2 Lincoln Memorial Circle SW` into `-77.050,38.889`.
-   Reverse geocoding turns geographic coordinates into place names, turning `-77.050, 38.889` into `2 Lincoln Memorial Circle SW`. These location names can vary in specificity, from individual addresses to states and countries that contain the given coordinates.

> **Note (beta): Building entrance locations in public preview**
> 
> The `entrances` parameter is in public preview. This parameter allows you to access building entrances in the responses of your geocoding queries - both with permanent and temporary geocoding. To learn more about this parameter, see the [Forward geocoding optional parameters table](https://docs.mapbox.com/api/search/geocoding/#forward-geocoding-with-search-text-input) in the Geocoding API documentation.

> **Note: Geocoding v5**
> 
> If you are looking for the previous version, see the [Geocoding v5 API](https://docs.mapbox.com/api/search/geocoding-v5/) documentation.
> 
> If you are migrating from v5 to v6, note the new changes below:
> 
> -   Permanent and temporary geocoding are no longer different endpoints, and are now set via an optional parameter. The default behavior for all endpoints is temporary geocoding.
> -   Forward and reverse geocoding now have their own separate endpoints.
> -   Batch geocoding now has it's own endpoint.
> -   The Geocoding v6 API no longer provides POI data (use the [Search Box API](https://docs.mapbox.com/api/search/search-box/) for POI search).
> -   Streets can now be returned as a feature type and be filtered for via the `types` parameter.

> **Note: Geocoding from Snowflake**
> 
> [Snowflake](https://www.snowflake.com/) users can do batch geocoding operations on data stored in their data warehouse. The Mapbox Snowflake Native App is available [in the Snowflake Marketplace](https://app.snowflake.com/marketplace/listing/GZT0ZIFQPEA/mapbox-mapbox?originTab=provider&providerName=Mapbox).

For more background information on the Mapbox Geocoding API and how it works, see the [How geocoding works guide](https://docs.mapbox.com/help/getting-started/geocoding/#how-geocoding-works).

You may also use one of several [wrapper libraries](https://docs.mapbox.com/help/getting-started/geocoding/#libraries-and-plugins) to integrate the Mapbox Geocoding API into an application instead of using it directly.

-   For [Mapbox Search JS SDK](https://docs.mapbox.com/mapbox-search-js/guides/), Geocoding components are available on releases `1.0.0-beta.20` and later.

> **Related content (playground): [Geocoding API playground](https://docs.mapbox.com/playground/geocoding/)**
> 
> Try forward, reverse, and batch geocoding queries and see the results on a map.

## Geographic Feature Types

Various types of geographic features are available the Mapbox geocoding. Any type might appear as a top-level response, as context in a top-level response, or as a filtering option using the `types` parameter. Not all features are available or relevant in all parts of the world. New types are occasionally added as necessary to correctly capture global administrative hierarchies.

The geographic feature types listed from the largest to the most granular, are:

| Feature Type | Description |
| --- | --- |
| `country` | Generally recognized countries or, in some cases like Hong Kong, an area of quasi-national administrative status that has a designated country code under [ISO 3166-1](https://www.iso.org/iso-3166-country-codes.html). |
| `region` | Top-level sub-national administrative features, such as states in the United States or provinces in Canada or China. |
| `postcode` | Postal codes used in country-specific national addressing systems. |
| `district` | Features that are smaller than top-level administrative features but typically larger than cities, in countries that use such an additional layer in postal addressing (for example, prefectures in China). |
| `place` | Typically these are cities, villages, municipalities, etc. They're usually features used in postal addressing, and are suitable for display in ambient end-user applications where current-location context is needed (for example, in weather displays). |
| `locality` | Official sub-city features present in countries where such an additional administrative layer is used in postal addressing, or where such features are commonly referred to in local parlance. Examples include city districts in Brazil and Chile and arrondissements in France. |
| `neighborhood` | Colloquial sub-city features often referred to in local parlance. Unlike `locality` features, these typically lack official status and may lack universally agreed-upon boundaries. |
| `street` | Street features which host one or more addresses |
| `block` | Special feature type reserved for Japanese addresses. |
| `address` | Individual residential or business addresses. |
| `secondary_address` | Sub-unit, suite, or lot within a single parent address. Currently available in the US only. |

## Storing Geocoding Results

The Mapbox Geocoding API offers two types of result storage: **Permanent** and **Temporary**.

Temporary results are not allowed to be cached, while Permanent results are allowed to be cached and stored indefinitely.

Using Permanent storage with the Geocoding API requires that you have a [valid credit card on file](https://console.mapbox.com/account/settings/billing) or an active [enterprise contract](https://www.mapbox.com/contact/sales).

By default, the Geocoding API will use Temporary geocoding. To use Permanent geocoding, set the optional `permanent` parameter to `true`.

## Forward geocoding with search text input

**GET** : `https://api.mapbox.com/search/geocode/v6/forward?q={search_text}`

The **forward geocoding** query type allows you to look up a location using a string of search text and returns its standardized address, geographic context, and coordinates.

<table><thead><tr><th>Required parameters</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>q</code></td><td><code>string</code></td><td>The feature you’re trying to look up. This could be an address, a city name, etc. The search text should be expressed as a URL-encoded UTF-8 string, and must not contain the semicolon character (either raw or URL-encoded). Your search text, once decoded, must consist of at most 20 words and numbers separated by spacing and punctuation, and at most 256 characters.<br><br>The accuracy of coordinates returned by a forward geocoding request can be affected by how the addresses in the query are formatted. Learn more about address formatting best practices in the <a href="https://docs.mapbox.com/help/troubleshooting/address-geocoding-format-guide/">Address geocoding format guide</a>.</td></tr><tr><td><code>access_token</code></td><td><code>string</code></td><td>A valid Mapbox <a href="/guides/#access-tokens-and-token-scopes">access token</a>.</td></tr></tbody></table>

You can further refine the results of a forward geocoding query with the following optional parameters:

<table><thead><tr><th>Optional parameters</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>permanent</code></td><td>boolean</td><td>Specify whether you intend to store the results of the query (<code>true</code>) or not (<code>false</code>, default).</td></tr><tr><td><code>autocomplete</code></td><td>boolean</td><td>Specify whether to return autocomplete results (<code>true</code>, default) or not (<code>false</code>). When autocomplete is enabled, results will be included that start with the requested string, rather than responses that match it exactly. For example, a query for <code>India</code> might return both <code>India</code> and <code>Indiana</code> with autocomplete enabled, but only <code>India</code> if it’s disabled.<br><br>When autocomplete is enabled, each user keystroke counts as one request to the Geocoding API. For example, a search for "Cali" would be reflected as four separate Geocoding API requests. To reduce the total requests sent, you can configure your application to only call the Geocoding API after a specific number of characters are typed.</td></tr><tr><td><code>bbox</code></td><td>number</td><td>Limit results to only those contained within the supplied bounding box. Bounding boxes should be supplied as four numbers separated by commas, in <code>minLon,minLat,maxLon,maxLat</code> order. The bounding box cannot cross the 180th meridian.<br>You can use the <a href="https://labs.mapbox.com/location-helper/">Location Helper</a> to find a bounding box for use with this API.</td></tr><tr><td><code>country</code></td><td>string</td><td>Limit results to one or more countries. Permitted values are <a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2">ISO 3166 alpha 2</a> country codes separated by commas.</td></tr><tr><td><code>format</code></td><td>string</td><td>Specify the desired response format of results (<code>geojson</code>, default) or for backwards compatibility (<code>v5</code>).</td></tr><tr><td><code>language</code></td><td>string</td><td>Set the language of the text supplied in responses. Also affects result scoring, with results matching the user’s query in the requested language being preferred over results that match in another language. For example, an autocomplete query for things that start with <code>Frank</code> might return <code>Frankfurt</code> as the first result with an English (<code>en</code>) language parameter, but <code>Frankreich</code> (“France”) with a German (<code>de</code>) language parameter.<br><br>Options are <a href="https://en.wikipedia.org/wiki/IETF_language_tag">IETF language tags</a> comprised of a mandatory <a href="https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes">ISO 639-1 language code</a> and, optionally, one or more IETF subtags for country or script. More than one value can also be specified, separated by commas. The first language in the list will be considered as the primary language and a response will be generated for it. For other languages, translations will be provided, see the <a href="#translations">translations section</a>.<br><br>For more information on which specific languages are supported, see the <a href="#language-coverage">language coverage section</a>.</td></tr><tr><td><code>limit</code></td><td>integer</td><td>Specify the maximum number of results to return. The default is <code>5</code> and the maximum supported is <code>10</code>.</td></tr><tr><td><code>proximity</code></td><td>string</td><td>Bias the response to favor results that are closer to this location. Provided as two comma-separated coordinates in <code>longitude,latitude</code> order, or the string <code>ip</code> to bias based on reverse IP lookup.</td></tr><tr><td><code>types</code></td><td>string</td><td>Filter results to include only a subset (one or more) of the available feature types. Options are <code>country</code>, <code>region</code>, <code>postcode</code>, <code>district</code>, <code>place</code>, <code>locality</code>, <code>neighborhood</code>, <code>street</code>, and <code>address</code>. Multiple options can be comma-separated.<br><br>For more information on the available types, see the <a href="#geographic-feature-types">geographic feature types section</a>.</td></tr><tr><td><code>worldview</code></td><td>string</td><td>Returns features that are defined differently by audiences that belong to various regional, cultural, or political groups. Available worldviews are: <code>ar</code>,<code>cn</code>,<code>in</code>,<code>jp</code>,<code>ma</code>,<code>rs</code>,<code>ru</code>,<code>tr</code>,<code>us</code>. If <code>worldview</code> is not set, the <code>us</code> worldview boundaries are returned by default.<br><br>For more information about using the <code>worldview</code> parameter, see the <a href="#worldviews">worldviews section</a>.</td></tr><tr><td><code>entrances</code></td><td>boolean</td><td>Specify whether to return building entrance data (<code>true</code>) or not (<code>false</code>, default). When set to <code>true</code>, building entrances associated with address features will be included in the response. This includes both permanent and temporary geocoding. <strong>This parameter is in Public Preview.</strong></td></tr></tbody></table>

### Example requests: Forward geocoding with search text input

```bash
# A basic forward geocoding request
# Find Los Angeles

$ curl "https://api.mapbox.com/search/geocode/v6/forward?q=Los%20Angeles&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Find a town called 'Chester' in a specific region
# Add the proximity parameter with local coordinates
# This ensures the town of Chester, New Jersey is in the results

$ curl "https://api.mapbox.com/search/geocode/v6/forward?q=chester&proximity=-74.70850,40.78375&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Specify types=country to search only for countries named Georgia
# Results will exclude the American state of Georgia

$ curl "https://api.mapbox.com/search/geocode/v6/forward?q=georgia&types=country&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Limit the results to two results using the limit option
# Even though there are many possible matches
# for "Washington", this query will only return two results.

$ curl "https://api.mapbox.com/search/geocode/v6/forward?q=Washington&limit=2&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Search for the Place feature "Kaaleng" in the Ilemi Triangle. Specifying the cn worldview will return the country value South Sudan. Not including leaving the worldview parameter would default to the us worldview and return the country value Kenya.

$ curl "https://api.mapbox.com/search/geocode/v6/forward?q=Kaaleng&worldview=cn&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Forward geocoding with search text input

See the [Geocoding response object section](#geocoding-response-object).

> **Note (beta): Querying for Secondary Addresses**
> 
> -   If no types parameter is set, or the "secondary_address" type is included in the types filter, searching for secondary addresses will be enabled. - Secondary address queries are detected after a "designator" token (e.g. "Apt", "Suite", "Unit", "#") is followed by an identifier token (e.g. "12B", "A", "103"). - If a parent address is known to have secondary addresses, we will first return known units within our data matching the provided identifier. If the identifier is not known in our data, the unit will be "extrapolated", i.e. returned as typed with the same coordinate as the parent address.

## Forward geocoding with structured input

New in v6

**GET** : `https://api.mapbox.com/search/geocode/v6/forward?address_number={address_number}&street={street}&...`

**Structured Input** is a type of forward geocoding search that allows you to define the feature type of each element of the search query by type. This can increase the accuracy of results for well-formed datasets. To use Structured Input, the `q` parameter is dropped in favor of separate parameters for each feature type.

For best results, each element of the query must be assigned a feature type, and set `autocomplete` to `false`.

| Required parameters | Type | Description |
| --- | --- | --- |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |

The following feature types can be defined in a Structured Input query:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `address_line1` | `string` | A string including address_number and street. These values can be provided as separate parameters `address_number` and `street` listed below. |
| `address_number` | `string` | The number associated with the house. |
| `street` | `string` | The name of the street in the address |
| `block` | `string` | In some countries like Japan, the block is a component in the address |
| `place` | `string` | Typically these are cities, villages, municipalities, etc. They're usually features used in postal addressing, and are suitable for display in ambient end-user applications where current-location context is needed (for example, in weather displays). |
| `region` | `string` | Top-level sub-national administrative features, such as states in the United States or provinces in Canada or China. |
| `postcode` | `string` | Postal codes used in country-specific national addressing systems. |
| `locality` | `string` | Official sub-city features present in countries where such an additional administrative layer is used in postal addressing, or where such features are commonly referred to in local parlance. Examples include city districts in Brazil and Chile and arrondissements in France. |
| `neighborhood` | `string` | Colloquial sub-city features often referred to in local parlance. Unlike locality features, these typically lack official status and may lack universally agreed-upon boundaries. Not available for reverse geocoding requests. |
| `country` | `string` | **Either the [ISO 3166-1](https://www.iso.org/iso-3166-country-codes.html) code or the full country name**. When the ISO code is passed as parameter it acts as a hard filter on search results. When a full country name is passed, it is not a hard filter but influences the search results. |

You can further refine the results of a forward geocoding query with the following optional parameters:

<table><thead><tr><th>Optional parameters</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>permanent</code></td><td>boolean</td><td>Specify whether you intend to store the results of the query (<code>true</code>) or not (<code>false</code>, default).</td></tr><tr><td><code>autocomplete</code></td><td>boolean</td><td>Specify whether to return autocomplete results (<code>true</code>, default) or not (<code>false</code>). When autocomplete is enabled, results will be included that start with the requested string, rather than responses that match it exactly. For example, a query for <code>India</code> might return both <code>India</code> and <code>Indiana</code> with autocomplete enabled, but only <code>India</code> if it’s disabled.<br><br>When autocomplete is enabled, each user keystroke counts as one request to the Geocoding API. For example, a search for "Cali" would be reflected as four separate Geocoding API requests. To reduce the total requests sent, you can configure your application to only call the Geocoding API after a specific number of characters are typed.</td></tr><tr><td><code>bbox</code></td><td>number</td><td>Limit results to only those contained within the supplied bounding box. Bounding boxes should be supplied as four numbers separated by commas, in <code>minLon,minLat,maxLon,maxLat</code> order. The bounding box cannot cross the 180th meridian.<br>You can use the <a href="https://labs.mapbox.com/location-helper/">Location Helper</a> to find a bounding box for use with this API.</td></tr><tr><td><code>format</code></td><td>string</td><td>Specify the desired response format of results (<code>geojson</code>, default) or for backwards compatibility (<code>v5</code>).</td></tr><tr><td><code>language</code></td><td>string</td><td>Set the language of the text supplied in responses. Also affects result scoring, with results matching the user’s query in the requested language being preferred over results that match in another language. For example, an autocomplete query for things that start with <code>Frank</code> might return <code>Frankfurt</code> as the first result with an English (<code>en</code>) language parameter, but <code>Frankreich</code> (“France”) with a German (<code>de</code>) language parameter.<br><br>Options are <a href="https://en.wikipedia.org/wiki/IETF_language_tag">IETF language tags</a> comprised of a mandatory <a href="https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes">ISO 639-1 language code</a> and, optionally, one or more IETF subtags for country or script.<br><br>More than one value can also be specified, separated by commas. The first language in the list will be considered as the primary language and a response will be generated for it. For other languages, translations will be provided, see the <a href="#translations">translations section</a>.<br><br>For more information on which specific languages are supported, see the <a href="#language-coverage">language coverage section</a>.</td></tr><tr><td><code>limit</code></td><td>integer</td><td>Specify the maximum number of results to return. The default is <code>5</code> and the maximum supported is <code>10</code>.</td></tr><tr><td><code>proximity</code></td><td>string</td><td>Bias the response to favor results that are closer to this location. Provided as two comma-separated coordinates in <code>longitude,latitude</code> order, or the string <code>ip</code> to bias based on reverse IP lookup.</td></tr><tr><td><code>types</code></td><td>string</td><td>Filter results to include only a subset (one or more) of the available feature types. Options are <code>country</code>, <code>region</code>, <code>postcode</code>, <code>district</code>, <code>place</code>, <code>locality</code>, <code>neighborhood</code>, <code>street</code>, and <code>address</code>. Multiple options can be comma-separated.<br><br>For more information on the available types, see the <a href="#geographic-feature-types">geographic feature types section</a>.</td></tr><tr><td><code>worldview</code></td><td>string</td><td>Returns features that are defined differently by audiences that belong to various regional, cultural, or political groups. Available worldviews are: <code>ar</code>,<code>cn</code>,<code>in</code>,<code>jp</code>,<code>ma</code>,<code>rs</code>,<code>ru</code>,<code>tr</code>,<code>us</code>. If <code>worldview</code> is not set, the <code>us</code> worldview boundaries are returned by default.<br><br>For more information about using the <code>worldview</code> parameter, see the <a href="#worldviews">worldviews section</a>.</td></tr><tr><td><code>entrances</code></td><td>boolean</td><td>Specify whether to return building entrance data (<code>true</code>) or not (<code>false</code>, default). When set to <code>true</code>, building entrances associated with address features will be included in the response. This includes both permanent and temporary geocoding. <strong>This parameter is in Public Preview.</strong></td></tr></tbody></table>

### Example request: Forward geocoding with structured input

```bash
$ curl --location --request GET "https://api.mapbox.com/search/geocode/v6/forward?country=us&address_number=1600&street=pennsylvania%20ave%20nw&postcode=20500&place=Washington%20dc&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Forward geocoding with structured input

See the [Geocoding response object section](#geocoding-response-object).

### Recommended testing method for entrance data

> **Note (beta)**
> 
> The `entrances` parameter is in public preview.

Run an offline comparison of entrance data to test it quickly at scale:

1.  Run existing geocoder locations and Mapbox entrance points against the same set of addresses.
2.  Compare each result to a known reference point (for example, a past delivery location or other ground-truth coordinate) and measure which point is closer to the reference.
3.  The resulting distance-to-reference metric quantifies how often Mapbox entrance data places drivers closer to the correct entrance. The metric can be directly correlated to the efficiency gains, reduced dwell time, and lower support volume that would result from deploying Mapbox entrance locations in a production environment.

## Reverse geocoding

**GET** : `https://api.mapbox.com/search/geocode/v6/reverse?longitude={longitude}&latitude={latitude}`

The **reverse geocoding** query type allows you to look up a pair of coordinates and returns the geographic features there, including a standardized address or place and full geographic context.

| Required parameters | Type | Description |
| --- | --- | --- |
| `longitude` | `number` | The `longitude` decimal value from the geographic coordinate for the location being queried. |
| `latitude` | `number` | The `latitude` decimal value from the geographic coordinate for the location being queried. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |

You can further refine the results of a reverse geocoding query with the following optional parameters:

<table><thead><tr><th>Optional parameters</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>permanent</code></td><td><code>boolean</code></td><td>Specify whether you intend to store the results of the query (<code>true</code>) or not (<code>false</code>, default).</td></tr><tr><td><code>country</code></td><td><code>string</code></td><td>Limit results to one or more countries. Permitted values are <a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2">ISO 3166 alpha 2</a> country codes separated by commas.</td></tr><tr><td><code>language</code></td><td><code>string</code></td><td>Specify the user’s language. This parameter controls the language of the text supplied in responses.<br><br>Options are <a href="https://en.wikipedia.org/wiki/IETF_language_tag">IETF language tags</a> comprised of a mandatory <a href="https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes">ISO 639-1 language code</a> and, optionally, one or more IETF subtags for country or script.<br><br>More than one value can also be specified, separated by commas. The first language in the list will be considered as the primary language and a response will be generated for it. For other languages, translations will be provided, see the <a href="#translations">translations section</a>.<br><br>For more information on which specific languages are supported, see the <a href="#language-coverage">language coverage section</a>.</td></tr><tr><td><code>limit</code></td><td><code>integer</code></td><td>Specify the maximum number of results to return. The default is <code>1</code> and the maximum supported is <code>5</code>.<br><br>The default behavior in reverse geocoding is to return at most one feature at each of the multiple levels of the administrative hierarchy (for example, one address, one region, one country). Increasing the limit allows returning multiple features of the same type, but only for one type (for example, multiple <code>address</code> results). So, setting <code>limit</code> to a higher-than-default value requires specifying exactly one <code>types</code> parameter.</td></tr><tr><td><code>types</code></td><td><code>string</code></td><td>Filter results to include only a subset (one or more) of the available feature types. Options are <code>country</code>, <code>region</code>, <code>postcode</code>, <code>district</code>, <code>place</code>, <code>locality</code>, <code>neighborhood</code>, <code>street</code>, and <code>address</code>. Multiple options can be comma-separated.<br><br>For more information on the available types, see the <a href="#geographic-feature-types">geographic feature types section</a>.</td></tr><tr><td><code>worldview</code></td><td><code>string</code></td><td>Returns features that are defined differently by audiences that belong to various regional, cultural, or political groups. Available worldviews are: <code>ar</code>,<code>cn</code>,<code>in</code>,<code>jp</code>,<code>ma</code>,<code>rs</code>,<code>ru</code>,<code>tr</code>,<code>us</code>. If <code>worldview</code> is not set, the <code>us</code> <code>worldview</code> boundaries are returned by default.<br><br>For more information about using the <code>worldview</code> parameter, see the <a href="#worldviews">worldviews section</a>.</td></tr></tbody></table>

### Example request: Reverse geocoding

```bash
# A basic reverse geocoding request

# Retrieve places near a specific location

$ curl "https://api.mapbox.com/search/geocode/v6/reverse?longitude=-73.989&latitude=40.733&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Filter results to only include addresses

$ curl "https://api.mapbox.com/search/geocode/v6/reverse?longitude=-73.989&latitude=40.733&types=address&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Query within the Ilemi Triangle to return features for the US worldview

$ curl "https://api.mapbox.com/search/geocode/v6/reverse?longitude=35.4628&latitude=4.8975&worldview=us&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Reverse geocoding

The API response for a reverse geocoding query returns a GeoJSON feature collection in Mapbox Geocoding Response format. For more details on how a response from the Geocoding API is formatted, see the [Geocoding response object section](#geocoding-response-object).

## Batch geocoding

Improved in v6

> **Note: Geocoding from Snowflake**
> 
> [Snowflake](https://www.snowflake.com/) users can do batch geocoding operations on data stored in their data warehouse. The Mapbox Snowflake Native App is available [in the Snowflake Marketplace](https://app.snowflake.com/marketplace/listing/GZT0ZIFQPEA/mapbox-mapbox?originTab=provider&providerName=Mapbox).

**POST** : `https://api.mapbox.com/search/geocode/v6/batch`

The batch geocoding query type allows you to request up to 1000 forward or reverse geocoding queries in a single request.

Batch geocoding requests are formatted as a JSON object passed as the `BODY` of the request, but with multiple search queries one after another. Fields which were defined as query parameters in the URL string will become fields in the JSON object. Fields specifying multiple values (for example `types`, `country`, `bbox`, `proximity`) can be passed either as a comma-separated strings or as JSON-formatted arrays (for example a types filter value could be represented as `"address,street,place"` or `["address", "street", "place"]`).

In a single batch geocoding request, you can bundle queries of different types, including forward queries, forward Structure Input queries, and reverse queries. Each query can also has parameters defined individually -- for example one query could have IP proximity set to on while the rest have it off.

> **Note: Billing for batch geocoding requests**
> 
> Each individual search in a batch geocoding request counts as one request.

You can make a *batch geocoding request* by using an `HTTP POST` request with the following body structure:

```bash
https://api.mapbox.com/search/geocode/v6/batch
```

Request Body

```bash
[
    {
        "types": ["address"],
        "q": "1600 Pennsylvania Avenue NW, Washington, DC 20500, United States",
        "bbox": [-80, 35, -70, 40],
        "limit": 1
    },
    {
        "types": ["address"],
        "longitude": -73.986136,
        "latitude": 40.748895
    }
]
```

| Required parameters | Type | Description |
| --- | --- | --- |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |

| Optional parameters | Type | Description |
| --- | --- | --- |
| `permanent` | `boolean` | Specify if you intend to store the results of the query (`true`) or not (`false`, default). |

### Example request: Batch geocoding

```bash
$ curl --location --request POST "https://api.mapbox.com/search/geocode/v6/batch?access_token=YOUR_MAPBOX_ACCESS_TOKEN" \
--header 'Content-Type: application/json' \
--data-raw '[
    {/**** FORWARD GEOCODING REQUEST ****/
        "types": ["address"],
        "q": "1600 Pennsylvania Avenue NW, Washington, DC 20500, United States",
        "bbox": [-80, 35, -70, 40],
        "limit": 1
    },
    { /**** FORWARD GEOCODING REQUEST USING STRUCTURED INPUT ****/
        "country": "us",
        "address_number": "1600",
        "street": "Pennsylvania Avenue NW",
        "postcode": "20500",
        "place": "Washington, DC"
    },
    {/**** REVERSE GEOCODING REQUEST ****/
        "types": ["address"],
        "longitude": -73.986136,
        "latitude": 40.748895
    }
]'
```

### Response: Batch geocoding

The response from a batch geocoding query is an object with a `batch` property. The batch property is an array of [Geocoding response objects](#geocoding-response-object). The order of queries in the request determines the order of corresponding objects in the batch array. If there are no results returned for a particular query within the batch, the `features` array for that query is empty ( `"features": []` ).

Here's an example request in which there is a list of three queries. The first two requests are for forward geocoding, and the last one is for reverse.

```bash
$ curl --location --request POST 'https://api.mapbox.com/search/geocode/v6/batch?access_token=YOUR_MAPBOX_ACCESS_TOKEN&permanent=true' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
        "types": ["address"],
        "q": "1600 Pennsylvania Avenue NW, Washington, DC 20500, United States",
        "bbox": [-80, 35, -70, 40],
        "limit": 1
    },
    {
        "types": ["address"],
        "q": "aslkdjf",
        "bbox": [-80, 35, -70, 40]
    },
    {
        "types": ["address"],
        "longitude": -73.986136,
        "latitude": 40.748895
    }
]'
```

Notice that in the response, the results are in same the order as the queries in the request, where the second query `aslkdjf` doesn't return a result but still exists in the second position of the array:

```json
{
  "batch": [
    {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "id": "dXJuOm1ieGFkcjo2YzdhYjM4Yi05YzM4LTQ3ZDItODFkMS1jYzZlYjg5YzliMWM",
          "geometry": {
            "type": "Point",
            "coordinates": [-77.03655, 38.89768]
          },
          "properties": {
            "mapbox_id": "dXJuOm1ieGFkcjo2YzdhYjM4Yi05YzM4LTQ3ZDItODFkMS1jYzZlYjg5YzliMWM",
            "feature_type": "address",
            "name": "1600 Pennsylvania Avenue Northwest",
            "coordinates": {
              "longitude": -77.03655,
              "latitude": 38.89768,
              "accuracy": "rooftop"
            },
            "place_formatted": "Washington, District of Columbia 20500, United States",
            "match_code": {
              "address_number": "matched",
              "street": "matched",
              "postcode": "matched",
              "place": "matched",
              "region": "matched",
              "locality": "not_applicable",
              "country": "inferred",
              "confidence": "exact"
            },
            "context": {
              "address": {
                "mapbox_id": "dXJuOm1ieGFkcjo2YzdhYjM4Yi05YzM4LTQ3ZDItODFkMS1jYzZlYjg5YzliMWM",
                "address_number": "1600",
                "street_name": "Pennsylvania Avenue Northwest",
                "name": "1600 Pennsylvania Avenue Northwest"
              },
              "street": {
                "mapbox_id": "dXJuOm1ieGFkcjo2YzdhYjM4Yi05YzM4LTQ3ZDItODFkMS1jYzZlYjg5YzliMWM",
                "name": "Pennsylvania Avenue Northwest"
              },
              "neighborhood": {
                "mapbox_id": "dXJuOm1ieHBsYzpHYUVzN0E",
                "name": "National Mall",
                "alternate": {
                  "mapbox_id": "dXJuOm1ieHBsYzpEY1ZNN0E",
                  "name": "Franklin Mcpherson Square"
                }
              },
              "postcode": {
                "mapbox_id": "dXJuOm1ieHBsYzpBOEZPN0E",
                "name": "20500"
              },
              "place": {
                "mapbox_id": "dXJuOm1ieHBsYzpGSmlvN0E",
                "name": "Washington",
                "wikidata_id": "Q61"
              },
              "region": {
                "mapbox_id": "dXJuOm1ieHBsYzpCUVRz",
                "name": "District of Columbia",
                "wikidata_id": "Q3551781",
                "region_code": "DC",
                "region_code_full": "US-DC"
              },
              "country": {
                "mapbox_id": "dXJuOm1ieHBsYzpJdXc",
                "name": "United States",
                "wikidata_id": "Q30",
                "country_code": "US",
                "country_code_alpha_3": "USA"
              }
            }
          }
        }
      ],
      "attribution": "NOTICE: © 2023 Mapbox and its suppliers. All rights reserved. Use of this data is subject to the Mapbox Terms of Service (https://www.mapbox.com/about/maps/). This response and the information it contains may not be retained."
    },
    {
      "type": "FeatureCollection",
      "features": [],
      "attribution": "NOTICE: © 2023 Mapbox and its suppliers. All rights reserved. Use of this data is subject to the Mapbox Terms of Service (https://www.mapbox.com/about/maps/). This response and the information it contains may not be retained."
    },
    {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "id": "dXJuOm1ieGFkcjplMzVmMzZiOC1kYjRmLTQyOWItOWE4ZC0yZGQ3ZmQ1OTUzMTY",
          "geometry": {
            "type": "Point",
            "coordinates": [-73.9861365, 40.7488949]
          },
          "properties": {
            "mapbox_id": "dXJuOm1ieGFkcjplMzVmMzZiOC1kYjRmLTQyOWItOWE4ZC0yZGQ3ZmQ1OTUzMTY",
            "feature_type": "address",
            "name": "20 West 34th Street",
            "coordinates": {
              "longitude": -73.9861365,
              "latitude": 40.7488949,
              "accuracy": "rooftop"
            },
            "place_formatted": "New York, New York 10118, United States",
            "context": {
              "address": {
                "mapbox_id": "dXJuOm1ieGFkcjplMzVmMzZiOC1kYjRmLTQyOWItOWE4ZC0yZGQ3ZmQ1OTUzMTY",
                "address_number": "20",
                "street_name": "West 34th Street",
                "name": "20 West 34th Street"
              },
              "street": {
                "mapbox_id": "dXJuOm1ieGFkcjplMzVmMzZiOC1kYjRmLTQyOWItOWE4ZC0yZGQ3ZmQ1OTUzMTY",
                "name": "West 34th Street"
              },
              "neighborhood": {
                "mapbox_id": "dXJuOm1ieHBsYzpGQ2VNN0E",
                "name": "Koreatown"
              },
              "postcode": {
                "mapbox_id": "dXJuOm1ieHBsYzpBWUFPN0E",
                "name": "10118"
              },
              "locality": {
                "mapbox_id": "dXJuOm1ieHBsYzpGREtLN0E",
                "name": "Manhattan",
                "wikidata_id": "Q11299"
              },
              "place": {
                "mapbox_id": "dXJuOm1ieHBsYzpEZTVJN0E",
                "name": "New York",
                "wikidata_id": "Q60"
              },
              "district": {
                "mapbox_id": "dXJuOm1ieHBsYzpBUU5tN0E",
                "name": "New York County",
                "wikidata_id": "Q500416"
              },
              "region": {
                "mapbox_id": "dXJuOm1ieHBsYzpBYVRz",
                "name": "New York",
                "wikidata_id": "Q1384",
                "region_code": "NY",
                "region_code_full": "US-NY"
              },
              "country": {
                "mapbox_id": "dXJuOm1ieHBsYzpJdXc",
                "name": "United States",
                "wikidata_id": "Q30",
                "country_code": "US",
                "country_code_alpha_3": "USA"
              }
            }
          }
        }
      ],
      "attribution": "NOTICE: © 2023 Mapbox and its suppliers. All rights reserved. Use of this data is subject to the Mapbox Terms of Service (https://www.mapbox.com/about/maps/). This response and the information it contains may not be retained."
    }
  ]
}
```

## Geocoding response object

The response to a Geocoding API request is an object that contains the following properties:

<table><thead><tr><th>Property</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>type</code></td><td><code>string</code></td><td><code>"FeatureCollection"</code>, a GeoJSON type from the <a href="https://tools.ietf.org/html/rfc7946">GeoJSON specification</a>.</td></tr><tr><td><code>features</code></td><td><code>array</code></td><td>An array of feature objects.<br><strong>Forward geocodes:</strong> Returned features are ordered by relevance.<br><strong>Reverse geocodes:</strong> Returned features are ordered by index hierarchy, from most specific features to least specific features that overlap the queried coordinates.<br><br>Read the <a href="https://docs.mapbox.com/help/getting-started/geocoding/#search-result-prioritization">Search result prioritization</a> guide to learn more about how returned features are organized in the Geocoding API response.</td></tr><tr><td><code>attribution</code></td><td><code>string</code></td><td>Attributes the results of the Mapbox Geocoding API to Mapbox.</td></tr></tbody></table>

The data powering the Mapbox Geocoding API is constantly being updated and improved. This means that the values of properties in the response object are not guaranteed and may change within the same version of the API. Properties may be added to, but will not be removed from, the response within the same API version.

Each feature object in the `features` array may have the properties described below:

| Property | Type | Description |
| --- | --- | --- |
| `id` | `string` | Feature id. This property is named "id" to conform to the GeoJSON specification, but is the same id referred to as `mapbox_id` elsewhere in the response. |
| `type` | `string` | `"Feature"`, a GeoJSON type from the [GeoJSON specification](https://tools.ietf.org/html/rfc7946). |
| `geometry` | `object` | An object describing the spatial geometry of the returned feature. |
| `geometry.type` | `string` | `"Point"`, a GeoJSON type from the [GeoJSON specification](https://tools.ietf.org/html/rfc7946). |
| `geometry.coordinates` | `array` | An array in the format `[longitude,latitude]` at the center of the specified `bbox`. |
| `properties` | `object` | An object containing the resulting feature's details. |

Each properties object in a `feature` may have the attributes described below:

<table><thead><tr><th>Property</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>mapbox_id</code></td><td><code>string</code></td><td>Feature id. The <code>mapbox_id</code> uniquely identifies a place in the Mapbox search database. Mapbox ID’s are accepted in requests to the Geocoding API as a forward search, and will return the feature corresponding to that id.</td></tr><tr><td><code>feature_type</code></td><td><code>string</code></td><td>A string describing the type of the feature. Options are <code>country</code>, <code>region</code>, <code>postcode</code>, <code>district</code>, <code>place</code>, <code>locality</code>, <code>neighborhood</code>, <code>street</code>, <code>address</code>.<br><em>Formerly <code>place_type</code> in v5.</em></td></tr><tr><td><code>name</code></td><td><code>string</code></td><td>Formatted string of address_number and street.</td></tr><tr><td><code>name_preferred</code></td><td><code>string</code></td><td>The canonical or otherwise more common alias for the feature name. For example, searching for "America" will return "America" as the name, and "United States" as <code>name_preferred</code>.</td></tr><tr><td><code>place_formatted</code></td><td><code>string</code></td><td>Formatted string of result context: place region country postcode. The part of the result which comes after <code>name</code>.</td></tr><tr><td><code>full_address</code></td><td><code>string</code></td><td>Full formatted string of the feature, combining name_preferred and place_formatted.</td></tr><tr><td><code>context</code></td><td><code>object</code></td><td>A <a href="#the-context-object">context object</a> is an object representing the hierarchy of encompassing parent features. This may include a sub-object for any of the following properties: <code>country</code>, <code>region</code>, <code>postcode</code>, <code>district</code>, <code>place</code>, <code>locality</code>, <code>neighborhood</code>, <code>street</code>.<br><em>Which sub-objects are included is dependent upon the data coverage available and applicable to a given country or area.</em></td></tr><tr><td><code>coordinates</code></td><td><code>object</code></td><td>An object representing the geographical position and accuracy of the feature any routable points.</td></tr><tr><td><code>coordinates.longitude</code></td><td><code>number</code></td><td>Longitude of result</td></tr><tr><td><code>coordinates.latitude</code></td><td><code>number</code></td><td>Latitude of result</td></tr><tr><td><code>coordinates.accuracy</code></td><td><code>string</code></td><td>Accuracy metric for a returned <code>address</code>-<code>type</code> result. See <a href="#point-accuracy-for-address-features">Point accuracy for address features</a> below.</td></tr><tr><td><code>coordinates.routable_points</code></td><td><code>array</code></td><td>An array of routable point objects for an address feature, each including <code>name</code>, <code>longitude</code>, and <code>latitude</code> properties.</td></tr><tr><td><code>bbox</code></td><td><code>array</code></td><td>The bounding box of the feature as an array of <code>[minLon,minLat,maxLon,maxLat]</code>. This property is only provided with features of type <code>country</code>, <code>region</code>, <code>postcode</code>, <code>district</code>, <code>place</code>, <code>locality</code>, or <code>neighborhood</code>.</td></tr><tr><td><code>match_code</code></td><td><code>object</code></td><td>Additional metadata indicating how the result components match to the input query. See <a href="#smart-address-match">Smart Address Match</a> below.</td></tr></tbody></table>

### The Context Object

The `context` object within `properties` is a powerful piece of data which breaks out the complete geographical hierarchy for a given address or place. It is both a reliable way to access the named values of each component part of an address, plus contains feature-specific data such as the Wikidata id and 3-letter alpha country code.

Each sub-object in the context always has a `mapbox_id` and `name` associated with it. The id can be queried directly via a forward geocoding search to traverse into a different geographical layer. Address features will also include an `address` sub-object, which additionally contains `street_name` and `address_number` properties.

A `secondary_address` feature will also have a matching sub-object in the context:

```json
{
  "secondary_address": {
    "mapbox_id": "dXJuOm1ieGFkci11bml0OjdkZTE3MmUxLTJiMjktNDU1Mi1iNGQzLTkwN2JjMGZmOGQ1NDoyMDE",
    "name": "UNIT 201",
    "designator": "UNIT",
    "identifier": "201",
    "extrapolated": true // Indicates that the unit number is not known in our data, but the primary address is known to have secondary addresses associated with it
  },
  ...
}
```

#### Example Context Object

The following is an example of the context object returned for the address `2595 Lucky John Drive, Park City, Utah 84060, United States`:

```json
"context": {
    "address": {
        "mapbox_id": "dXJuOm1ieGFkcjozOGY3OTg1OC1jNzY0LTQ4ZGUtYTFmMC04NjJjOTM1ZWViMjc",
        "address_number": "2595",
        "street_name": "Lucky John Drive",
        "name": "2595 Lucky John Drive"
    },
    "street": {
        "mapbox_id": "dXJuOm1ieGFkcjozOGY3OTg1OC1jNzY0LTQ4ZGUtYTFmMC04NjJjOTM1ZWViMjc",
        "name": "Lucky John Drive"
    },
    "neighborhood": {
        "mapbox_id": "dXJuOm1ieHBsYzpITktzN0E",
        "name": "Park Meadows"
    },
    "postcode": {
        "mapbox_id": "dXJuOm1ieHBsYzpFUjNPN0E",
        "name": "84060"
    },
    "place": {
        "mapbox_id": "dXJuOm1ieHBsYzpEdjlvN0E",
        "name": "Park City",
        "wikidata_id": "Q482993"
    },
    "district": {
        "mapbox_id": "dXJuOm1ieHBsYzpBVlRHN0E",
        "name": "Summit County",
        "wikidata_id": "Q484563"
    },
    "region": {
        "mapbox_id": "dXJuOm1ieHBsYzpCa1Rz",
        "name": "Utah",
        "wikidata_id": "Q829",
        "region_code": "UT",
        "region_code_full": "US-UT"
    },
    "country": {
        "mapbox_id": "dXJuOm1ieHBsYzpJdXc",
        "name": "United States",
        "wikidata_id": "Q30",
        "country_code": "US",
        "country_code_alpha_3": "USA"
    }
}
```

#### Translations

When providing more than one language in the context object for the `country`, `region`, `district`, `place`, `neighborhood` properties will appear the translations property. This is an object where the key is the language code and the value is an object with language and name properties.

##### Translations Example

Example of the context object when passing the parameter `language=en,es`.

```json
{
  "context": {
    "address": {
      "mapbox_id": "dXJuOm1ieGFkcjowYzNhM2QzZi03ODIyLTQ3YzItODRlNC04YzA3ZDIxMTE2MmE",
      "address_number": "100",
      "street_name": "Jefferson Street Northwest",
      "name": "100 Jefferson Street Northwest"
    },
    "street": {
      "mapbox_id": "dXJuOm1ieGFkcjowYzNhM2QzZi03ODIyLTQ3YzItODRlNC04YzA3ZDIxMTE2MmE",
      "name": "Jefferson Street Northwest"
    },
    "neighborhood": {
      "mapbox_id": "dXJuOm1ieHBsYzpCQTRzN0E",
      "name": "Brightwood Park",
      "translations": {
        "en": {
          "language": "en",
          "name": "Brightwood Park"
        },
        "es": {
          "language": "es",
          "name": "Brightwood Park"
        }
      }
    },
    "postcode": {
      "mapbox_id": "dXJuOm1ieHBsYzpBNSt1N0E",
      "name": "20011"
    },
    "place": {
      "mapbox_id": "dXJuOm1ieHBsYzpGSmlvN0E",
      "name": "Washington",
      "wikidata_id": "Q61",
      "translations": {
        "en": {
          "language": "en",
          "name": "Washington"
        },
        "es": {
          "language": "es",
          "name": "Washington"
        }
      }
    },
    "region": {
      "mapbox_id": "dXJuOm1ieHBsYzpCUVRz",
      "name": "District of Columbia",
      "wikidata_id": "Q3551781",
      "region_code": "DC",
      "region_code_full": "US-DC",
      "translations": {
        "en": {
          "language": "en",
          "name": "District of Columbia"
        },
        "es": {
          "language": "es",
          "name": "Distrito de Columbia"
        }
      }
    },
    "country": {
      "mapbox_id": "dXJuOm1ieHBsYzpJdXc",
      "name": "United States",
      "wikidata_id": "Q30",
      "country_code": "US",
      "country_code_alpha_3": "USA",
      "translations": {
        "en": {
          "language": "en",
          "name": "United States"
        },
        "es": {
          "language": "es",
          "name": "Estados Unidos"
        }
      }
    }
  }
}
```

### The `routable_points` property

`routable_points` is an array that contains auxiliary geographic points associated with an address, each optimized for a specific access or navigation use case.

The array contains the following:

-   `default` is the standard, always-included routable point. This is the best navigable vehicle stopping location for accessing the address. This is algorithmically generated and has coverage across all address results.
-   `entrance` is the physical entrance location of the address, when available. This is distinct from the vehicle stop point and is intended for locating the door/entrance to access the building at that address.

> **Note (beta)**
> 
> The entrance routable point is in public preview.

### Point accuracy for `address` features

The `coordinates.accuracy` property in a Geocoding response object is a point accuracy metric for the returned `address` feature. This list is subject to change.

| Accuracy | Description |
| --- | --- |
| `rooftop` | Result intersects a known building/entrance. |
| `parcel` | Result is associated with one or more address within a specified polygonal boundary |
| `point` | Result is a known address point but does not intersect a known rooftop/parcel. |
| `interpolated` | Result position and existence are estimated based on nearby known addresses. |
| `approximate` | Result position is approximated by a 9-digit zipcode centroid. |
| `intersection` | For street type features only. The result is an intersection of 2 streets. |

### Smart Address Match

New in v6

The `match_code` object in the Geocoding API helps you understand how the resulting address feature aligns with the query submitted. Available only for address-type features, the `match_code` provides a breakdown of how each element of the result matches with the query, plus a confidence score, based on how well it matches. This can help you make decisions about what results to keep or throw out based on your application's tolerance for fuzzy matching on the query.

Smart Address Match is available for all forward geocoding requests that return an `address` type feature. It works best when using [Structured Input](#forward-geocoding-with-structured-input) forward queries, as the request components must be typed explicitly.

| Confidence Score | Description |
| --- | --- |
| `exact` | No components are unmatched (up to 2 may be inferred) and no there are no extraneous query tokens. |
| `high` | One component (excluding `house_number` or `region`) may have been corrected. Additionally, if only `house_number`, `street`, and `postcode` are provided and match, high confidence is returned. |
| `medium` | Two components (excluding `house_number` or `region`) may have changed. Allows for minor misspellings. If `house_number`, `street`, `place`, and `postcode` are matched the `region` may be corrected. |
| `low` | `house_number`, `region`, or more than 2 other components have been corrected. |

| Match Code | Description |
| --- | --- |
| `matched` | The component value matches the user's input. |
| `unmatched` | The component value doesn't match the user's input, or the user didn't submit this component type as part of the query. |
| `not_applicable` | The component is not used in the postal address string for example `locality`. |
| `inferred` | The component type wasn't submitted as part of the query, but we were able to confidently fill in the value. only returned for the `country` component. |
| `plausible` | Only relevant for the `address_number` and `secondary_address` components. In the case of `address_number`, this means the address accuracy is interpolated. In the case of `secondary_address`, this means the secondary address was extrapolated, i.e. the primary address is known to have secondary addresses, but the geocoding service did not find a specific matching secondary address in our data. |

#### Example match_code

This forward geocoding request with structured input includes a valid street address in Park City Utah, USA.

`https://api.mapbox.com/search/geocode/v6/forward?&address_number=2595&street=lucky john dr&place=park city&region=CO`

Note that the `region` parameter value is `CO`, and will yield an `unmatched` status in the `match_code` in the resulting feature's properties as the matched address is not in the US state of Colorado.

```json
{
  "name": "2595 Lucky John Drive",
  "place_formatted": "Park City, Utah 84060, United States",
  "match_code": {
    "address_number": "matched",
    "street": "matched",
    "postcode": "unmatched",
    "place": "matched",
    "region": "unmatched",
    "locality": "not_applicable",
    "country": "inferred",
    "confidence": "medium"
  }
}
```

### Example response: Forward geocoding

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "id": "dXJuOm1ieGFkcjo5ZDQzNzM1Mi0xZGZiLTRkNTItYWMxNC01MzllZjY5ODIwMmI",
      "geometry": {
        "type": "Point",
        "coordinates": [-111.86313, 40.725163]
      },
      "properties": {
        "mapbox_id": "dXJuOm1ieGFkcjo5ZDQzNzM1Mi0xZGZiLTRkNTItYWMxNC01MzllZjY5ODIwMmI",
        "feature_type": "address",
        "name": "974 2100 South",
        "coordinates": {
          "longitude": -111.86313,
          "latitude": 40.725163,
          "accuracy": "rooftop"
        },
        "place_formatted": "Salt Lake City, Utah 84106, United States",
        "match_code": {
          "address_number": "matched",
          "street": "matched",
          "postcode": "unmatched",
          "place": "unmatched",
          "region": "unmatched",
          "locality": "not_applicable",
          "country": "inferred",
          "confidence": "low"
        },
        "context": {
          "address": {
            "mapbox_id": "dXJuOm1ieGFkcjo5ZDQzNzM1Mi0xZGZiLTRkNTItYWMxNC01MzllZjY5ODIwMmI",
            "address_number": "974",
            "street_name": "2100 South",
            "name": "974 2100 South"
          },
          "street": {
            "mapbox_id": "dXJuOm1ieGFkcjo5ZDQzNzM1Mi0xZGZiLTRkNTItYWMxNC01MzllZjY5ODIwMmI",
            "name": "2100 South"
          },
          "neighborhood": {
            "mapbox_id": "dXJuOm1ieHBsYzpERWdNN0E",
            "name": "Fairmont",
            "alternate": {
              "mapbox_id": "dXJuOm1ieHBsYzpLMmRzN0E",
              "name": "Winfield"
            }
          },
          "postcode": {
            "mapbox_id": "dXJuOm1ieHBsYzpFU011N0E",
            "name": "84106"
          },
          "place": {
            "mapbox_id": "dXJuOm1ieHBsYzpFVmhvN0E",
            "name": "Salt Lake City",
            "wikidata_id": "Q23337",
            "alternate": {
              "mapbox_id": "dXJuOm1ieHBsYzpETE5vN0E",
              "name": "Millcreek"
            }
          },
          "district": {
            "mapbox_id": "dXJuOm1ieHBsYzpBVGdtN0E",
            "name": "Salt Lake County",
            "wikidata_id": "Q484556"
          },
          "region": {
            "mapbox_id": "dXJuOm1ieHBsYzpCa1Rz",
            "name": "Utah",
            "wikidata_id": "Q829",
            "region_code": "UT",
            "region_code_full": "US-UT"
          },
          "country": {
            "mapbox_id": "dXJuOm1ieHBsYzpJdXc",
            "name": "United States",
            "wikidata_id": "Q30",
            "country_code": "US",
            "country_code_alpha_3": "USA"
          }
        }
      }
    },
    {
      "type": "Feature",
      "id": "dXJuOm1ieGFkcjpkNjZkM2M0Zi1hNTA0LTQ3NTQtYTZjMS1iNjYwMGU2NWY4NmI",
      "geometry": {
        "type": "Point",
        "coordinates": [-111.919654, 40.725872]
      },
      "properties": {
        "mapbox_id": "dXJuOm1ieGFkcjpkNjZkM2M0Zi1hNTA0LTQ3NTQtYTZjMS1iNjYwMGU2NWY4NmI",
        "feature_type": "address",
        "name": "974 2100 South",
        "coordinates": {
          "longitude": -111.919654,
          "latitude": 40.725872,
          "accuracy": "interpolated"
        },
        "place_formatted": "South Salt Lake, Utah 84119, United States",
        "match_code": {
          "address_number": "plausible",
          "street": "matched",
          "postcode": "unmatched",
          "place": "unmatched",
          "region": "unmatched",
          "locality": "not_applicable",
          "country": "inferred",
          "confidence": "low"
        },
        "context": {
          "address": {
            "mapbox_id": "dXJuOm1ieGFkcjpkNjZkM2M0Zi1hNTA0LTQ3NTQtYTZjMS1iNjYwMGU2NWY4NmI",
            "address_number": "974",
            "street_name": "2100 South",
            "name": "974 2100 South"
          },
          "street": {
            "mapbox_id": "dXJuOm1ieGFkcjpkNjZkM2M0Zi1hNTA0LTQ3NTQtYTZjMS1iNjYwMGU2NWY4NmI",
            "name": "2100 South"
          },
          "neighborhood": {
            "mapbox_id": "dXJuOm1ieHBsYzpCVG5NN0E",
            "name": "Cannon",
            "alternate": {
              "mapbox_id": "dXJuOm1ieHBsYzpGYU5zN0E",
              "name": "Lincoln Park"
            }
          },
          "postcode": {
            "mapbox_id": "dXJuOm1ieHBsYzpFU1RPN0E",
            "name": "84119"
          },
          "place": {
            "mapbox_id": "dXJuOm1ieHBsYzpFbStJN0E",
            "name": "South Salt Lake",
            "alternate": {
              "mapbox_id": "dXJuOm1ieHBsYzpFMHRJN0E",
              "name": "Taylorsville"
            }
          },
          "district": {
            "mapbox_id": "dXJuOm1ieHBsYzpBVGdtN0E",
            "name": "Salt Lake County",
            "wikidata_id": "Q484556"
          },
          "region": {
            "mapbox_id": "dXJuOm1ieHBsYzpCa1Rz",
            "name": "Utah",
            "wikidata_id": "Q829",
            "region_code": "UT",
            "region_code_full": "US-UT"
          },
          "country": {
            "mapbox_id": "dXJuOm1ieHBsYzpJdXc",
            "name": "United States",
            "wikidata_id": "Q30",
            "country_code": "US",
            "country_code_alpha_3": "USA"
          }
        }
      }
    }
  ],
  "attribution": "NOTICE: © 2023 Mapbox and its suppliers. All rights reserved. Use of this data is subject to the Mapbox Terms of Service (https://www.mapbox.com/about/maps/). This response and the information it contains may not be retained."
}
```

## Worldviews

Use the optional `worldview` parameter to identify geographic features whose characteristics are defined differently by audiences belonging to various regional, cultural, or political groups. The Geocoding API only supports worldviews for the `country` and `region` data types, but may expand to additional data types in the future.

### Available worldviews

The Geocoding API supports the following values for the `worldview` parameter:

| Value | Description |
| --- | --- |
| `ar` | Features for an Argentinian audience |
| `cn` | Features for a mainland Chinese audience |
| `in` | Features for an Indian audience |
| `jp` | Features for a Japanese audience |
| `ma` | Features for a Moroccan audience |
| `rs` | Features for a Serbian audience |
| `ru` | Features for a Russian audience |
| `tr` | Features for a Turkish audience |
| `us` | Features for an American audience |

*Returned features are not officially approved for use by the above countries' respective governments.*

Each query supports a maximum of one worldview. Attempting to query for multiple worldviews in one search will result in an error message. When no worldview is selected, the Geocoding API returns features for the `us` worldview.

### Using the `worldview` parameter

In Mapbox services, the [worldview](https://docs.mapbox.com/help/glossary/worldview/) data field is used to identify geographic features whose characteristics are defined differently by audiences belonging to various regional, cultural, or political groups. Setting a worldview affects the place values returned for an address that is located in an area of non-unanimous ownership or definition.

For example, consider an address in the Ilemi Triangle, which borders Eastern Equatoria (South Sudan) and Turkana (Kenya). Depending on the worldview selected, an address in this location will be returned in different country contexts:

-   If `worldview=cn`, Kaaleng, Eastern Equatoria, or South Sudan will be returned.
-   If `worldview=us`, Kaaleng, Turkana, or Kenya will be returned.

This means that if you use `wordlview=cn` worldview with the Kenya (ke) country filter, a search for an address in this area will not be successful. Instead, you would need to use `worldview=cn` in combination with the South Sudan (ss) country filter.

Be cautious when combining `worldview` with the `country` parameter, since these indices are only loosely coupled.

For a visual example of how changing the `worldview` value impacts the boundaries on a map, see the [companion Maps boundary service](https://docs.mapbox.com/mapbox-gl-js/example/toggle-worldviews/).

## Language coverage

Translation availability also varies by language and region. We provide the following tiers of language support:

### Global coverage

These languages are almost always present for `country`, `region`, and prominent `place` features.

| Language |  |  |
| --- | --- | --- |
| `de` German | `en` English | `es` Spanish |
| `fr` French | `it` Italian | `nl` Dutch |
| `pl` Polish | — | — |

### Local coverage

These languages may lack global coverage but are almost always present for `country`, `region`, and prominent `place` features where they are widely used.

| Language |  |  |
| --- | --- | --- |
| `az` Azerbaijani | `bn` Bengali | `ca` Catalan |
| `cs` Czech | `da` Danish | `el` Modern Greek |
| `et` Estonian | `fa` Persian | `fi` Finnish |
| `ga` Irish | `hu` Hungarian | `id` Indonesian |
| `is` Icelandic | `ja` Japanese | `ka` Georgian |
| `km` Central Khmer | `ko` Korean | `lt` Lithuanian |
| `lv` Latvian | `mk` Macedonian | `mn` Mongolian |
| `ms` Malay macrolanguage | `nb` Norwegian Bokmål | `pt` Portuguese |
| `ro` Romanian | `sk` Slovak | `sl` Slovenian |
| `sq` Albanian | `th` Thai | `tl` Tagalog |
| `uk` Ukrainian | `vi` Vietnamese | `zh` Chinese |
| `zh_Hans` Simplified Chinese | `zh_Hant` Traditional Chinese | `zh_TW` Taiwanese Mandarin |

### Limited coverage

These languages are sometimes present but coverage tends to be inconsistent or geographically constrained.

| Language |  |  |
| --- | --- | --- |
| `ar` Arabic | `bs` Bosnian | `he` Hebrew |
| `hi` Hindi | `kk` Kazakh | `lo` Lao |
| `my` Burmese | `ru` Russian | `sr` Serbian |
| `sv` Swedish | `te` Telugu | `tk` Turkmen |
| `tr` Turkish | — | — |

Find more details about the format and properties of Geocoding API responses in the [Geocoding response object section](#geocoding-response-object).

## Intersection search support

**GET** : `https://api.mapbox.com/search/geocode/v6/forward?q={street1}%20and%20{street2}`

Intersection search allows users to search for a point where two streets intersect, without a precise address. Make an intersection search request by inserting an intersection connector between 2 streets. Intersection search is not available in all countries.

| Connector | Applicable countries |
| --- | --- |
| `&` | All countries where intersection search is supported |
| `and` or `x` | IN, JM, US |
| `e` | AR, BO, BR, CO, CR, DO, EC, GT, NI, PA, PE, SV, UY |

### Example request: Intersection search

Intersection search requests made by inserting `and`:

```bash
$ curl "https://api.mapbox.com/search/geocode/v6/forward?q=Market%20Street%20and%20Fremont%20Street&proximity=-122.39738575285674,37.7925147111369453&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

Intersection search requests made by inserting `&`:

```bash
$ curl "https://api.mapbox.com/search/geocode/v6/forward?q=Market%20Street%20%26%20Fremont%20Street&proximity=-122.39738575285674,37.7925147111369453&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

Intersection search requests made by inserting `x`:

```bash
$ curl "https://api.mapbox.com/search/geocode/v6/forward?q=Market%20Street%20x%20Fremont%20Street&proximity=-122.39738575285674,37.7925147111369453&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Intersection search

An intersection in a search response is a GeoJSON feature with `feature_type` of `street` and `accuracy` of `intersection`. For example, the result for the intersection `Market Street and Fremont Street` in San Francisco would look like:

```bash
https://api.mapbox.com/search/geocode/v6/forward?q=Market%20Street%20and%20Fremont%20Street&proximity=-122.39738575285674,37.7925147111369453&access_token=YOUR_MAPBOX_ACCESS_TOKEN&limit=1
```

The corresponding GeoJSON response would be:

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "id": "dXJuOm1ieGFkci1zdHI6Njk1ZDU0ZjctZTU2Yy00Y2ZhLThkNTctZTkzMGFiZTRkMDRl",
      "geometry": {
        "type": "Point",
        "coordinates": [-122.398298, 37.791734]
      },
      "properties": {
        "mapbox_id": "dXJuOm1ieGFkci1zdHI6Njk1ZDU0ZjctZTU2Yy00Y2ZhLThkNTctZTkzMGFiZTRkMDRl",
        "feature_type": "street",
        "full_address": "Market Street and Fremont Street, San Francisco, California 94105, United States",
        "name": "Market Street and Fremont Street",
        "name_preferred": "Market Street and Fremont Street",
        "coordinates": {
          "longitude": -122.398298,
          "latitude": 37.791734,
          "accuracy": "intersection"
        },
        "place_formatted": "San Francisco, California 94105, United States",
        "context": {
          "street": {
            "mapbox_id": "dXJuOm1ieGFkci1zdHI6Njk1ZDU0ZjctZTU2Yy00Y2ZhLThkNTctZTkzMGFiZTRkMDRl",
            "name": "Fremont Street"
          },
          "neighborhood": {
            "mapbox_id": "dXJuOm1ieHBsYzpETVlzN0E",
            "name": "Financial District",
            "wikidata_id": "Q599301"
          },
          "postcode": {
            "mapbox_id": "dXJuOm1ieHBsYzpFcCtPN0E",
            "name": "94105"
          },
          "place": {
            "mapbox_id": "dXJuOm1ieHBsYzpFVzBJN0E",
            "name": "San Francisco",
            "wikidata_id": "Q62"
          },
          "district": {
            "mapbox_id": "dXJuOm1ieHBsYzpBVG1HN0E",
            "name": "San Francisco County",
            "wikidata_id": "Q62"
          },
          "region": {
            "mapbox_id": "dXJuOm1ieHBsYzpCbVRz",
            "name": "California",
            "wikidata_id": "Q99",
            "region_code": "CA",
            "region_code_full": "US-CA"
          },
          "country": {
            "mapbox_id": "dXJuOm1ieHBsYzpJdXc",
            "name": "United States",
            "wikidata_id": "Q30",
            "country_code": "US",
            "country_code_alpha_3": "USA"
          }
        }
      }
    }
  ],
  "attribution": "NOTICE: © 2026 Mapbox and its suppliers. All rights reserved. Use of this data is subject to the Mapbox Terms of Service (https://www.mapbox.com/about/maps/). This response and the information it contains may not be retained."
}
```

Find more details about the format and properties of Geocoding API responses in the [response object section](#geocoding-response-object).

## Geocoding API errors

<table><thead><tr><th>Response body <code>message</code></th><th>HTTP error code</th><th>Description</th></tr></thead><tbody><tr><td><code>Not Authorized - No Token</code></td><td><code>401</code></td><td>No token was used in the query.</td></tr><tr><td><code>Not Authorized - Invalid Token</code></td><td><code>401</code></td><td>Check the access token you used in the query.</td></tr><tr><td><code>Forbidden</code></td><td><code>403</code></td><td>There may be an issue with your account. Check your <a href="https://console.mapbox.com/">Account page</a> for more details.<br><br>In some cases, using an access tokens with URL restrictions can also result in a <code>403</code> error. For more information, see the <a href="https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions">URL Restrictions</a> in the Token management guide.</td></tr><tr><td><code>Not Found</code></td><td><code>404</code></td><td>Check the endpoint you used in the query.</td></tr><tr><td><code>Not Found</code></td><td><code>404</code></td><td>No search text or structured input parameters were provided in the query.</td></tr><tr><td><code>BBox is not valid. Must be an array of format [minX, minY, maxX, maxY]</code></td><td><code>422</code></td><td>Check the format for <code>bbox</code>, where the first coordinate pair should refer to the southwestern corner of the box and the second pair should refer to the northeastern corner of the box.</td></tr><tr><td><code>BBox {minX/maxX} value must be a number between -180 and 180</code></td><td><code>422</code></td><td>Check the format of <code>minX</code> and <code>maxX</code> for <code>bbox</code>.</td></tr><tr><td><code>BBox {minY/maxY} value must be a number between -90 and 90</code></td><td><code>422</code></td><td>Check the format of <code>minY</code> and <code>maxY</code> for <code>bbox</code>.</td></tr><tr><td><code>BBox {minX/minY} value cannot be greater than {maxX/maxY} value</code></td><td><code>422</code></td><td>Check the values of the coordinate pairs used in <code>bbox</code>.</td></tr><tr><td><code>Type "{input}" is not a known type. Must be one of: country, region, place, district, postcode, locality, neighborhood, and address</code></td><td><code>422</code></td><td>Check the <a href="#geographic-feature-types"><code>type</code></a> you used in the query.</td></tr><tr><td><code>Stack "{input}" is not a known stack. Must be one of: …</code></td><td><code>422</code></td><td>The <code>country</code> parameter must be a valid <a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2">ISO 3166 alpha 2</a> country code.</td></tr><tr><td><code>Batch queries must include 50 queries or less</code></td><td><code>422</code></td><td>Your batch geocode request cannot contain more than 50 queries.</td></tr><tr><td><code>Query too long {query length}/256 characters</code></td><td><code>422</code></td><td>Your query cannot contain more than 256 characters.</td></tr><tr><td><code>Query too long - {query tokens length}/20 tokens</code></td><td><code>422</code></td><td>Your query cannot contain more than 20 word boundary-delimited strings (also known as tokens).</td></tr><tr><td><code>Proximity must be an array in the form [lon, lat]</code></td><td><code>422</code></td><td>The <code>proximity</code> parameter must contain two comma-separated values.</td></tr><tr><td><code>Proximity lon value must be a number between -180 and 180</code></td><td><code>422</code></td><td>Check the <code>proximity</code> parameter's longitude value.</td></tr><tr><td><code>Proximity lat value must be a number between -90 and 90</code></td><td><code>422</code></td><td>Check the <code>proximity</code> parameter's latitude value.</td></tr><tr><td><code>"{input}" is not a valid language code</code></td><td><code>422</code></td><td>The <code>language</code> parameter must be a valid <a href="https://en.wikipedia.org/wiki/IETF_language_tag">IETF language tag</a> comprised of a mandatory <a href="https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes">ISO 639-1 language code</a> and, optionally, one or more IETF subtags for country or script.</td></tr><tr><td><code>options.language should be a list of no more than 20 languages</code></td><td><code>422</code></td><td>Your query's <code>language</code> parameter cannot contain more than 20 language codes.</td></tr><tr><td><code>options.language should be a list of unique language codes</code></td><td><code>422</code></td><td>The comma-separated values in <code>language</code> must be unique.</td></tr><tr><td><code>limit must be combined with a single type parameter when reverse geocoding</code></td><td><code>422</code></td><td>If you make a <a href="#reverse-geocoding">reverse geocoding request</a> with the <code>limit</code> parameter, you must also use the <code>type</code> parameter.</td></tr><tr><td><code>Rate limit exceeded</code></td><td><code>429</code></td><td>You have exceeded your set rate limit. Check your <a href="https://console.mapbox.com/">Account page</a> for more details.</td></tr></tbody></table>

## Geocoding restrictions and rate limits

To protect the Geocoding API and maximize service stability, Mapbox rate limits Geocoding API requests.

-   The default Geocoding API rate limit is 1000 requests per minute, but can be adjusted on a per-account basis. Mapbox may automatically adjust customer rate limits to accommodate high traffic applications, or prevent abuse, if deemed appropriate.
-   An HTTP error code of `429` will be returned if the rate limit is reached.
-   If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).

## Geocoding API pricing

-   Billed by **requests**
-   See rates and discounts per Geocoding API request in the pricing page's **[Search](https://www.mapbox.com/pricing/#search)** section

Usage of the Geocoding API is measured in **API requests**. Details about the number of requests included in the free tier and the cost per request beyond what is included in the free tier are available on the [pricing page](https://www.mapbox.com/pricing/#search). You may only use responses from the Geocoding API in conjunction with a Mapbox map.

### Autocomplete and pricing

The Geocoding API's [`autocomplete`](#forward-geocoding-with-search-text-input) parameter is enabled by default. If you are using the API with `autocomplete` enabled, you will likely accumulate one API request per keystroke. For example, if you have autocomplete turned on and you set up your application to make a Geocoding API request every time a user types a character into a search bar, then a user typing "Cali" looking for "California" will be making *four Geocoding API requests*, one for each letter.

To reduce the number of requests made while `autocomplete` is enabled, you can wait to begin making API requests until a specific number of characters are entered. The Geocoding API doesn't have a parameter for limiting requests built in, but you can use the [Mapbox Search JS SDK](https://docs.mapbox.com/mapbox-search-js/guides/) to configure the search configuration, or you can write it into your application directly.

### Batch geocoding and pricing

[Batch geocoding](#batch-geocoding) allows you to have multiple geocoding queries in a single request. Each individual search in a batch geocoding request counts as one request. For example, a request that contains three searches would count as three requests and be billed as such. These three requests would be reflected on your [statistics graph](https://console.mapbox.com/account/statistics/) and your bill.

## Japan Geocoding

The Geocoding API includes a robust Japan address and place search. To access the new, high-accuracy and high-coverage Japan search, you must set both the `language` and `country` parameters:

-   `language=ja`
-   `country=jp`

Japan geocoding responses use the same [geographic feature types section](#geographic-feature-types) as other countries to define a feature's context components (with the addition of a `block` component, where applicable). These feature types represent the following equivalents of a Japanese address structure:

| Geographic feature type | Japan address equivalent |
| --- | --- |
| `block` | block |
| `neighborhood` | chome |
| `locality` | oaza |
| `place` | city |
| `region` | prefecture |

Japan geocoding responses also contain a unique `feature.properties.reading` object for Japanese reading equivalents by script type, for example:

```json
{
  ja-Kana:"ヤマグチケンシモノセキシシンアカダニシマチ"
  ja-Latn:"yamaguchiken shimonosekishi shinakadanishimachi"
}
```