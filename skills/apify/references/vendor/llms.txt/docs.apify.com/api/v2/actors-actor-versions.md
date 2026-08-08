---
title: Actor versions
url: https://docs.apify.com/api/v2/actors-actor-versions.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify API documentation](https://docs.apify.com/api.md)
  - [Apify API](https://docs.apify.com/api/v2.md)
children:
  - [Get list of versions](https://docs.apify.com/api/v2/actor-versions-get.md)
  - [Create version](https://docs.apify.com/api/v2/actor-versions-post.md)
  - [Get version](https://docs.apify.com/api/v2/actor-version-get.md)
  - [Update version](https://docs.apify.com/api/v2/actor-version-put.md)
  - [Update version (POST)](https://docs.apify.com/api/v2/actor-version-post.md)
  - [Delete version](https://docs.apify.com/api/v2/actor-version-delete.md)
  - [Get list of environment variables](https://docs.apify.com/api/v2/actor-version-env-vars-get.md)
  - [Create environment variable](https://docs.apify.com/api/v2/actor-version-env-vars-post.md)
  - [Get environment variable](https://docs.apify.com/api/v2/actor-version-env-var-get.md)
  - [Update environment variable](https://docs.apify.com/api/v2/actor-version-env-var-put.md)
  - [Update environment variable (POST)](https://docs.apify.com/api/v2/actor-version-env-var-post.md)
  - [Delete environment variable](https://docs.apify.com/api/v2/actor-version-env-var-delete.md)
previous: [Validate Actor input](https://docs.apify.com/api/v2/actor-validate-input-post.md)
next: [Get list of versions](https://docs.apify.com/api/v2/actor-versions-get.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Actor versions

The API endpoints in this section allow you to manage your Apify Actors versions.

* The version object contains the source code of a specific version of an Actor.
* The `sourceType` property indicates where the source code is hosted, and based on its value the Version object has the following additional property:

| **Value**        | **Description**                                                                                                                                                                                                                                                                                                                                     |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `"SOURCE_FILES"` | Source code is comprised of multiple files specified in the `sourceFiles` array. Each item of the array is an object with the following fields: - `name`: File path and name - `format`: Format of the content, can be either `"TEXT"` or `"BASE64"` - `content`: File content Source files can be shown and edited in the Apify Console's Web IDE. |
| `"GIT_REPO"`     | Source code is cloned from a Git repository, whose URL is specified in the `gitRepoUrl` field.                                                                                                                                                                                                                                                      |
| `"TARBALL"`      | Source code is downloaded using a tarball or Zip file from a URL specified in the `tarballUrl` field.                                                                                                                                                                                                                                               |
| `"GITHUB_GIST"`  | Source code is taken from a GitHub Gist, whose URL is specified in the `gitHubGistUrl` field.                                                                                                                                                                                                                                                       |

For more information about source code and Actor versions, check out [Source code](https://docs.apify.com/platform/actors/development/actor-definition/source-code) in Actors documentation.

<!-- -->

## [Get list of versions](https://docs.apify.com/api/v2/actor-versions-get.md)

[/actors/{actorId}/versions](https://docs.apify.com/api/v2/actor-versions-get.md)

## [Create version](https://docs.apify.com/api/v2/actor-versions-post.md)

[/actors/{actorId}/versions](https://docs.apify.com/api/v2/actor-versions-post.md)

## [Get version](https://docs.apify.com/api/v2/actor-version-get.md)

[/actors/{actorId}/versions/{versionNumber}](https://docs.apify.com/api/v2/actor-version-get.md)

## [Update version](https://docs.apify.com/api/v2/actor-version-put.md)

[/actors/{actorId}/versions/{versionNumber}](https://docs.apify.com/api/v2/actor-version-put.md)

## [Update version (POST)](https://docs.apify.com/api/v2/actor-version-post.md)

[/actors/{actorId}/versions/{versionNumber}](https://docs.apify.com/api/v2/actor-version-post.md)

## [Delete version](https://docs.apify.com/api/v2/actor-version-delete.md)

[/actors/{actorId}/versions/{versionNumber}](https://docs.apify.com/api/v2/actor-version-delete.md)

## [Get list of environment variables](https://docs.apify.com/api/v2/actor-version-env-vars-get.md)

[/actors/{actorId}/versions/{versionNumber}/env-vars](https://docs.apify.com/api/v2/actor-version-env-vars-get.md)

## [Create environment variable](https://docs.apify.com/api/v2/actor-version-env-vars-post.md)

[/actors/{actorId}/versions/{versionNumber}/env-vars](https://docs.apify.com/api/v2/actor-version-env-vars-post.md)

## [Get environment variable](https://docs.apify.com/api/v2/actor-version-env-var-get.md)

[/actors/{actorId}/versions/{versionNumber}/env-vars/{envVarName}](https://docs.apify.com/api/v2/actor-version-env-var-get.md)

## [Update environment variable](https://docs.apify.com/api/v2/actor-version-env-var-put.md)

[/actors/{actorId}/versions/{versionNumber}/env-vars/{envVarName}](https://docs.apify.com/api/v2/actor-version-env-var-put.md)

## [Update environment variable (POST)](https://docs.apify.com/api/v2/actor-version-env-var-post.md)

[/actors/{actorId}/versions/{versionNumber}/env-vars/{envVarName}](https://docs.apify.com/api/v2/actor-version-env-var-post.md)

## [Delete environment variable](https://docs.apify.com/api/v2/actor-version-env-var-delete.md)

[/actors/{actorId}/versions/{versionNumber}/env-vars/{envVarName}](https://docs.apify.com/api/v2/actor-version-env-var-delete.md)
