# Uploads API

> **Note (legacy): Use the Mapbox Tiling Service**
> 
> We recommend you use the [Mapbox Tiling Service (MTS)](https://docs.mapbox.com/api/maps/mapbox-tiling-service/) to create tilesets instead of the Uploads API. MTS gives you more control over the tiling process, including [zoom levels](https://docs.mapbox.com/mapbox-tiling-service/reference/#zoom-levels) and configuration rules called [recipes](https://docs.mapbox.com/mapbox-tiling-service/guides/tileset-recipes/).

The **Mapbox Uploads API** transforms geographic data into tilesets that can be used with maps and geographic applications. Given a wide variety of geospatial formats, it normalizes [projections](https://docs.mapbox.com/help/glossary/projection/) and generates tiles at multiple zoom levels to make data viewable on the web.

The upload workflow follows these steps:

1.  Request temporary S3 credentials that allow you to stage the file. *Jump to the [Retrieve S3 credentials section](#retrieve-s3-credentials).*
2.  Use an S3 client to upload the file to Mapbox's S3 staging bucket using these credentials.
3.  Create an upload using the staged file's URL. *Jump to the [Create an upload section](#create-an-upload).*
4.  Retrieve the upload's status as it is being processed into a tileset. *Jump to the [Retrieve upload status section](#retrieve-upload-status).*

## Retrieve S3 credentials

**POST** : `https://api.mapbox.com/uploads/v1/{username}/credentials` [Token scope: **uploads:write**]

Mapbox provides an Amazon S3 bucket to stage your file while your upload is processed. This endpoint allows you to retrieve temporary S3 credentials to use during the staging process.

**Note:** This step is necessary before you can stage a file in the Amazon S3 bucket provided by Mapbox. All uploads must be staged in this Amazon S3 bucket before being uploaded to your Mapbox account. To learn more about how to stage an upload to Amazon S3, read the [Upload to Mapbox using cURL tutorial](https://docs.mapbox.com/help/tutorials/upload-curl/#stage-your-file-on-amazon-s3).

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which you are uploading a tileset. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `uploads:write` scope. |

### Example request: Retrieve S3 credentials

```bash
$ curl -X POST "https://api.mapbox.com/uploads/v1/YOUR_MAPBOX_USERNAME/credentials?access_token=TOKEN_PLACEHOLDER::uploads:write"
```

### Example AWS CLI usage

```
$ export AWS_ACCESS_KEY_ID={accessKeyId}
$ export AWS_SECRET_ACCESS_KEY={secretAccessKey}
$ export AWS_SESSION_TOKEN={sessionToken}
$ aws s3 cp /path/to/file s3://{bucket}/{key} --region us-east-1
```

### Response: Retrieve S3 credentials

The response body is a JSON object that contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `accessKeyId` | `string` | AWS Access Key ID |
| `bucket` | `string` | S3 bucket name |
| `key` | `string` | The unique key for data to be staged |
| `secretAccessKey` | `string` | AWS Secret Access Key |
| `sessionToken` | `string` | A temporary security token |
| `url` | `string` | The destination URL of the file |

Use these credentials to store your data in the provided bucket with the provided key using the [AWS CLI](http://docs.aws.amazon.com/cli/latest/reference/s3/index.html) or AWS SDK of your choice. The bucket is located in AWS region `us-east-1`.

### Example response: Retrieve S3 credentials

```json
{
  "accessKeyId": "{accessKeyId}",
  "bucket": "somebucket",
  "key": "hij456",
  "secretAccessKey": "{secretAccessKey}",
  "sessionToken": "{sessionToken}",
  "url": "{url}"
}
```

### Supported libraries: Retrieve S3 credentials

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox Python CLI](https://github.com/mapbox/mapbox-cli-py#upload)
-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#createuploadcredentials)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Create an upload

**POST** : `https://api.mapbox.com/uploads/v1/{username}` [Token scope: **uploads:write**]

Uploads can be created from a file staged on Mapbox's S3 bucket, or from an existing Mapbox dataset.

After you have used the temporary S3 credentials to transfer your file to Mapbox's staging bucket, you can trigger the generation of a tileset using the file's URL and a destination tileset ID.

Uploaded files *must* be in the bucket provided by Mapbox. Requests for resources from other S3 buckets or URLs will fail.

**Note:** You can only replace existing tilesets created with the Uploads API with this endpoint. You cannot replace a tileset created with [Mapbox Tiling Service (MTS)](https://docs.mapbox.com/api/maps/mapbox-tiling-service/) with a tileset created with the Uploads API.

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account to which you are uploading. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `uploads:write` scope. |

The request body must be a JSON object that contains the following properties:

| Request body properties | Type | Description |
| --- | --- | --- |
| `tileset` | `string` | The tileset ID to create or replace, in the format `username.nameoftileset`. Limited to 32 characters. This character limit does not include the username. The only allowed special characters are `-` and `_`. If you reuse a `tileset` value, this action will replace existing data. Use a random value so that a new tileset is created, or check your existing tilesets first. |
| `url` | `string` | Either the HTTPS URL of the S3 object provided in the credential request, or the [dataset ID](https://docs.mapbox.com/help/glossary/dataset-id/) of an existing Mapbox dataset. |
| `name` | `string` | *Optional.* The name of the tileset. Limited to 64 characters. |

> **Note: Private vs. public tilesets**
> 
> A newly-created tileset is *private* by default. If you want to make the tileset *public*, click on the tileset from your Studio account's tilesets page and select the option [Make public](https://docs.mapbox.com/studio-manual/reference/tilesets/#make-private).

### Example request: Create an upload from a file staged on S3

```bash
$ curl -X POST -H "Content-Type: application/json" -H "Cache-Control: no-cache" -d '{
  "url": "https://{bucket}.s3.amazonaws.com/{key}",
  "tileset": "{username}.{tileset-name}"
}' 'https://api.mapbox.com/uploads/v1/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::uploads:write'
```

### Example request: Create an upload from a Mapbox dataset

```bash
$ curl -X POST -H "Content-Type: application/json" -H "Cache-Control: no-cache" -d '{
  "tileset": "{username}.mytileset",
  "url": "mapbox://datasets/{username}/{dataset}",
  "name": "example-dataset"
}' 'https://api.mapbox.com/uploads/v1/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::uploads:write'
```

### Response: Create an upload

The response body is a JSON object that contains the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `complete` | `boolean` | Whether the upload is complete (`true`) or not complete (`false`). |
| `tileset` | `string` | The ID of the tileset that will be created or replaced if upload is successful. |
| `error` | `string` or `null` | If `null`, the upload is in progress or has successfully completed. Otherwise, provides a brief explanation of the error. |
| `id` | `string` | The unique identifier for the upload. |
| `name` | `string` | The name of the upload. |
| `modified` | `string` | A timestamp indicating when the upload resource was last modified. |
| `created` | `string` | A timestamp indicating when the upload resource was created. |
| `owner` | `string` | The unique identifier for the owner's account. |
| `progress` | `integer` | The progress of the upload, expressed as a float between `0` (started) and `1` (completed). |

### Example response: Create an upload

```json
{
  "complete": false,
  "tileset": "example.markers",
  "error": null,
  "id": "hij456",
  "name": "example-markers",
  "modified": "{timestamp}",
  "created": "{timestamp}",
  "owner": "{username}",
  "progress": 0
}
```

### Supported libraries: Create an upload

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#createupload)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Retrieve upload status

**GET** : `https://api.mapbox.com/uploads/v1/{username}/{upload_id}` [Token scope: **uploads:read**]

Once an upload is created, you can track its status. Uploads have a `progress` property that start at `0` and end at `1` when an upload is complete. If there's an error processing an upload, the `error` property will include an [error message](https://docs.mapbox.com/help/troubleshooting/uploads/#errors).

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account for which you are requesting an upload status. |
| `upload_id` | `string` | The ID of the upload. Provided in the response body to a successful upload request. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `uploads:read` scope. |

### Example request: Retrieve upload status

```bash
$ curl "https://api.mapbox.com/uploads/v1/YOUR_MAPBOX_USERNAME/{upload_id}?access_token=TOKEN_PLACEHOLDER::uploads:read"
```

### Response: Retrieve upload status

The response body is a JSON object containing the following properties:

| Property | Type | Description |
| --- | --- | --- |
| `complete` | `boolean` | Indicates whether the upload is complete (`true`) or not complete (`false`). |
| `tileset` | `string` | The ID of the tileset that will be created or replaced if upload is successful. |
| `error` | `string` or `null` | If `null`, the upload is in progress or has successfully completed. Otherwise, provides a brief explanation of the error. |
| `id` | `string` | The unique identifier for the upload. |
| `name` | `string` | The name of the upload. |
| `modified` | `string` | The timestamp for when the upload resource was last modified. |
| `created` | `string` | The timestamp for when the upload resource was created. |
| `owner` | `string` | The unique identifier for the owner's account. |
| `progress` | `integer` | The progress of the upload, expressed as a float between `0` (started) and `1` (completed). |

### Example response: Retrieve upload status

```json
{
  "complete": true,
  "tileset": "example.markers",
  "error": null,
  "id": "hij456",
  "name": "example-markers",
  "modified": "{timestamp}",
  "created": "{timestamp}",
  "owner": "{username}",
  "progress": 1
}
```

### Supported libraries: Retrieve upload status

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#getupload)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Retrieve recent upload statuses

**GET** : `https://api.mapbox.com/uploads/v1/{username}` [Token scope: **uploads:list**]

Retrieve multiple upload statuses at the same time, sorted by the most recently created.

This endpoint supports [pagination](https://docs.mapbox.com/api/guides/#pagination) so that you can list many uploads.

| Required parameter | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the account for which you are requesting upload statuses. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `uploads:list` scope. |

The results from this endpoint can be further modified with the following optional parameters:

| Optional parameters | Type | Description |
| --- | --- | --- |
| `reverse` | `boolean` | Set this parameter to `true` to reverse the sorting order of the results. By default, the Uploads API returns the most recently created upload statuses first. |
| `limit` | `integer` | The maximum number of statuses to return, up to `100`. |

### Example request: Retrieve recent upload statuses

```bash
$ curl "https://api.mapbox.com/uploads/v1/YOUR_MAPBOX_USERNAME?access_token=TOKEN_PLACEHOLDER::uploads:list"

# Return at most 10 upload statuses, in chronological order

$ curl "https://api.mapbox.com/uploads/v1/YOUR_MAPBOX_USERNAME?reverse=true&limit=10&access_token=TOKEN_PLACEHOLDER::uploads:list"
```

### Response: Retrieve recent upload statuses

This request returns the same information as an individual upload status request does, but for all an account's recent uploads. The list is limited to one MB of JSON.

### Example response: Retrieve recent upload statuses

```json
[
  {
    "complete": true,
    "tileset": "example.mbtiles",
    "error": null,
    "id": "abc123",
    "name": null,
    "modified": "2014-11-21T19:41:10.000Z",
    "created": "2014-11-21T19:41:10.000Z",
    "owner": "example",
    "progress": 1
  },
  {
    "complete": false,
    "tileset": "example.foo",
    "error": null,
    "id": "xyz789",
    "name": "foo",
    "modified": "2014-11-21T19:41:10.000Z",
    "created": "2014-11-21T19:41:10.000Z",
    "owner": "example",
    "progress": 0
  }
]
```

### Supported libraries: Retrieve recent upload statuses

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#listuploads)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Remove an upload status

**DELETE** : `https://api.mapbox.com/uploads/v1/{username}/{upload_id}` [Token scope: **uploads:write**]

Remove the status of a completed upload from the upload listing.

Uploads are only statuses, so removing an upload from the listing doesn't delete the associated tileset. Tilesets can only be deleted from within Mapbox Studio. An upload status cannot be removed from the upload listing until after it has completed.

| Required parameters | Type | Description |
| --- | --- | --- |
| `username` | `string` | The username of the associated account. |
| `upload_id` | `string` | The ID of the upload. Provided in the response body to a successful upload request. |
| `access_token` | `string` | A valid Mapbox [access token](https://docs.mapbox.com/api/guides/#access-tokens-and-token-scopes) with the `uploads:write` scope. |

### Example request: Remove an upload status

```bash
$ curl -X DELETE "https://api.mapbox.com/uploads/v1/YOUR_MAPBOX_USERNAME/{upload_id}?access_token=TOKEN_PLACEHOLDER::uploads:write"
```

### Response: Remove an upload status

> `HTTP 204 No content`

### Supported libraries: Remove an upload status

Mapbox wrapper libraries help you integrate Mapbox APIs into your existing application. The following SDKs support this endpoint:

-   [Mapbox JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js/blob/main/docs/services.md#deleteupload)

See the SDK documentation for details and examples of how to use the relevant methods to query this endpoint.

## Uploads API restrictions and limits

-   Tileset names are limited to 64-characters.
-   The default rate limit for the Mapbox Uploads API endpoint is 60 requests per minute. If you require a higher rate limit, [contact us](https://www.mapbox.com/contact/sales/).
-   If you exceed the rate limit, you will receive an `HTTP 429 Too Many Requests` response. For information on rate limit headers, see the [Rate limit headers](https://docs.mapbox.com/api/guides/#rate-limit-headers) section.
-   The Uploads API supports different file sizes for various file types:

<table><thead><tr><th>File type</th><th>Size limit</th></tr></thead><tbody><tr><td>CSV</td><td>1 GB</td></tr><tr><td>GeoJSON</td><td>1 GB<br>GeoJSON must use the <a href="https://tools.ietf.org/html/rfc7946#section-4"><code>OGC:CRS84</code> coordinate reference system</a>.</td></tr><tr><td>GPX</td><td>260 MB</td></tr><tr><td>KML</td><td>260 MB</td></tr><tr><td>Mapbox Dataset</td><td>1 GB</td></tr><tr><td>MBTiles</td><td>25 GB</td></tr><tr><td>Shapefile (unzipped)</td><td>260 MB (combined uncompressed size of <code>.shp</code> and <code>.dbf</code> files)<br>Shapefiles must be uploaded as compressed <code>.zip</code> files.</td></tr><tr><td>TIFF and GeoTIFF</td><td>10 GB</td></tr></tbody></table>

## Uploads API errors

To see a list of possible upload errors, visit the [Uploads errors page](https://docs.mapbox.com/help/troubleshooting/uploads/#errors).

## Uploads API pricing

-   Billed by **tileset processing and tileset hosting**
-   See rates and discounts per tileset processing and tileset hosting in the pricing page's **[Tilesets](https://www.mapbox.com/pricing/#tilesets)** section

> **Note**
> 
> Mapbox Studio is powered by the Uploads API, but is subject to usage restrictions and alternative pricing. See the [Mapbox Studio pricing guide](https://docs.mapbox.com/studio-manual/reference/tilesets/#pricing) for more details.

When you use the Uploads API, usage statistics can be reviewed on your [Statistics dashboard](https://console.mapbox.com/account/statistics/). If an account's tilesets usage exceeds the monthly [free tier](https://www.mapbox.com/pricing/#tilesets), you will see line items on the account's invoice for [**tileset processing**](https://docs.mapbox.com/mapbox-tiling-service/guides/pricing/#tilesets-processing) and [**tileset hosting**](https://docs.mapbox.com/mapbox-tiling-service/guides/pricing/#tilesets-hosting).

To see billing metrics for a specific tileset, go to your [tilesets page](https://console.mapbox.com/studio/tilesets/) and click on the tileset's name to visit the Tileset Explorer.

To learn more about how pricing works for this service, see the [tilesets pricing guide](https://docs.mapbox.com/mapbox-tiling-service/guides/pricing/).