# Datasets API

The **Mapbox Datasets API** supports reading, creating, updating, and removing features from a dataset.

Using the Datasets API involves interacting with two types of objects: [**datasets**](#the-dataset-object) and [**features**](#the-feature-object). Datasets contain one or more collections of [GeoJSON features](https://tools.ietf.org/html/rfc7946). When you edit a dataset object, you change the `name` and `description` properties of the dataset itself. When you edit a feature object, you edit the contents of the dataset, such as the coordinates or properties of a feature.

To serve your geographic data at scale, you can convert your dataset into a tileset using [the Uploads API](https://docs.mapbox.com/api/maps/uploads/).

> **Note: Required access token scopes**
> 
> Using the Datasets API requires an access token with the appropriate `dataset` [scopes](https://docs.mapbox.com/accounts/guides/tokens/#scopes) (`datasets:list`, `datasets:write`, and `datasets:read`). Each endpoint below specifies the required scope. You can create a new access token with these scopes on the [access token page](https://console.mapbox.com/account/access-tokens/) in your Developer Console.

## The dataset object

The **dataset** object contains information pertinent to a specific dataset. Each dataset object contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `owner` | `string` | The username of the dataset owner. |
| `id` | `string` | The ID for an existing dataset. |
| `created` | `string` | A timestamp indicating when the dataset was created. |
| `modified` | `string` | A timestamp indicating when the dataset was last modified. |
| `bounds` | `array` | The extent of features in the dataset in the format [`west`, `south`, `east`, `north`]. |
| `features` | `integer` | The number of features in the dataset. |
| `size` | `integer` | The size of the dataset in bytes. |
| `name` | `string` | *Optional.* The name of the dataset. |
| `description` | `string` | *Optional.* A description of the dataset. |

### Example dataset object

```json
{
  "owner": "{username}",
  "id": "{dataset_id}",
  "created": "{timestamp}",
  "modified": "{timestamp}",
  "bounds": [-10, -10, 10, 10],
  "features": 100,
  "size": 409600,
  "name": "{name}",
  "description": "{description}"
}
```

## List datasets

**GET** : `https://api.mapbox.com/datasets/v1/{username}` [Token scope: **datasets:list**]

List all the datasets that belong to a particular account. This endpoint supports [pagination](https://docs.mapbox.com/api/guides/#pagination).

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account for which to list datasets. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `datasets:list` scope. |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `limit` | `integer` | The maximum number of datasets to return. |
| `sortby` | `string` | Sort the results by `modified` or `created` dates. `modified` sorts by the most recently updated dataset, while `created` sorts by the oldest dataset first. |
| `start` | `string` | The ID of the dataset after which to start the listing. Find the dataset ID in the `Link` header of a response. See the [pagination](https://docs.mapbox.com/api/guides/#pagination) section for details. |

### Example request: List datasets

```bash
$ curl "https://api.mapbox.com/datasets/v1/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::datasets:list"
```

### Response: List datasets

The response to a request to this endpoint is a list of [datasets](#the-dataset-object).

### Example response: List datasets

```json
[
  {
    "owner": "{username}",
    "id": "{dataset_id}",
    "created": "{timestamp}",
    "modified": "{timestamp}",
    "bounds": [-10, -10, 10, 10],
    "features": 100,
    "size": 409600,
    "name": "{name}",
    "description": "{description}"
  },
  {
    "owner": "{username}",
    "id": "{dataset_id}",
    "created": "{timestamp}",
    "modified": "{timestamp}",
    "bounds": [-10, -10, 10, 10],
    "features": 100,
    "size": 409600,
    "name": "{name}",
    "description": "{description}"
  }
]
```

### Supported libraries: List datasets

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Python CLI](https://github.com/mapbox/mapbox-cli-py#datasets-list)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#listdatasets)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Create a dataset

**POST** : `https://api.mapbox.com/datasets/v1/{username}` [Token scope: **datasets:write**]

Create a new, empty dataset.

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the new dataset belongs. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `datasets:write` scope. |

### Example request: Create a dataset

```bash
$ curl -X POST "https://api.mapbox.com/datasets/v1/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::datasets:write" \
  -d @request.json \
  --header "Content-Type:application/json
```

### Example request body: Create a dataset

```json
{
  "name": "foo",
  "description": "bar"
}
```

### Response: Create a dataset

The response to a successful request to this endpoint will be a new, empty [dataset](#the-dataset-object).

### Example response: Create a dataset

```json
{
  "owner": "{username}",
  "id": "{dataset_id}",
  "created": "{timestamp}",
  "modified": "{timestamp}",
  "bounds": [-10, -10, 10, 10],
  "features": 100,
  "size": 409600,
  "name": "foo",
  "description": "bar"
}
```

### Supported libraries: Create a dataset

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Python CLI](https://github.com/mapbox/mapbox-cli-py#datasets-create)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#createdataset)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Retrieve a dataset

**GET** : `https://api.mapbox.com/datasets/v1/{username}/{dataset_id}` [Token scope: **datasets:read**]

Retrieve information about a single existing dataset.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the dataset belongs. |
| `dataset_id` | `string` | The ID of the dataset to be retrieved. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `datasets:read` scope. |

### Example request: Retrieve a dataset

```bash
$ curl "https://api.mapbox.com/datasets/v1/YOUR_MAPBOX_USERNAME/{dataset_id}?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Retrieve a dataset

The response to a successful request to this endpoint will be the requested [dataset](#the-dataset-object).

### Example response: Retrieve a dataset

```json
{
  "owner": "{username}",
  "id": "{dataset_id}",
  "created": "{timestamp}",
  "modified": "{timestamp}",
  "bounds": [-10, -10, 10, 10],
  "features": 100,
  "size": 409600,
  "name": "{name}",
  "description": "{description}"
}
```

### Supported libraries: Retrieve a dataset

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Python CLI](https://github.com/mapbox/mapbox-cli-py#datasets-read-dataset)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#getmetadata)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Update a dataset

**PATCH** : `https://api.mapbox.com/datasets/v1/{username}/{dataset_id}` [Token scope: **datasets:write**]

Update the properties of a specific dataset. The request body must be valid JSON. This endpoint is used to change the `name` and `description` of the dataset object. To add features to a dataset or edit existing dataset features, use the [Insert or update a feature](#insert-or-update-a-feature) endpoint.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the dataset belongs. |
| `dataset_id` | `string` | The ID of the dataset to be updated. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `datasets:write` scope. |

### Example request: Update a dataset

```bash
$ curl -X PATCH "https://api.mapbox.com/datasets/v1/YOUR_MAPBOX_USERNAME/{dataset_id}?access_token=TOKEN_PLACEHOLDER::datasets:write" \
  -H "Content-Type: application/json" \
  -d @data.json
```

### Example request body: Update a dataset

```json
{
  "name": "foo",
  "description": "bar"
}
```

### Response: Update a dataset

The response to a successful request to this endpoint will be the updated [dataset](#the-dataset-object).

### Example response: Update a dataset

```json
{
  "owner": "{username}",
  "id": "{dataset_id}",
  "created": "{timestamp}",
  "modified": "{timestamp}",
  "bounds": [-10, -10, 10, 10],
  "features": 100,
  "size": 409600,
  "name": "foo",
  "description": "bar"
}
```

### Supported libraries: Update a dataset

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Python CLI](https://github.com/mapbox/mapbox-cli-py#datasets-update-dataset)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#updatemetadata)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Delete a dataset

**DELETE** : `https://api.mapbox.com/datasets/v1/{username}/{dataset_id}` [Token scope: **datasets:write**]

Delete a specific dataset. All the features contained in the dataset will be deleted too.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the dataset belongs. |
| `dataset_id` | `string` | The ID of the dataset to be deleted. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `datasets:write` scope. |

### Example request: Delete a dataset

```bash
$ curl -X DELETE "https://api.mapbox.com/datasets/v1/YOUR_MAPBOX_USERNAME/{dataset_id}?access_token=TOKEN_PLACEHOLDER::datasets:write"
```

### Response: Delete a dataset

```
HTTP 204 No Content
```

### Supported libraries: Delete a dataset

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Python CLI](https://github.com/mapbox/mapbox-cli-py#datasets-delete-dataset)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#deletedataset)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## The feature object

A **feature** is a GeoJSON Feature object representing a feature in the dataset. GeometryCollections and null geometries are not supported. For a full list of GeoJSON Feature properties, see the [GeoJSON specification](https://tools.ietf.org/html/rfc7946#section-3.2).

### Example feature object

```json
{
  "id": "{feature_id}",
  "type": "Feature",
  "properties": {
    "prop0": "value0"
  },
  "geometry": {
    "coordinates": [102, 0.5],
    "type": "Point"
  }
}
```

## List features

**GET** : `https://api.mapbox.com/datasets/v1/{username}/{dataset_id}/features` [Token scope: **datasets:read**]

List all the features in a dataset.

This endpoint supports [pagination](https://docs.mapbox.com/api/guides/#pagination) so that you can list many features.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the dataset belongs. |
| `dataset_id` | `string` | The ID of the dataset for which to retrieve features. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `datasets:read` scope. |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `limit` | `integer` | The maximum number of features to return, from `1` to `1000`. The default is `1000`. |
| `start` | `string` | The ID of the feature after which to start the listing. Find the feature ID in the `Link` header of a response. See the [pagination](https://docs.mapbox.com/api/guides/#pagination) section for details. |

### Example request: List features

```bash
$ curl "https://api.mapbox.com/datasets/v1/YOUR_MAPBOX_USERNAME/{dataset_id}/features?access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Limit results to 50 features

$ curl "https://api.mapbox.com/datasets/v1/YOUR_MAPBOX_USERNAME/{dataset_id}/features?limit=50&access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Use pagination to start the listing after the feature with ID f6d9

$ curl "https://api.mapbox.com/datasets/v1/YOUR_MAPBOX_USERNAME/{dataset_id}/features?start=f6d9&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: List features

The response body will be a GeoJSON FeatureCollection.

### Example response: List features

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "id": "{feature_id}",
      "type": "Feature",
      "properties": {
        "prop0": "value0"
      },
      "geometry": {
        "coordinates": [102, 0.5],
        "type": "Point"
      }
    },
    {
      "id": "{feature_id}",
      "type": "Feature",
      "properties": {
        "prop0": "value0"
      },
      "geometry": {
        "coordinates": [
          [102, 0],
          [103, 1],
          [104, 0],
          [105, 1]
        ],
        "type": "LineString"
      }
    }
  ]
}
```

### Supported libraries: List features

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Python CLI](https://github.com/mapbox/mapbox-cli-py#datasets-list-features)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#listfeatures)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Insert or update a feature

**PUT** : `https://api.mapbox.com/datasets/v1/{username}/{dataset_id}/features/{feature_id}` [Token scope: **datasets:write**]

Insert or update a feature in a specified dataset:

-   **Insert**: If a feature with the given `feature_id` does not already exist, a new feature will be created.
-   **Update**: If a feature with the given `feature_id` already exists, it will be replaced.

If you are inserting a feature into a dataset, you must add the feature as the body of the `PUT` request. This should be one individual GeoJSON feature, not a GeoJSON FeatureCollection. If the GeoJSON feature has a top-level `id` property, it must match the `feature_id` you use in the URL endpoint.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the dataset belongs. |
| `dataset_id` | `string` | The ID of the dataset for which to insert or update features. |
| `feature_id` | `string` | The ID of the feature to be inserted or updated. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `datasets:write` scope. |

### Example request: Insert or update a feature

```bash
$ curl "https://api.mapbox.com/datasets/v1/YOUR_MAPBOX_USERNAME/{dataset_id}/features/{feature_id}?access_token=TOKEN_PLACEHOLDER::datasets:write" \
  -X PUT \
  -H "Content-Type: application/json" \
  -d @file.geojson
```

### Example request body: Insert or update a feature

```json
{
  "id": "{feature_id}",
  "type": "Feature",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [100, 0],
        [101, 0],
        [101, 1],
        [100, 1],
        [100, 0]
      ]
    ]
  },
  "properties": {
    "prop0": "value0"
  }
}
```

### Response: Insert or update a feature

The response to a successful request to this endpoint will be the new or updated [feature object](#the-feature-object).

### Example response: Insert or update a feature

```json
{
  "id": "{feature_id}",
  "type": "Feature",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [100, 0],
        [101, 0],
        [101, 1],
        [100, 1],
        [100, 0]
      ]
    ]
  },
  "properties": {
    "prop0": "value0"
  }
}
```

### Supported libraries: Insert or update a feature

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Python CLI](https://github.com/mapbox/mapbox-cli-py#datasets-put-feature)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#putfeature)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Retrieve a feature

**GET** : `https://api.mapbox.com/datasets/v1/{username}/{dataset_id}/features/{feature_id}` [Token scope: **datasets:read**]

Retrieve a specific feature from a dataset.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the dataset belongs. |
| `dataset_id` | `string` | The ID of the dataset from which to retrieve a feature. |
| `feature_id` | `string` | The ID of the feature to be retrieved. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `datasets:read` scope. |

### Example request: Retrieve a feature

```bash
$ curl "https://api.mapbox.com/datasets/v1/YOUR_MAPBOX_USERNAME/{dataset_id}/features/{feature_id}?access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Retrieve a feature

The response to a successful request to this endpoint is the requested [feature object](#the-feature-object).

### Example response: Retrieve a feature

```json
{
  "id": "{feature_id}",
  "type": "Feature",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [100, 0],
        [101, 0],
        [101, 1],
        [100, 1],
        [100, 0]
      ]
    ]
  },
  "properties": {
    "prop0": "value0"
  }
}
```

### Supported libraries: Retrieve a feature

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Python CLI](https://github.com/mapbox/mapbox-cli-py#datasets-read-feature)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#getfeature)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Delete a feature

**DELETE** : `https://api.mapbox.com/datasets/v1/{username}/{dataset_id}/features/{feature_id}` [Token scope: **datasets:write**]

Remove a specific feature from a dataset.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which the dataset belongs. |
| `dataset_id` | `string` | The ID of the dataset from which to delete a feature. |
| `feature_id` | `string` | The ID of the feature to be deleted. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `datasets:write` scope. |

### Example request: Delete a feature

```bash
$ curl -X DELETE "https://api.mapbox.com/datasets/v1/YOUR_MAPBOX_USERNAME/{dataset_id}/features/{feature_id}?access_token=TOKEN_PLACEHOLDER::datasets:write"
```

### Response: Delete a feature

```
HTTP 204 No Content
```

### Supported libraries: Delete a feature

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Python CLI](https://github.com/mapbox/mapbox-cli-py#datasets-delete-feature)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#deletefeature)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Datasets API errors

| Response body `message` | HTTP status code | Description |
| --- | --- | --- |
| `Unauthorized` | `401` | Check the access token you used in the query. |
| `No such user` | `404` | Check the username you used in the query. |
| `Not found` | `404` | The access token used in the query needs the `datasets:list` [scope](https://docs.mapbox.com/accounts/guides/tokens/#scopes). |
| `Invalid start key` | `422` | Check the start key used in the query. |
| `No dataset` | `422` | Check the dataset ID used in the query (or, if you are retrieving a feature, check the feature ID). |

## Datasets API restrictions and limits

-   The Datasets API is limited on a per dataset basis.
-   The default *read* rate limit is 480 reads per minute. If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).
-   The default *write* rate limit is 40 writes per minute. If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).
-   If you exceed the rate limit, you will receive an `HTTP 429 Too Many Requests` response. For information on rate limit headers, see the [Rate limit headers](https://docs.mapbox.com/api/guides/#rate-limit-headers) section.
-   Dataset names are limited to 60 characters, and dataset descriptions are limited to 300 characters.
-   Each feature cannot exceed 1023 KB compressed. Features are compressed server-side using [geobuf](https://github.com/mapbox/geobuf).
-   Responses from the Datasets API set both the device and CDN TTLs to 5 minutes.
-   For general information on caching, see the [Maps APIs caching dive deeper guide](https://docs.mapbox.com/help/dive-deeper/api-caching/).