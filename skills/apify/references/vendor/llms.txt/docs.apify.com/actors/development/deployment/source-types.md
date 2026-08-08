---
title: Source types
url: https://docs.apify.com/actors/development/deployment/source-types.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Development](https://docs.apify.com/actors/development.md)
  - [Deployment](https://docs.apify.com/actors/development/deployment.md)
previous: [Deployment](https://docs.apify.com/actors/development/deployment.md)
next: [Continuous integration](https://docs.apify.com/actors/development/deployment/continuous-integration.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Source types

This section explains the various sources types available for Apify Actors and how to deploy an Actor from GitHub using CLI or Gist. Apify Actors supports four source types:

* Web IDE

* Git repository

  * Private repositories
    * How to configure deployment keys
  * Actor monorepos

* Zip file

* GitHub Gist

## Web IDE

This is the default option when your Actor's source code is hosted on the Apify platform. It offers quick previews and updates to your source code, easy file and directory browsing, and direct testing of the [INPUT_SCHEMA.json](https://docs.apify.com/actors/development/actor-definition/input-schema.md) on the Apify platform.

A `Dockerfile` is mandatory for all Actors. When using the default NodeJS Dockerfile, you'll typically need `main.js` for your source code and `package.json` for [npm](https://www.npmjs.com/) package configurations.

For more information on creating custom Dockerfiles or using Apify's base images, refer to the [Dockerfile](https://docs.apify.com/actors/development/actor-definition/dockerfile.md#custom-dockerfile) and [base Docker images](https://docs.apify.com/actors/development/actor-definition/dockerfile.md#base-docker-images) documentation.

## Git repository

[YouTube video player](https://www.youtube-nocookie.com/embed/NEzT_p_RE1Q)

Hosting your Actor's source code in a Git repository allows for multiple files and directories, a custom `Dockerfile` for build process control, and a user description fetched from `README.md`. Specify the repository location using the **Git URL** setting with `https`, `git`, or `ssh` protocols.

To deploy an Actor from GitHub, set the **Source Type** to **Git repository** and enter the GitHub repository URL in the **Git URL** field. You can optionally specify a branch or tag by adding a URL fragment (e.g., `#develop`).

To use a specific directory, add it after the branch/tag, separated by a colon (e.g., `#develop:some/dir`)

GitHub integration

You can easily set up an integration where the Actor is automatically rebuilt on every commit to the Git repository. For more details, see [GitHub integration](https://docs.apify.com/integrations/github.md).

### Private repositories

When using a private Git repository for your Actor's source code, you need to configure a deployment key. This key grants Apify secure, read-only access to your repository.

What are deployment keys

Deployment keys are unique SSH keys that allow Apify to clone and build your Actor from a private repository.

#### How to configure deployment keys

To configure the deployment key for your Actor's source code you need to:

1. In your Actor's settings, find the **Git URL** input field
2. Click the **deployment key** link below the input field
3. Follow the instruction to add the key to your Git hosting service.

![Deployment keys instruction window](/assets/images/deployment-keys-5929eb8db9a4de3afa0a87033b621d98.png)

By using deployment keys, you enable secure, automated builds of your Actor from private repositories.

Key usage limitations

Remember that each key can only be used once per Git hosting service (GitHub, Bitbucket, GitLab)

### Actor monorepos

To manage multiple Actors in a single repository, use the `dockerContextDir` property in the [Actor definition](https://docs.apify.com/actors/development/actor-definition/actor-json.md) to set the Docker context directory (if not provided then the repository root is used). In the Dockerfile, copy both the Actor's source and any shared code into the Docker image.

To enable sharing Dockerfiles between multiple Actors, the Actor build process passes the `ACTOR_PATH_IN_DOCKER_CONTEXT` build argument to the Docker build. It contains the relative path from `dockerContextDir` to the directory selected as the root of the Actor in Apify Console (the "directory" part of the Actor's git URL).

For an example, see the [apify/actor-monorepo-example](https://github.com/apify/actor-monorepo-example) repository. To build Actors from this monorepo, you would set the source URL (including branch name and folder) as `https://github.com/apify/actor-monorepo-example#main:actors/javascript-actor` and `https://github.com/apify/actor-monorepo-example#main:actors/typescript-actor` respectively.

## Zip file

Actors can also use source code from a Zip archive hosted on an external URL. This option supports multiple files and directories, allows for custom `Dockerfile`, and uses `README.md` for the Actor description. If not using a [custom Dockerfile](https://docs.apify.com/actors/development/actor-definition/dockerfile.md#custom-dockerfile), ensure your main file is named `main.js`.

Automatic use of ZIP file

This source type is used automatically when you are using Apify-CLI and the source size exceeds `3MB`.

## GitHub Gist

For smaller projects, GitHub Gist offers a simpler alternative to full Git repositories or hosted Zip files. To use a GitHub Gist, create your Gist at [https://gist.github.com/](https://gist.github.com/), set the **Source type** to **GitHub Gist**, and paste the Gist URL in the provided field.

Like other source types, Gists can include multiple files, directories, and a custom Dockerfile. The Actor description is taken from `README.md`.

By understanding these source types, you can choose the most appropriate option for hosting and deploying your Apify Actors. Each type offers unique advantages, allowing you to select the best fit for your project's size, complexity, and collaboration needs.
