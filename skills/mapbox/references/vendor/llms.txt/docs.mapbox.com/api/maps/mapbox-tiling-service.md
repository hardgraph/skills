# Mapbox Tiling Service

**Mapbox Tiling Service (MTS)** is a tool for creating vector and raster tilesets. With MTS, you use sets of configuration options ([tileset recipes](https://docs.mapbox.com/help/glossary/tileset-recipe/)) to transform your geospatial data into vector tiles. The resulting tiles are hosted on Mapbox servers for use in your applications.

On this page you'll find the API documentation for interacting with MTS.

> **Note: Developer Guides and Examples for Mapbox Tiling Service**
> 
> For a general overview of MTS, its use cases, developer guides and examples, see the [Mapbox Tiling Service documentation](https://docs.mapbox.com/mapbox-tiling-service/guides/).

Besides accessing MTS using the API endpoints described here, you can also prepare and upload data for MTS using the *Tilesets CLI*, a command-line Python tool. For more information about the Tilesets CLI, see the [Tilesets CLI documentation](https://github.com/mapbox/tilesets-cli/) and the [Get started with MTS and the Tilesets CLI tutorial](https://docs.mapbox.com/help/tutorials/get-started-mts-and-tilesets-cli/).

## Create a tileset source

**POST** : `https://api.mapbox.com/tilesets/v1/sources/{username}/{id}` [Token scope: **tilesets:write**]

Creates a tileset source. A tileset source is raw geographic data formatted as [line-delimited GeoJSON](https://en.wikipedia.org/wiki/JSON_streaming#Line-delimited_JSON), or a supported [raster file format](https://docs.mapbox.com/mapbox-tiling-service/raster/supported-file-formats/) and uploaded to Mapbox.com. (Learn more about how line-delimited GeoJSON is used by MTS in the [Tileset sources guide](https://docs.mapbox.com/mapbox-tiling-service/overview/tileset-sources/#line-delimited-geojson).)

Tileset sources are necessary to use MTS to [create a new vector or raster tileset](#create-a-tileset), and they are referenced via a tileset source ID. The same tileset source can be used across multiple tilesets.

A tileset source can be composed of up to 10 source files. Each individual source file must not exceed 20 GB. The maximum combined total size of all files that compose a tileset source is 50 GB. If the total size of all the files that compose a tileset source is greater than 50 GB, MTS will return a response that contains an `error` property with more details. To add multiple source files to a tileset source, see the [Append to an existing tileset source endpoint](#append-to-an-existing-tileset-source). To replace a tileset source with new source files, use the [Replace a tileset source endpoint](#replace-a-tileset-source).

If you no longer need a tileset source, you should manually delete it after any related tilesets are finished processing using the [Delete a tileset source endpoint](#delete-a-tileset-source). The related tilesets will continue working normally.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The Mapbox username of the account for which to create a tileset source. |
| `id` | `string` | The ID for the tileset source to be created. Limited to 32 characters. The only allowed special characters are `-` and `_`. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:write` scope. |

The request body must be line-delimited GeoJSON or a supported [raster file format](https://docs.mapbox.com/mapbox-tiling-service/raster/supported-file-formats/). For information about how to convert GeoJSON or other data formats to line-delimited GeoJSON, see the [Tileset sources troubleshooting guide](https://docs.mapbox.com/help/troubleshooting/tileset-sources/#convert-geojson-to-line-delimited-geojson).

### Example request: Create a tileset source

```bash
$ curl -X POST "https://api.mapbox.com/tilesets/v1/sources/YOUR_MAPBOX_USERNAME/hello-world?access_token=TOKEN_PLACEHOLDER::tilesets:write" \
    -F file=@/Users/username/data/mts/countries.geojson.ld \
    --header "Content-Type: multipart/form-data"
```

### Response: Create a tileset source

If the request is successful, the response will contain the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `file_size` | `integer` | The size in bytes of the individual file you have added to your tileset source. |
| `files` | `integer` | The total number of files in the tileset source. |
| `id` | `string` | The unique identifier for the tileset source. |
| `source_size` | `integer` | The total size in bytes of all the files in the tileset source. |

#### Example response: Create a tileset source

```json
{
  "file_size": 10592,
  "files": 1,
  "id": "mapbox://tileset-source/username/hello-world",
  "source_size": 10592
}
```

## Append to an existing tileset source

**POST** : `https://api.mapbox.com/tilesets/v1/sources/{username}/{id}` [Token scope: **tilesets:write**]

Appends new source data to a tileset source, or creates a source if it does not exist already. A tileset source is raw geographic data formatted as [line-delimited GeoJSON](https://en.wikipedia.org/wiki/JSON_streaming#Line-delimited_JSON), or a supported [raster file format](https://docs.mapbox.com/mapbox-tiling-service/raster/supported-file-formats/) and uploaded to Mapbox.com. (Learn more about how line-delimited GeoJSON is used by MTS in the [Tileset sources guide](https://docs.mapbox.com/mapbox-tiling-service/overview/tileset-sources/#line-delimited-geojson).)

Tileset sources are necessary to use MTS to [create a new vector or vector tileset](#create-a-tileset), and they are referenced via a tileset source ID. The same tileset source can be used across multiple tilesets.

A tileset source can be composed of up to 10 source files. Each individual source file must not exceed 20 GB. The maximum combined total size of all files that compose a tileset source is 50 GB. If the total size of all the files that compose a tileset source is greater than 50 GB, MTS will return a response that contains an `error` property with more details. To add multiple source files to a tileset source, post to this endpoint multiple times. This will append the uploaded files to the tileset source. To replace a tileset source with new source files, use the [Replace a tileset source endpoint](#replace-a-tileset-source).

If you no longer need a tileset source, you should manually delete it after any related tilesets are finished processing using the [Delete a tileset source endpoint](#delete-a-tileset-source). The related tilesets will continue working normally.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The Mapbox username of the account for which to create a tileset source. |
| `id` | `string` | The ID for the tileset source to be append the new source data. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:write` scope. |

The request body must be line-delimited GeoJSON or a supported [raster file format](https://docs.mapbox.com/mapbox-tiling-service/raster/supported-file-formats/). For information about how to convert GeoJSON or other data formats to line-delimited GeoJSON, see the [Tileset sources troubleshooting guide](https://docs.mapbox.com/help/troubleshooting/tileset-sources/#convert-geojson-to-line-delimited-geojson).

### Example request: Append to an existing tileset source

```bash
$ curl -X POST "https://api.mapbox.com/tilesets/v1/sources/YOUR_MAPBOX_USERNAME/hello-world?access_token=TOKEN_PLACEHOLDER::tilesets:write" \
    -F file=@/Users/username/data/mts/countries.geojson.ld \
    --header "Content-Type: multipart/form-data"
```

### Response: Append to an existing tileset source

If the request is successful, the response will contain the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `file_size` | `integer` | The size in bytes of the individual file you have added to your tileset source. |
| `files` | `integer` | The total number of files in the tileset source. |
| `id` | `string` | The unique identifier for the tileset source. |
| `source_size` | `integer` | The total size in bytes of all the files in the tileset source. |

#### Example response: Append to an existing tileset source

```json
{
  "file_size": 10592,
  "files": 2,
  "id": "mapbox://tileset-source/username/hello-world",
  "source_size": 20884
}
```

## Replace a tileset source

**PUT** : `https://api.mapbox.com/tilesets/v1/sources/{username}/{id}` [Token scope: **tilesets:write**]

Replaces a tileset source with new source data, or creates a source if it does not exist already. If the total size of the uploaded file is greater than 20 GB, MTS will return a response that contains an `error` property with more details.

If you no longer need a tileset source, you should manually delete it after any related tilesets are finished processing using the [Delete a tileset source endpoint](#delete-a-tileset-source). The related tilesets will continue working normally.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The Mapbox username of the account for which to create a tileset source. |
| `id` | `string` | The ID for the tileset source to be replaced. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:write` scope. |

The request body must be line-delimited GeoJSON or a supported [raster file format](https://docs.mapbox.com/mapbox-tiling-service/raster/supported-file-formats/). For information about how to convert GeoJSON or other data formats to line-delimited GeoJSON, see the [Tileset sources troubleshooting guide](https://docs.mapbox.com/help/troubleshooting/tileset-sources/#convert-geojson-to-line-delimited-geojson).

### Example request: Replace a tileset source

```bash
$ curl -X PUT "https://api.mapbox.com/tilesets/v1/sources/YOUR_MAPBOX_USERNAME/hello-world?access_token=TOKEN_PLACEHOLDER::tilesets:write" \
    -F file=@/Users/username/data/mts/countries.geojson.ld \
    --header "Content-Type: multipart/form-data"
```

### Response: Replace a tileset source

If the request is successful, the response will contain the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `file_size` | `integer` | The size in bytes of the individual file you have added to your tileset source. |
| `files` | `integer` | The total number of files in the tileset source. |
| `id` | `string` | The unique identifier for the tileset source. |
| `source_size` | `integer` | The total size in bytes of all the files in the tileset source. |

#### Example response: Replace a tileset source

```json
{
  "file_size": 10592,
  "files": 1,
  "id": "mapbox://tileset-source/username/hello-world",
  "source_size": 10592
}
```

## Retrieve tileset source information

**GET** : `https://api.mapbox.com/tilesets/v1/sources/{username}/{id}` [Token scope: **tilesets:read**]

Get information for a specific tileset source, including the number and total size of the files in the tileset source.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The Mapbox username of the account for which to retrieve tileset source information. |
| `id` | `string` | The ID for the tileset source to be retrieved. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:read` scope. |

### Example request: Retrieve tileset source information

```bash
$ curl "https://api.mapbox.com/tilesets/v1/sources/YOUR_MAPBOX_USERNAME/hello-world?access_token=TOKEN_PLACEHOLDER::tilesets:read"
```

### Response: Retrieve tileset source information

If the request is successful, the response will contain the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `files` | `integer` | The total number of files in the tileset source. |
| `id` | `string` | The unique identifier for the tileset source. |
| `size` | `integer` | The total size in bytes of all files in the tileset source. |
| `size_nice` | `string` | The total size of all files in the tileset source, in a human-readable format. |

#### Example response: Retrieve tileset source information

```json
{
  "files": 2,
  "id": "mapbox://tileset-source/username/hello-world",
  "size": 20884,
  "size_nice": "20.39KB"
}
```

## List tileset sources

**GET** : `https://api.mapbox.com/tilesets/v1/sources/{username}` [Token scope: **tilesets:list**]

List all the tileset sources that belong to an account. This endpoint supports [pagination](https://docs.mapbox.com/api/guides/#pagination).

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The Mapbox username of the account for which to retrieve tileset source information. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:list` scope. |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `sortby` | `string` | Sort the listings by their `created` or `modified` timestamps. |
| `limit` | `integer` | The maximum number of tileset sources to return, from `1` to `500`. The default is `100`. |
| `start` | `string` | The tileset after which to start the listing. Find the key in the `Link` header of a response. See the [pagination](https://docs.mapbox.com/api/guides/#pagination) section for details. |

### Example request: List tileset sources

```bash
$ curl "https://api.mapbox.com/tilesets/v1/sources/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::tilesets:list"
```

### Response: List tileset sources

If the request is successful, the response will be a list of the tileset sources that belong to the specified account. Each individual source in the list will contain the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `files` | `integer` | The total number of files in the tileset source. |
| `id` | `string` | The unique identifier for the tileset source. |
| `size` | `integer` | The total size in bytes of all files in the tileset source. |

If the account has more than 2000 tileset sources, the response list will be capped at 2000.

#### Example response: List tileset sources

```json
[
  {
    "files": 2,
    "id": "mapbox://tileset-source/username/hello-world",
    "size": 20884
  },
  {
    "files": 3,
    "id": "mapbox://tileset-source/username/hola-mundo",
    "size": 650332
  }
]
```

## Delete a tileset source

**DELETE** : `https://api.mapbox.com/tilesets/v1/sources/{username}/{id}` [Token scope: **tilesets:write**]

Permanently delete a tileset source and all its files. This is not a recoverable action.

> **Note (warning): Don't delete a tileset source while it's in use**
> 
> If you delete a tileset source, any in-progress jobs that use that tileset source will fail.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The Mapbox username of the account for which to delete a tileset source. |
| `id` | `string` | The ID for the tileset source to be deleted. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:write` scope. |

### Example request: Delete a tileset source

```bash
$ curl -X DELETE "https://api.mapbox.com/tilesets/v1/sources/YOUR_MAPBOX_USERNAME/hello-world?access_token=TOKEN_PLACEHOLDER::tilesets:write"
```

### Response: Delete a tileset source

If the tileset source is successfully deleted, the response will be `HTTP 204 No Content`.

## Create a changeset

**POST** : `https://api.mapbox.com/tilesets/v1/changesets/{username}/{id}` [Token scope: **tilesets:write**]

Creates a changeset. A changeset is raw geographic data formatted as [line-delimited GeoJSON](https://en.wikipedia.org/wiki/JSON_streaming#Line-delimited_JSON) uploaded to Mapbox.com. Learn more about how line-delimited GeoJSON is used by MTS in the [Tileset sources guide](https://docs.mapbox.com/mapbox-tiling-service/overview/tileset-sources/#line-delimited-geojson).

Changesets are used to incrementally update vector tilesets enabled for incremental updates. They are referenced via a changeset ID. The same changeset can be used across multiple tilesets.

A changeset can be composed of up to 10 source files. Each individual source file must not exceed 20 GB. The maximum combined total size of all files that compose a tileset source is 50 GB. If the total size of all the files that compose a tileset source is greater than 50 GB, MTS will return a response that contains an `error` property with more details. To add multiple source files to a changeset, see the [Append to an existing changeset endpoint](#append-to-an-existing-changeset). To replace a changeset with new source files, use the [Replace a changeset endpoint](#replace-a-changeset).

If you no longer need a changeset, you should manually delete it after any related tilesets are finished processing using the [Delete a changeset endpoint](#delete-a-changeset). The related tilesets will continue working normally.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The Mapbox username of the account for which to create a changeset. |
| `id` | `string` | The ID for the changeset to be created. Limited to 32 characters. The only allowed special characters are `-` and `_`. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:write` scope. |

The request body must be line-delimited GeoJSON. For information about how to convert GeoJSON or other data formats to line-delimited GeoJSON, see the [Tileset sources troubleshooting guide](https://docs.mapbox.com/help/troubleshooting/tileset-sources/#convert-geojson-to-line-delimited-geojson).

### Example request: Create a changeset

```bash
$ curl -X POST "https://api.mapbox.com/tilesets/v1/changesets/YOUR_MAPBOX_USERNAME/hello-world?access_token=TOKEN_PLACEHOLDER::tilesets:write" \
    -F file=@/Users/username/data/mts/countries.geojson.ld \
    --header "Content-Type: multipart/form-data"
```

### Response: Create a changeset

If the request is successful, the response will contain the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `file_size` | `integer` | The size in bytes of the individual file you have added to your changeset. |
| `files` | `integer` | The total number of files in the changeset. |
| `id` | `string` | The unique identifier for the changeset. |
| `source_size` | `integer` | The total size in bytes of all the files in the changeset. |

#### Example response: Create a changeset

```json
{
  "file_size": 10592,
  "files": 1,
  "id": "mapbox://tileset-changeset/username/hello-world",
  "source_size": 10592
}
```

## Append to an existing changeset

**POST** : `https://api.mapbox.com/tilesets/v1/changesets/{username}/{id}` [Token scope: **tilesets:write**]

Appends new source data to a changeset, or creates a changeset if it does not exist already. A changeset is raw geographic data formatted as [line-delimited GeoJSON](https://en.wikipedia.org/wiki/JSON_streaming#Line-delimited_JSON) uploaded to Mapbox.com. (Learn more about how line-delimited GeoJSON is used by MTS in the [Tileset sources guide](https://docs.mapbox.com/mapbox-tiling-service/overview/tileset-sources/#line-delimited-geojson).)

changesets are necessary to update incrementally updatable MTS to [create a new vector or raster tileset](#create-a-tileset), and they are referenced via a changeset ID. The same changeset can be used across multiple tilesets.

A changeset can be composed of up to 10 source files. Each individual source file must not exceed 20 GB. The maximum combined total size of all files that compose a changeset is 50 GB. If the total size of all the files that compose a changeset is greater than 50 GB, MTS will return a response that contains an `error` property with more details. To add multiple source files to a changeset, post to this endpoint multiple times. This will append the uploaded files to the changeset. To replace a changeset with new source files, use the [Replace a changeset endpoint](#replace-a-changeset).

If you no longer need a changeset, you should manually delete it after any related tilesets are finished processing using the [Delete a tileset source endpoint](#delete-a-changeset). The related tilesets will continue working normally.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The Mapbox username of the account for which to create a changeset. |
| `id` | `string` | The ID for the changeset to be append the new source data. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:write` scope. |

The request body must be line-delimited GeoJSON. For information about how to convert GeoJSON or other data formats to line-delimited GeoJSON, see the [Tileset sources troubleshooting guide](https://docs.mapbox.com/help/troubleshooting/tileset-sources/#convert-geojson-to-line-delimited-geojson).

### Example request: Append to an existing changeset

```bash
$ curl -X POST "https://api.mapbox.com/tilesets/v1/changesets/YOUR_MAPBOX_USERNAME/hello-world?access_token=TOKEN_PLACEHOLDER::tilesets:write" \
    -F file=@/Users/username/data/mts/countries.geojson.ld \
    --header "Content-Type: multipart/form-data"
```

### Response: Append to an existing changeset

If the request is successful, the response will contain the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `file_size` | `integer` | The size in bytes of the individual file you have added to your changeset. |
| `files` | `integer` | The total number of files in the changeset. |
| `id` | `string` | The unique identifier for the changeset. |
| `source_size` | `integer` | The total size in bytes of all the files in the changeset. |

#### Example response: Append to an existing changeset

```json
{
  "file_size": 10592,
  "files": 2,
  "id": "mapbox://tileset-changeset/username/hello-world",
  "source_size": 20884
}
```

## Replace a changeset

**PUT** : `https://api.mapbox.com/tilesets/v1/changesets/{username}/{id}` [Token scope: **tilesets:write**]

Replaces a changeset with new source data, or creates a source if it does not exist already. If the total size of the uploaded file is greater than 20 GB, MTS will return a response that contains an `error` property with more details.

If you no longer need a changeset, you should manually delete it after any related tilesets are finished processing using the [Delete a changeset endpoint](#delete-a-changeset). The related tilesets will continue working normally.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The Mapbox username of the account for which to create a changeset. |
| `id` | `string` | The ID for the changeset to be replaced. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:write` scope. |

The request body must be line-delimited GeoJSON. For information about how to convert GeoJSON or other data formats to line-delimited GeoJSON, see the [Tileset sources troubleshooting guide](https://docs.mapbox.com/help/troubleshooting/tileset-sources/#convert-geojson-to-line-delimited-geojson).

### Example request: Replace a changeset

```bash
$ curl -X PUT "https://api.mapbox.com/tilesets/v1/changesets/YOUR_MAPBOX_USERNAME/hello-world?access_token=TOKEN_PLACEHOLDER::tilesets:write" \
    -F file=@/Users/username/data/mts/countries.geojson.ld \
    --header "Content-Type: multipart/form-data"
```

### Response: Replace a changeset

If the request is successful, the response will contain the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `file_size` | `integer` | The size in bytes of the individual file you have added to your changeset. |
| `files` | `integer` | The total number of files in the changeset. |
| `id` | `string` | The unique identifier for the changeset. |
| `source_size` | `integer` | The total size in bytes of all the files in the changeset. |

#### Example response: Replace a changeset

```json
{
  "file_size": 10592,
  "files": 1,
  "id": "mapbox://tileset-changeset/username/hello-world",
  "source_size": 10592
}
```

## List changesets

**GET** : `https://api.mapbox.com/tilesets/v1/changesets/{username}` [Token scope: **tilesets:list**]

List all the changesets that belong to an account. This endpoint supports [pagination](https://docs.mapbox.com/api/guides/#pagination).

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The Mapbox username of the account for which to retrieve changeset information. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:list` scope. |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `sortby` | `string` | Sort the listings by their `created` or `modified` timestamps. |
| `limit` | `integer` | The maximum number of changesets to return, from `1` to `500`. The default is `100`. |
| `start` | `string` | The tileset after which to start the listing. Find the key in the `Link` header of a response. See the [pagination](https://docs.mapbox.com/api/guides/#pagination) section for details. |

### Example request: List changesets

```bash
$ curl "https://api.mapbox.com/tilesets/v1/changesets/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::tilesets:list"
```

### Response: List changesets

If the request is successful, the response will be a list of the changesets that belong to the specified account. Each individual source in the list will contain the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `files` | `integer` | The total number of files in the changeset. |
| `id` | `string` | The unique identifier for the changeset. |
| `size` | `integer` | The total size in bytes of all files in the changeset. |

If the account has more than 2000 changesets, the response list will be capped at 2000.

#### Example response: List changesets

```json
[
  {
    "files": 2,
    "id": "mapbox://tileset-changeset/username/hello-world",
    "size": 20884
  },
  {
    "files": 3,
    "id": "mapbox://tileset-changeset/username/hola-mundo",
    "size": 650332
  }
]
```

## Delete a changeset

**DELETE** : `https://api.mapbox.com/tilesets/v1/changesets/{username}/{id}` [Token scope: **tilesets:write**]

Permanently delete a changeset and all its files. This is not a recoverable action.

> **Note (warning): Don't delete a changeset while it's in use**
> 
> If you delete a changeset, any in-progress update jobs that use that changeset source will fail.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The Mapbox username of the account for which to delete a changeset. |
| `id` | `string` | The ID for the changeset to be deleted. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:write` scope. |

### Example request: Delete a changeset

```bash
$ curl -X DELETE "https://api.mapbox.com/tilesets/v1/changesets/YOUR_MAPBOX_USERNAME/hello-world?access_token=TOKEN_PLACEHOLDER::tilesets:write"
```

### Response: Delete a changeset

If the changeset is successfully deleted, the response will be `HTTP 204 No Content`.

## Create a tileset

**POST** : `https://api.mapbox.com/tilesets/v1/{tileset_id}` [Token scope: **tilesets:write**]

> **Note: Prerequisites for creating a new tileset**
> 
> ***Before you can create a new tileset, you need to create a [tileset source](#create-a-tileset-source) and write a [tileset recipe](#validate-a-recipe).*** The tileset recipe defines how to transform the data in the tileset source into vector tiles.

Create a new tileset.

| Required parameters | Type | Description |
| --- | --- | --- |
| `tileset_id` | `string` | The ID for the tileset to be created, which is composed of your username followed by a period and the tileset's unique name (`username.id`). Limited to 32 characters. This character limit does not include your username. The only allowed special characters are `-` and `_`. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:write` scope. |

The request body must be a JSON object that contains the following properties:

| Required request body property | Type | Description |
| --- | --- | --- |
| `recipe` | `object` | A recipe that describes how the GeoJSON data you uploaded should be transformed into tiles. For more information on how to create and format recipes, see the [Recipe reference](https://docs.mapbox.com/mapbox-tiling-service/reference/) and [Recipe examples](https://docs.mapbox.com/mapbox-tiling-service/examples/). |
| `name` | `string` | The name of the tileset. Limited to 64 characters. |

Additionally, the request body may contain the following optional properties:

| Optional request body properties | Type | Description |
| --- | --- | --- |
| `private` | `boolean` | Describes whether the tileset must be used with an access token from your Mapbox account. Default is `true`. |
| `description` | `string` | A description of the tileset. Limited to 500 characters. |
| `attribution` | array of objects | An array of attribution objects, each with `text` and `link` keys. Limited to three attribution objects, 80 characters maximum combined across all `text` values, and 1000 characters maximum combined across all `link` values. |
| `attribution.text` | `string` | The attribution text for the tileset. |
| `attribution.link` | `string` | The URL used for the tileset's attribution. |

### Example request: Create a tileset

```bash
$ curl -X POST "https://api.mapbox.com/tilesets/v1/{tileset_id}?access_token=TOKEN_PLACEHOLDER::tilesets:write" \
  -d @tileset-information-and-recipe.json \
  --header "Content-Type:application/json"
```

### Example request body: Create a tileset

```json
{
  "recipe": {
    "version": 1,
    "layers": {
      "hello_world": {
        "source": "mapbox://tileset-source/username/hello-world",
        "minzoom": 0,
        "maxzoom": 5
      }
    }
  },
  "name": "Hello World",
  "description": "Spaceship Earth with all of the people, places, and things.",
  "attribution": [{ "text": "© Hello Legal World", "link": "" }]
}
```

### Response: Create a tileset

```json
{
  "message": "Successfully created empty tileset <tileset>. Publish your tileset to begin processing your data into vector tiles."
}
```

If a tileset with the specified ID already exists, MTS will return an `HTTP 400` status code.

## Update a tileset

You can update an existing tileset's source, recipe, metadata, and tiles using MTS:

-   **Update a tileset's source** using the [Update a tileset's recipe](#update-a-tilesets-recipe) endpoint. You can reference a new tileset source in the updated tileset recipe.
-   **Update a tileset's recipe** using the [Update a tileset's recipe](#update-a-tilesets-recipe) endpoint. The updated recipe can specify a new `minzoom`, `maxzoom`, and more following the rules in the [MTS recipe specification](https://docs.mapbox.com/mapbox-tiling-service/reference/).
-   **Update a tileset's metadata** using the [Update tileset information](#update-tileset-information) MTS endpoint. You can update a tileset's name, description, private or public state, and attribution information.
-   **Update a tileset's tiles** using the [Publish a tileset](#publish-a-tileset) MTS endpoint.

## Publish a tileset

**POST** : `https://api.mapbox.com/tilesets/v1/{tileset_id}/publish` [Token scope: **tilesets:write**]

Once you've created a tileset, you can request that the data be published into vector tiles. This action will start an asynchronous process known as a **job** that retrieves your data and processes it into vector tiles according to the recipe you have defined.

This endpoint can also be used to *update* an existing tileset or to *reset* a tileset enabled for incremental updates. Note that due to tile caching, when updating an existing tileset new tiles will only become visible as the cached versions of those tiles expire.

A given tileset can only have one active processing publish job at a time. This ensures the data you have staged is processed before any future data you have staged is processed. If your tileset has an active publish request being processed, all later publish requests will be queued to run in the order they were received. You can only have five jobs in the queue per account.

All jobs are entered into a global Mapbox queue. The larger the queue, the longer it will take your tileset to process. For instructions on how to see the size of the queue, see the [View the global queue](#view-the-global-queue) section.

**The size of a single vector tile is limited to 500 KB.** If a job drops features due to tile size, this will be noted in the `warnings` field of the job object.

| Required parameters | Type | Description |
| --- | --- | --- |
| `tileset_id` | `string` | The ID of the tileset to be published, which is composed of your username followed by a period and the tileset's unique name (`username.id`). |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:write` scope. |

### Processing time

There are many factors that can influence the processing time when you publish a tileset, including:

-   The size of the geographical area represented by the data that is being processed. For example, data for a city block will be processed much faster than data for the entire earth.
-   The maximum zoom level applied to each layer. The higher the maximum zoom level, the more tiles that need to be created. Each additional zoom level creates four times as many tiles as the previous zoom level did. As an example, a maximum zoom level of `1` produces four tiles, while a maximum zoom level of `2` produces 16 tiles.
-   The complexity of the [tileset recipe](https://docs.mapbox.com/help/glossary/tileset-recipe/), in particular [filters](https://docs.mapbox.com/mapbox-tiling-service/reference/#feature-filters), [attributes](https://docs.mapbox.com/mapbox-tiling-service/reference/#feature-attributes), and [union configurations](https://docs.mapbox.com/mapbox-tiling-service/reference/#feature-union) will likely add to the processing time.
-   The size of the global queue, which you can check with the [View the global queue endpoint](#view-the-global-queue). MTS global queue indicates the number of tileset jobs waiting to be processed.

### Example request: Publish a tileset

```bash
$ curl -X POST "https://api.mapbox.com/tilesets/v1/{tileset_id}/publish?access_token=TOKEN_PLACEHOLDER::tilesets:write"
```

### Response: Publish a tileset

```json
{
  "message": "Processing <tileset>",
  "jobId": "<job_id>"
}
```

### Response: Publish a tileset using incremental updates

```json
{
  "message": "Processing <tileset>",
  "jobId": "<job_id>",
  "incremental": "initial"
}
```

### Response: Re-publish (reset) a tileset using incremental updates

```json
{
  "message": "Processing <tileset>",
  "jobId": "<job_id>",
  "incremental": "reset"
}
```

If your recipe references a non-existent tileset source, MTS will return an `HTTP 400` status code.

## Publish changesets

**POST** : `https://api.mapbox.com/tilesets/v1/{tileset_id}/publish-changesets` [Token scope: **tilesets:write**]

Once you have enabled incremental updates on a tileset, you can request that the changeset be published into vector tiles. This action will start an asynchronous process known as a **job** that retrieves your data and processes it into vector tiles according to the recipe you have defined.

This endpoint is the only way to incrementally update a tileset. The tileset recipe must be unchanged since the initial publish or reset job. Note that due to tile caching, when updating an existing tileset new tiles will only become visible as the cached versions of those tiles expire.

A given tileset can only have one active processing publish job at a time. This ensures the data you have staged is processed before any future data you have staged is processed. If your tileset has an active publish request being processed, all later publish requests will be queued to run in the order they were received. You can only have five jobs in the queue per account.

All jobs are entered into a global Mapbox queue. The larger the queue, the longer it will take your tileset to process. For instructions on how to see the size of the queue, see the [View the global queue](#view-the-global-queue) section.

**The size of a single vector tile is limited to 500 KB.** If a job drops features due to tile size, this will be noted in the `warnings` field of the job object.

| Required parameters | Type | Description |
| --- | --- | --- |
| `tileset_id` | `string` | The ID of the tileset to be updated, which is composed of your username followed by a period and the tileset's unique name (`username.id`). |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:write` scope. |

### Processing time

There are many factors that can influence the processing time when you publish a tileset, including:

-   The size of the geographical area represented by the data that is being processed. For example, data for a city block will be processed much faster than data for the entire earth.
-   The maximum zoom level applied to each layer. The higher the maximum zoom level, the more tiles that need to be created. Each additional zoom level creates four times as many tiles as the previous zoom level did. As an example, a maximum zoom level of `1` produces four tiles, while a maximum zoom level of `2` produces 16 tiles.
-   The complexity of the [tileset recipe](https://docs.mapbox.com/help/glossary/tileset-recipe/), in particular [filters](https://docs.mapbox.com/mapbox-tiling-service/reference/#feature-filters), [attributes](https://docs.mapbox.com/mapbox-tiling-service/reference/#feature-attributes), and [union configurations](https://docs.mapbox.com/mapbox-tiling-service/reference/#feature-union) will likely add to the processing time.
-   The size of the global queue, which you can check with the [View the global queue endpoint](#view-the-global-queue). MTS global queue indicates the number of tileset jobs waiting

```bash
$ curl -X POST "https://api.mapbox.com/tilesets/v1/{tileset_id}/publish-changesets?access_token=TOKEN_PLACEHOLDER::tilesets:write" \
  -d @incremental-update.json \
  --header "Content-Type:application/json"
```

**Request body - incremental-update.json file**

The `incremental-update.json` file in the request body should contain the tileset layers and corresponding changesets for the incremental update:

```json
{
  "layers": {
    "{layer_name}": {
      "changeset": "mapbox://tileset-changeset/{username}/{changeset_id}"
    },
    "{layer_name}": {
      "changeset": "mapbox://tileset-changeset/{username}/{changeset_id}"
    }
  }
}
```

### Response: Publish a changeset

```json
{
  "message": "Processing <tileset>",
  "jobId": "<job_id>"
}
```

MTS will return an `HTTP 400` status code

-   If the tileset doesn't exist
-   If your recipe references a non-existent changeset
-   If the publish changeset payload doesn't have the `layers` property set or it isn't an object
-   If the publish changeset payload references a layer which doesn't exist in the tileset to be updated
-   If the layer in the publish changeset payload doesn't have the `changeset` property set
-   If the recipe of the tileset has changed since the most recent incremental initial or reset job

MTS will return an `HTTP 404` status code

-   If the tileset hasn't been published yet
-   If the tileset wasn't published as an incrementally updatable tileset

MTS will return an `HTTP 422` status code when all the update layers are empty (0 length).

## Update tileset information

**PATCH** : `https://api.mapbox.com/tilesets/v1/{tileset_id}` [Token scope: **tilesets:write**]

Update a tileset's information such as name, description, and privacy settings. This is **not** the endpoint for updating a tileset's [recipe](#update-a-tilesets-recipe), [sources](#create-a-tileset-source), or [tiles](#publish-a-tileset).

| Optional request body properties | Type | Description |
| --- | --- | --- |
| `name` | `string` | The name of the tileset. Limited to 64 characters. |
| `description` | `string` | A description of the tileset. Limited to 500 characters. |
| `private` | `boolean` | Describes whether the tileset must be used with an access token from your Mapbox account. |
| `attribution` | array of objects | An array of attribution objects, each with `text` and `link` keys. Limited to three attribution objects, 80 characters maximum combined across all `text` values, and 1000 characters maximum combined across all `link` values. For more details on how attribution is displayed when multiple tilesets with custom attribution are composited together, see the [attribution display section](#attribution-display-with-multiple-tilesets). |
| `attribution.text` | `string` | The attribution text for the tileset. |
| `attribution.link` | `string` | The URL used for the tileset's attribution. |

### Attribution display with multiple tilesets

Custom map attribution strings are composited in reverse order of the composite request and are separated by a space character. For example, the following request to `mapbox.mapbox-streets-v8,example.custom-tileset` (in which the first tileset is a Mapbox tileset and the second tileset is a custom tileset) will show attribution that looks like:

```
© Example Maps 2020 © Mapbox © OpenStreetMap
```

### Example request: Update a tileset

```bash
$ curl -X PATCH "https://api.mapbox.com/tilesets/v1/{tileset_id}?access_token=TOKEN_PLACEHOLDER::tilesets:write" \
  --data '{"name":"New name same me","attribution":[{"text":"New data license","link":"https://example.com"}]}' \
  --header "Content-Type:application/json"
```

### Response: Update a tileset

```
HTTP 204 No Content
```

## Retrieve information about a single tileset job

**GET** : `https://api.mapbox.com/tilesets/v1/{tileset_id}/jobs/{job_id}` [Token scope: **tilesets:read**]

Retrieve information about a single job associated with a tileset, based on its unique job ID.

| Required parameters | Type | Description |
| --- | --- | --- |
| `tileset_id` | `string` | The ID for the tileset for which to retrieve information, which is composed of your username followed by a period and the tileset's unique name (`username.id`). |
| `job_id` | `string` | The publish job's unique identifier. This identifier is returned in the `jobId` field of a [Publish a tileset](#publish-a-tileset) response. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:read` scope. |

### Example request: Retrieve information about a single tileset job

```bash
$ curl "https://api.mapbox.com/tilesets/v1/{tileset_id}/jobs/{job_id}?access_token=TOKEN_PLACEHOLDER::tilesets:read"
```

### Response: Retrieve information about a single tileset job

A successful request will return a JSON response that contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `id` | `string` | The publish job's unique identifier. |
| `stage` | `string` | The status of the job. Possible values are `queued`, `processing`, `success`, `failed`, or `superseded`. The `superseded` stage is reserved for queued jobs that are replaced, for a newer job on that tileset to process without waiting for old queued job to finish. |
| `created` | `integer` | Timestamp indicating when the job was created. |
| `created_nice` | `string` | Human-readable timestamp indicating when the job was created. |
| `published` | `integer` | Timestamp indicating when the job was published. |
| `tilesetId` | `string` | The specified tileset's unique identifier. |
| `errors` | `array` | The errors related to the tileset, if relevant. If there are no errors, this will be an empty array. For more information, see our [MTS errors documentation](https://docs.mapbox.com/mapbox-tiling-service/overview/errors/#mapbox-tiling-service-errors). |
| `warnings` | `array` | Any warnings related to the tileset, if relevant. If there are no warnings, this will be an empty array. For more information about warnings, see our [MTS warnings documentation](https://docs.mapbox.com/mapbox-tiling-service/overview/warnings/#mapbox-tiling-service-warnings). |
| `recipe` | `object` | The MTS [recipe](https://docs.mapbox.com/mapbox-tiling-service/guides/tileset-recipes/) that was used to publish the job. |
| `tileset_precisions` | `object` | An object listing the processed area in square kilometers for the precision tier (10m, 1m, 30cm, or 1cm) used by one or more layers in the job's recipe. |
| `cu` | `integer` | The number of processing compute units (CUs) used by the job. |
| `filesize` | `integer` | The file size in bytes of the tileset source used to publish the job. |
| `layer_stats` | `object` | An object that contains statistics about the given layer. |
| `layer_stats.total_tiles` | `integer` | The total number of tiles in the tileset that contain this layer. |
| `layer_stats.point_count` | `integer` | The total number of point features for the layer, multiplied by the number of tiles there are in the tileset. |
| `layer_stats.linestring_count` | `integer` | The total number of linestring features for the layer, multiplied by the number of tiles there are in the tileset. |
| `layer_stats.polygon_count` | `integer` | The total number of polygon features for the layer, multiplied by the number of tiles there are in the tileset. |
| `layer_stats.capped` | `integer` | The number of tiles in which features were no longer added to the layer during tiling due to the size of the layer. |
| `layer_stats.maxzoom` | `integer` | The maximum zoom level of the layer. |
| `layer_stats.minzoom` | `integer` | The minimum zoom level of the layer. |
| `layer_stats.checksum` | `string` | A checksum of the geometry and attributes of the features in the layer, including the order of the features but not including their IDs. If two checksums are the same, the tiles from the two tiling jobs are almost certainly identical, but identical recipes and sources are not strictly guaranteed to produce identical checksums. |
| `layer_stats.zooms` | `object` | An object that contains statistics for each zoom level in the layer: |
| `layer_stats.zooms.ymin` | `integer` | The minimum `y` tile coordinate at the zoom level. |
| `layer_stats.zooms.ymax` | `integer` | The maximum `y` tile coordinate at the zoom level. |
| `layer_stats.zooms.xmin` | `integer` | The minimum `x` tile coordinate at the zoom level. |
| `layer_stats.zooms.xmax` | `integer` | The maximum `x` tile coordinate at the zoom level. |
| `layer_stats.zooms.tiles` | `integer` | The total number of tiles at this zoom level for the layer. |
| `layer_stats.zooms.capped` | `integer` | The number of tiles in which features were no longer added during tiling due to the size of the layer at this zoom level. |
| `layer_stats.zooms.capped_list` | array of objects | A list of tiles in this zoom level that exceeded the `layer_size`. The list may be truncated if large numbers of tiles exceeded the limit. Each element in the `capped_list` is an object containing a `tile` (referred to by its zoom/x/y coordinates) and the `layer_size` that would have been required for that tile to have avoided being capped (or `"more than maximum"` if the required size exceeds system limits). |
| `layer_stats.zooms.sum_area` | `number` | The total area that tiles cover at this zoom level for the layer. |
| `layer_stats.zooms.sum_size` | `integer` | The total size in bytes of all tiles for this layer at this zoom level, uncompressed. |
| `layer_stats.zooms.avg_size` | `number` | The average size of a tile for this layer at this zoom level, uncompressed. |
| `layer_stats.zooms.max_size` | `integer` | The maximum size of a tile at this zoom level for this layer. |
| `layer_stats.zooms.size_histogram` | `array` | An array of 20 counts of tiles that shows the distribution of tile layer sizes. The first count is the number of tiles between 1 byte and 25 kilobytes, with each subsequent count representing another 25 kilobyte range, up to 500 kilobytes. Any tile layers larger than 500 kilobytes are also included in the last count. |

#### Example response: Retrieve information about a single tileset job

```json
{
  "id": "unique_hash",
  "stage": "success",
  "created": 1560981902377,
  "created_nice": "Wed Jun 19 2019 22:05:02 GMT+0000 (UTC)",
  "published": 1560982158721,
  "tilesetId": "user.id",
  "errors": [],
  "warnings": [],
  "tileset_precisions": { "1m": 658731.7540137176 },
  "cu": 3.3,
  "filesize": 1068607,
  "layer_stats": {
    "sample_pois": {
      "total_tiles": 71,
      "linestring_count": 0,
      "capped": 15,
      "maxzoom": 12,
      "zooms": {
        "0": {
          "ymin": 0,
          "ymax": 0,
          "xmin": 0,
          "xmax": 0,
          "tiles": 1,
          "capped": 1,
          "capped_list": [{ "layer_size": 1337, "tile": "0/0/0" }],
          "sum_size": 512036,
          "sum_area": 508164394.24620897,
          "avg_size": 512036,
          "max_size": 512036,
          "size_histogram": [
            0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1
          ]
        }
      }
    }
  },
  "recipe": {
    "version": 1,
    "layers": {
      "sample_pois": {
        "minzoom": 1,
        "maxzoom": 12,
        "source": "mapbox://tileset-source/user/source"
      }
    }
  }
}
```

## List information about all jobs for a tileset

**GET** : `https://api.mapbox.com/tilesets/v1/{tileset_id}/jobs` [Token scope: **tilesets:list**]

List information about all jobs associated with a tileset. You can also use this endpoint to query jobs at a specific processing stage: `processing`, `queued`, `success`, `failed`, or `superseded`. This endpoint supports [pagination](https://docs.mapbox.com/api/guides/#pagination).

| Required parameter | Type | Description |
| --- | --- | --- |
| `tileset_id` | `string` | The ID for the tileset for which to list all jobs, which is composed of your username followed by a period and the tileset's unique name (`username.id`). |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:list` scope. |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameter | Type | Description |
| --- | --- | --- |
| `stage` | `string` | Query for jobs at a specific processing stage: `processing`, `queued`, `success`, `failed`, or `superseded`. |
| `limit` | `integer` | The maximum number of tilesets to return, from `1` to `500`. The default is `100`. |
| `start` | `string` | The tileset after which to start the listing. Find the key in the `Link` header of a response. See the [pagination](https://docs.mapbox.com/api/guides/#pagination) section for details. |

### Example request: List information about all jobs for a tileset

```bash
# Request all associated jobs
$ curl "https://api.mapbox.com/tilesets/v1/{tileset_id}/jobs?access_token=TOKEN_PLACEHOLDER::tilesets:list"

# Request only associated jobs at the success stage
$ curl "https://api.mapbox.com/tilesets/v1/{tileset_id}/jobs?stage=success&access_token=TOKEN_PLACEHOLDER::tilesets:list"
```

### Response: List information about all jobs for a tileset

A successful request returns one or more JSON objects that describe a tileset's jobs. Each object will contain the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `id` | `string` | The publish job's unique identifier. |
| `stage` | `string` | The status of the job. Possible values are `queued`, `processing`, `success`, `failed`, or `superseded`. |
| `created` | `integer` | Timestamp indicating when the job was created. |
| `created_nice` | `string` | Human-readable timestamp indicating when the job was created. |
| `published` | `integer` | Timestamp indicating when the job was published. |
| `tilesetId` | `string` | The specified tileset's unique identifier. |
| `errors` | `array` | The errors related to the tileset, if relevant. If there are no errors, this will be an empty array. For more information, see our [MTS errors documentation](https://docs.mapbox.com/mapbox-tiling-service/overview/errors/#mapbox-tiling-service-errors). |
| `warnings` | `array` | Any warnings related to the tileset, if relevant. If there are no warnings, this will be an empty array. For more information about warnings, see our [MTS warnings documentation](https://docs.mapbox.com/mapbox-tiling-service/overview/warnings/#mapbox-tiling-service-warnings). |
| `recipe` | `object` | The MTS [recipe](https://docs.mapbox.com/mapbox-tiling-service/guides/tileset-recipes/) that was used to publish the job. |
| `tileset_precisions` | `object` | Lists the processed area in square kilometers for the precision tier (10m, 1m, 30cm, or 1cm) used by one or more layers in the job's recipe. |
| `cu` | `integer` | The number of processing compute units (CUs) used by the job. |
| `filesize` | `integer` | The file size in bytes of the tileset source used to publish the job. |

#### Example response: List information about all jobs for a tileset

```json
[
  {
    "id": "job_1_id",
    "stage": "success",
    "created": 1560981902377,
    "created_nice": "Wed Jun 19 2019 22:05:02 GMT+0000 (UTC)",
    "published": 1560982158721,
    "tilesetId": "user.id",
    "errors": [],
    "warnings": [],
    "tileset_precisions": { "1m": 658731.7540137176 },
    "cu": 3.3,
    "filesize": 1068607,
    "recipe": {
      "version": 1,
      "layers": {
        "sample_pois": {
          "minzoom": 1,
          "maxzoom": 12,
          "source": "mapbox://tileset-source/user/source"
        }
      }
    }
  },
  {
    "id": "job_2_id",
    "stage": "processing",
    "created": 1560982159327,
    "created_nice": "Wed Jun 19 2019 22:09:19 GMT+0000 (UTC)",
    "published": 1560985238565,
    "tilesetId": "user.id",
    "errors": [],
    "warnings": [],
    "recipe": {
      "version": 1,
      "layers": {
        "sample_pois": {
          "minzoom": 1,
          "maxzoom": 12,
          "source": "mapbox://tileset-source/user/revised-source"
        }
      }
    }
  }
]
```

## View the global queue

**PUT** : `https://api.mapbox.com/tilesets/v1/queue` [Token scope: **tilesets:read**]

View the number of queued jobs in the global queue, which shows the number of jobs waiting to be processed.

### Example request: View the global queue

```bash
$ curl -X PUT "https://api.mapbox.com/tilesets/v1/queue?access_token=TOKEN_PLACEHOLDER::tilesets:read"
```

### Response: View the global queue

A request to this endpoint returns the number of jobs waiting to be processed in the global MTS queue.

| Property | Type | Description |
| --- | --- | --- |
| `total` | `integer` | The number of queued jobs. |

#### Example response: View the global queue

```json
{
  "total": 42
}
```

## Validate a recipe

**PUT** : `https://api.mapbox.com/tilesets/v1/validateRecipe` [Token scope: **tilesets:write**]

Validate a recipe document before using it to create a new tileset. The entire request body must be the recipe JSON document, which is the `recipe` part of the request body property of a [tileset creation request](#create-a-tileset). For guidance on how to format a recipe, see the [Recipe reference](https://docs.mapbox.com/mapbox-tiling-service/reference/).

You can control the behavior of this endpoint with the following optional parameter:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `accept_invalid_sources` | `boolean` | If `true`, do not validate the `source` URL for each layer. This allows you to validate the syntax of a recipe while it is being developed before the sources are uploaded. |

### Example request: Validate a recipe

```bash
$ curl -X PUT "https://api.mapbox.com/tilesets/v1/validateRecipe?access_token=TOKEN_PLACEHOLDER::tilesets:write" \
  -d @recipe.json \
  --header "Content-Type:application/json"
```

### Response: Validate a recipe

The response will be a JSON object that tells you whether the recipe document is valid or not. If the recipe document is not valid, the `errors` property will contain an informational message about the issue.

#### Example response: Validate a recipe

**Valid recipe:**

```json
{
  "valid": true
}
```

**Recipe not valid:**

```json
{
  "valid": false,
  "errors": ["minzoom 22 cannot be larger than maxzoom 11"]
}
```

## Retrieve a tileset's recipe

**GET** : `https://api.mapbox.com/tilesets/v1/{tileset_id}/recipe` [Token scope: **tilesets:list**]

Request the recipe body that you used when you created a specific tileset.

| Required parameter | Type | Description |
| --- | --- | --- |
| `tileset_id` | `string` | The ID for the tileset for which to retrieve the recipe, which is composed of your username followed by a period and the tileset's unique name (`username.id`). |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:list` scope. |

### Example request: Retrieve a tileset's recipe

```bash
$ curl "https://api.mapbox.com/tilesets/v1/{tileset_id}/recipe?access_token=TOKEN_PLACEHOLDER::tilesets:list"
```

### Response: Retrieve a tileset's recipe

Returns the recipe body you provided when you originally created the specified tileset.

#### Example response: Retrieve a tileset's recipe

```json
{
  "recipe": {
    "version": 1,
    "layers": {
      "my_layer": {
        "source": "mapbox://tileset-source/my-source-data",
        "minzoom": 0,
        "maxzoom": 4
      }
    }
  },
  "id": "username.id"
}
```

## Update a tileset's recipe

**PATCH** : `https://api.mapbox.com/tilesets/v1/{tileset_id}/recipe` [Token scope: **tilesets:write**]

Update a tileset's recipe. This endpoint does a validation step on the new recipe.

| Required parameter | Type | Description |
| --- | --- | --- |
| `tileset_id` | `string` | The ID for the tileset for which you are updating the recipe, which is composed of your username followed by a period and the tileset's unique name (`username.id`). |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:write` scope. |

The entire request body must be the recipe JSON document, which is the `recipe` part of the request body property of a [tileset creation request](#create-a-tileset).

> **Note: Updates to the layer name**
> 
> Updating the `layer_name` of your recipe will break any downstream styles that reference this tileset. If a style references this tileset and you change the `layer_name`, you must update your style by deleting the relevant layer and re-adding the tileset's new layer.

### Example request: Update a tileset's recipe

```bash
$ curl -X PATCH "https://api.mapbox.com/tilesets/v1/{tileset_id}/recipe?access_token=TOKEN_PLACEHOLDER::tilesets:write" \
  -d @recipe.json \
  --header "Content-Type:application/json"
```

### Response: Update a tileset's recipe

If the updated recipe is valid, the response will be `HTTP 204 No Content`.

If the updated recipe is not valid, the `errors` property will contain an informational message about the issue:

```json
{
  "message": "Recipe is invalid.",
  "errors": ["minzoom 22 cannot be larger than maxzoom 11"]
}
```

## List tilesets

**GET** : `https://api.mapbox.com/tilesets/v1/{username}` [Token scope: **tilesets:list**]

List all the tilesets that belong to a specific account. This endpoint supports [pagination](https://docs.mapbox.com/api/guides/#pagination). It returns a maximum of 100 tilesets by default.

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account for which to list tilesets |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:list` scope. |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `type` | `string` | Filter results by tileset type, either `raster`, `rasterarray`, or `vector`. |
| `visibility` | `string` | Filter results by visibility, either `public` or `private`. Private tilesets require an access token that belong to the owner. Public tilesets can be requested with any user's access token. |
| `sortby` | `string` | Sort the listings by their `created` or `modified` timestamps. |
| `limit` | `integer` | The maximum number of tilesets to return, from `1` to `500`. The default is `100`. |
| `start` | `string` | The tileset after which to start the listing. Find the key in the `Link` header of a response. See the [pagination](https://docs.mapbox.com/api/guides/#pagination) section for details. |

### Example request: List tilesets

```bash
$ curl "https://api.mapbox.com/tilesets/v1/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::tilesets:list"

# Limit the results to the 25 most recently created vector tilesets

$ curl "https://api.mapbox.com/tilesets/v1/YOUR_MAPBOX_USERNAME?type=vector&limit=25&sortby=created&access_token=TOKEN_PLACEHOLDER::tilesets:list"

# Limit the results to the tilesets after the tileset with a start key of abc123

$ curl "https://api.mapbox.com/tilesets/v1/YOUR_MAPBOX_USERNAME?start=abc123&access_token=TOKEN_PLACEHOLDER::tilesets:list"
```

### Response: List tilesets

A request to MTS returns an array of tileset objects. Each tileset object contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `type` | `string` | The kind of data contained, either `raster`, `rasterarray`, or `vector`. |
| `center` | `array` | The longitude, latitude, and zoom level for the center of the contained data, given in the format `[lon, lat, zoom]`. |
| `created` | `string` | A timestamp indicating when the tileset was created. |
| `description` | `string` | A human-readable description of the tileset. |
| `filesize` | `integer` | The storage in bytes consumed by the tileset. |
| `id` | `string` | The unique identifier for the tileset. |
| `modified` | `string` | A timestamp indicating when the tileset was last modified. |
| `name` | `string` | The name of the tileset. |
| `visibility` | `string` | The access control for the tileset, either `public` or `private`. |
| `status` | `string` | The processing status of the tileset, one of: `available`, `pending`, or `invalid`. For tilesets created with the Mapbox Tiling Service, this is always set to `available`. To see the stage of a MTS tileset's most recent job, use the [tileset jobs listing endpoint](#list-information-about-all-jobs-for-a-tileset) with a `limit=1` query parameter. |

### Example response: List tilesets

```json
[
  {
    "type": "vector",
    "center": [-0.2680000000000007, 11.7014165, 2],
    "created": "2015-09-09T23:30:17.936Z",
    "description": "",
    "filesize": 17879790,
    "id": "mapbox.05bv6e12",
    "modified": "2015-09-09T23:30:17.906Z",
    "name": "routes.geojson",
    "visibility": "public",
    "status": "available"
  },
  {
    "type": "raster",
    "center": [-110.32479628173822, 44.56501277250615, 8],
    "created": "2016-12-10T01:29:37.682Z",
    "description": "",
    "filesize": 794079,
    "id": "mapbox.4umcnx2j",
    "modified": "2016-12-10T01:29:37.289Z",
    "name": "sample-4czm7e",
    "visibility": "private",
    "status": "available"
  }
]
```

### Supported libraries: List tilesets

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDK supports this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#tilesets)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Delete tileset

**DELETE** : `https://api.mapbox.com/tilesets/v1/{tileset_id}` [Token scope: **tilesets:write**]

Delete a tileset. Note that you can only delete your own tileset.

| Required parameter | Type | Description |
| --- | --- | --- |
| `tileset_id` | `string` | The ID of the tileset you want to delete, which is composed of your username followed by a period and the tileset's unique name (`username.id`). |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:write` scope. |

### Example request: Delete tileset

```bash
$ curl -X DELETE "https://api.mapbox.com/tilesets/v1/{tileset_id}?access_token=TOKEN_PLACEHOLDER::tilesets:write"
```

### Response: Delete tileset

```
HTTP 200
```

## Retrieve TileJSON metadata

**GET** : `https://api.mapbox.com/v4/{tileset_id}.json`

Given a valid Mapbox [tileset](https://docs.mapbox.com/help/glossary/tileset/) ID, returns [TileJSON](https://docs.mapbox.com/help/glossary/tilejson/) metadata for that tileset.

| Required parameters | Type | Description |
| --- | --- | --- |
| `tileset_id` | `string` | Unique identifier for the vector tileset in the format `username.id`. To composite multiple vector tilesets, use a comma-separated list of up to 15 tileset IDs. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes). |

This endpoint can be further customized with the optional `secure` parameter:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `secure` | `string` | By default, resource URLs in the retrieved TileJSON (such as in the `"tiles"` array) will use the HTTP scheme. Include `secure` in your request to receive HTTPS resource URLs instead. |

### Example request: Retrieve TileJSON metadata

```bash
$ curl "https://api.mapbox.com/v4/examples.civir98a801cq2oo6w6mk1aor-9msik.json?access_token=YOUR_MAPBOX_ACCESS_TOKEN"

# Request HTTPS resource URLs in the retrieved TileJSON
$ curl "https://api.mapbox.com/v4/examples.civir98a801cq2oo6w6mk1aor-9msik.json?secure&access_token=YOUR_MAPBOX_ACCESS_TOKEN"
```

### Response: Retrieve TileJSON metadata

Returns [TileJSON](https://github.com/mapbox/tilejson-spec/) metadata for a tileset. The TileJSON object describes a map's resources, like tiles, markers, and UTFGrid, as well as its name, description, and centerpoint.

### Example response: Retrieve TileJSON metadata

```json
{
  "bounds": [-87.769775, 41.715515, -87.530221, 41.940403],
  "center": [-87.649998, 41.82795900000001, 0],
  "created": 1479169405055,
  "filesize": 3321,
  "format": "pbf",
  "id": "examples.civir98a801cq2oo6w6mk1aor-9msik",
  "mapbox_logo": true,
  "maxzoom": 10,
  "minzoom": 0,
  "modified": 1542143499397,
  "name": "chicago-parks",
  "private": false,
  "scheme": "xyz",
  "tilejson": "2.2.0",
  "tiles": [
    "http://a.tiles.mapbox.com/v4/examples.civir98a801cq2oo6w6mk1aor-9msik/{z}/{x}/{y}.vector.pbf",
    "http://b.tiles.mapbox.com/v4/examples.civir98a801cq2oo6w6mk1aor-9msik/{z}/{x}/{y}.vector.pbf"
  ],
  "vector_layers": [
    {
      "description": "",
      "fields": {
        "description": "String",
        "id": "String",
        "marker-color": "String",
        "marker-size": "String",
        "marker-symbol": "String",
        "title": "String"
      },
      "id": "chicago-parks",
      "maxzoom": 22,
      "minzoom": 0,
      "source": "examples.civir98a801cq2oo6w6mk1aor-9msik",
      "source_name": "chicago-parks"
    }
  ],
  "version": "1.0.0",
  "webpage": "http://a.tiles.mapbox.com/v4/examples.civir98a801cq2oo6w6mk1aor-9msik/page.html"
}
```

## Retrieve tileset activity

**GET** : `https://api.mapbox.com/activity/v1/{username}/tilesets` [Token scope: **tilesets:list**]

Lists 30-day activity data for all tilesets belonging to a specific account. This endpoint supports pagination, returning 100 activity objects by default and a maximum of 500.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account for which the resource activities will be retrieved. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `tilesets:list` scope. |

You can further refine the results from this endpoint with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `sortby` | `string` | Sort the listings by their `requests` or `modified`. The default is `requests`. |
| `orderby` | `string` | Sort direction, `asc` or `desc`. The default is `desc`. |
| `limit` | `integer` | The maximum number of activity data to return, from `1` to `500`. The default is `100`. |
| `start` | `string` | The activity data after which to start the listing. Find the key in the `Link` header of a response. See the [pagination](https://docs.mapbox.com/api/guides/#pagination) section for details. |

### Example request: Retrieve tileset activity

```bash
$ curl "https://api.mapbox.com/activity/v1/YOUR_MAPBOX_USERNAME/tilesets?access_token=TOKEN_PLACEHOLDER"

# Limit the results to the 25 most requested tilesets in the past 30 days
$ curl "https://api.mapbox.com/activity/v1/YOUR_MAPBOX_USERNAME/tilesets?access_token=TOKEN_PLACEHOLDER&limit=25&sortby=requests&orderby=desc"
```

### Response: Retrieve tileset activity

Returns an array of activity objects. Each activity object contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `id` | `string` | The unique tileset Id. |
| `request_count` | `number` | The total number of requests in last 30 days |
| `last_modified` | `string` | A timestamp indicating when the tileset's request count was last modified. |

### Example response: Retrieve tileset activity

```json
[
  {
    "id": "TestAccount.00y37ujk",
    "request_count": 5887624756,
    "last_modified": "2023-06-19T00:31:10.226Z"
  },
  {
    "id": "TestAccount.ck29mgmu80bni2in6s83k6e1q-1y1m7",
    "request_count": 5887579047,
    "last_modified": "2023-06-19T00:31:10.226Z"
  }
]
```

## Mapbox Tiling Service errors

<table><thead><tr><th>Response body <code>message</code></th><th>HTTP status code</th><th>Description</th></tr></thead><tbody><tr><td><code>Not Authorized - No Token</code></td><td><code>401</code></td><td>No token was used in the query.</td></tr><tr><td><code>Not Authorized - Invalid Token</code></td><td><code>401</code></td><td>Check the access token you used in the query.</td></tr><tr><td><code>This endpoint requires a token with {scope} scope</code></td><td><code>403</code></td><td>The access token used in the query needs the specified <a href="https://docs.mapbox.com/accounts/guides/tokens/#scopes">scope</a>.</td></tr><tr><td><code>Forbidden</code></td><td><code>403</code></td><td>There may be an issue with your account. Check your <a href="https://console.mapbox.com/">Account page</a> for more details.<br><br>In some cases, using an access tokens with URL restrictions can also result in a <code>403</code> error. For more information, see our <a href="https://docs.mapbox.com/accounts/guides/tokens/#url-restrictions">Token management guide</a>.</td></tr><tr><td><code>Not Found</code></td><td><code>404</code></td><td>The resource or the account does not exist.</td></tr><tr><td><code>Cannot find tileset</code></td><td><code>404</code></td><td>Check the tileset ID you used in the query.</td></tr><tr><td><code>The requested url's querystring \"limit\" property contains in invalid value.</code></td><td><code>422</code></td><td>The limit specified in the query is larger than 500, or contains non-numeric characters.</td></tr><tr><td><code>Resource is locked and cannot be modified</code></td><td><code>409</code></td><td>A tileset source is "locked" when in use by an active tileset publish job. While locked the resource cannot be deleted or modified. Once a publish job is complete the resource is unlocked.</td></tr><tr><td><code>Invalid start key</code></td><td><code>422</code></td><td>Check the start key used in the query.</td></tr><tr><td><code>Classic styles are no longer supported; see [Classic Styles](https://docs.mapbox.com/map-styles/guides/classic-styles/) for more information</code></td><td><code>410</code></td><td>This is a deprecation notice from API requests for Classic styles, which are no longer supported.</td></tr></tbody></table>

For help troubleshooting common errors, see our [MTS errors documentation](https://docs.mapbox.com/mapbox-tiling-service/overview/errors/#mapbox-tiling-service-errors).

## Mapbox Tiling Service restrictions and limits

-   You must make requests over HTTPS. HTTP is not supported.

<table><thead><tr><th>Mapbox Tiling Service endpoint</th><th>Requests per minute</th><th>Size limits</th></tr></thead><tbody><tr><td><a href="#create-a-tileset-source">Create a tileset source</a></td><td>100</td><td>Tileset sources can be composed of up to 10 source files.<br><br>Each uploaded file must have a maximum size of 20 GB. The maximum combined total size of all files that compose a tileset source is 50 GB. If the total size of all the files that compose a tileset source is greater than 50 GB, MTS will return a response that contains an <code>error</code> property with more details.</td></tr><tr><td><a href="#retrieve-tileset-source-information">Retrieve tileset source information</a></td><td>3,000</td><td>—</td></tr><tr><td><a href="#list-tileset-sources">List tileset sources</a></td><td>3,000</td><td>—</td></tr><tr><td><a href="#delete-a-tileset-source">Delete a tileset source</a></td><td>100</td><td>—</td></tr><tr><td><a href="#create-a-tileset">Create a tileset</a></td><td>100</td><td>—</td></tr><tr><td><a href="#publish-a-tileset">Publish a tileset</a></td><td>2</td><td>The size of a single vector tile is limited to 500 KB. If a job drops features due to tile size, this will be noted in the <code>warnings</code> field of the job object.</td></tr><tr><td><a href="#update-tileset-information">Update tileset information</a></td><td>100</td><td>—</td></tr><tr><td><a href="#retrieve-information-about-a-single-tileset-job">Retrieve information about a single tileset job</a></td><td>3,000</td><td>—</td></tr><tr><td><a href="#list-information-about-all-jobs-for-a-tileset">List information about all jobs for a tileset</a></td><td>3,000</td><td>—</td></tr><tr><td><a href="#view-the-global-queue">View the global queue</a></td><td>3,000</td><td>—</td></tr><tr><td><a href="#validate-a-recipe">Validate a recipe</a></td><td>100</td><td>—</td></tr><tr><td><a href="#retrieve-a-tilesets-recipe">Retrieve a tileset's recipe</a></td><td>3,000</td><td>—</td></tr><tr><td><a href="#update-a-tilesets-recipe">Update a tileset's recipe</a></td><td>100</td><td>—</td></tr><tr><td><a href="#list-tilesets">List tilesets</a></td><td>3,000</td><td>—</td></tr><tr><td><a href="#delete-tileset">Delete a tileset</a></td><td>100</td><td>—</td></tr><tr><td><a href="#retrieve-tilejson-metadata">Retrieve TileJSON metadata</a></td><td>100,000</td><td>—</td></tr><tr><td><a href="#retrieve-tileset-activity">Retrieve tileset activity</a></td><td>500</td><td>—</td></tr></tbody></table>

If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).

### Caching

-   TileJSON responses set default Cache-Control headers to `max-age=43200,s-maxage=300`, or a device cache TTL of 12 hours and a CDN cache TTL of 5 minutes.
-   For general information on caching, see the [Maps APIs caching dive deeper guide](https://docs.mapbox.com/help/dive-deeper/api-caching/).

## Mapbox Tiling Service pricing

-   Billed by **tileset processing and tileset hosting**
-   See rates and discounts per tileset processing and tileset hosting in the pricing page's **[Tilesets](https://www.mapbox.com/pricing/#tilesets)** section

When you use the [Mapbox Tiling Service](https://docs.mapbox.com/mapbox-tiling-service/guides/), usage statistics can be reviewed on your [statistics dashboard](https://console.mapbox.com/account/statistics/). If an account's tilesets usage exceeds the monthly [free tier](https://www.mapbox.com/pricing/#tilesets), you will see line items on the account's invoice for [**tileset processing**](https://docs.mapbox.com/mapbox-tiling-service/guides/pricing/#tilesets-processing) and [**tileset hosting**](https://docs.mapbox.com/mapbox-tiling-service/guides/pricing/#tilesets-hosting).

To see billing metrics for a specific tileset, go to your [tilesets page](https://console.mapbox.com/studio/tilesets/) and click on the tileset's name to visit the Tileset Explorer.

To learn more about how pricing works for this service, see the [MTS pricing guide](https://docs.mapbox.com/mapbox-tiling-service/guides/pricing/).