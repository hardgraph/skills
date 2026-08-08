# Running in Docker

Copy for LLM

Running headless browsers in Docker requires a lot of setup to do it right. But there's no need to worry about that, because we already created base images that you can freely use. We use them every day on the [Apify Platform](https://docs.apify.com/sdk/js/sdk/js/docs/concepts/actor-lifecycle.md).

All images can be found in their [GitHub repo](https://github.com/apify/apify-actor-docker) and in our [DockerHub](https://hub.docker.com/u/apify).

## Overview[](#overview)

Browsers are pretty big, so we try to provide a wide variety of images to suit the specific needs. Here's a full list of our Docker images.

* [`apify/actor-node`](#actor-node)
* [`apify/actor-node-puppeteer-chrome`](#actor-node-puppeteer-chrome)
* [`apify/actor-node-playwright`](#actor-node-playwright)
* [`apify/actor-node-playwright-chrome`](#actor-node-playwright-chrome)
* [`apify/actor-node-playwright-firefox`](#actor-node-playwright-firefox)
* [`apify/actor-node-playwright-webkit`](#actor-node-playwright-webkit)

## Versioning[](#versioning)

Each image is tagged with up to 2 version tags, depending on the type of the image. One for Node.js version and second for pre-installed web automation library version. If you use the image name without a version tag, you'll always get the latest available version.

> We recommend always using at least the Node.js version tag in production Dockerfiles. It will ensure that a future update of Node.js will not break our automations.

### Node.js versioning[](#nodejs-versioning)

Our images are built with multiple Node.js versions to ensure backwards compatibility. Currently, we support Node.js **versions 20, 22, and 24** (legacy versions still exist, see DockerHub). To select the preferred version, use the appropriate number as the image tag.

```
# Use Node.js 24

FROM apify/actor-node:24
```

### Automation library versioning[](#automation-library-versioning)

Images that include a pre-installed automation library, which means all images that include `puppeteer` or `playwright` in their name, are also tagged with the pre-installed version of the library. For example, `apify/actor-node-puppeteer-chrome:24-24.14.0` comes with Node.js 24 and Puppeteer v24.14.0. If you try to install a different version of Puppeteer into this image, you may run into compatibility issues, because the Chromium version bundled with `puppeteer` will not match the version of Chromium that's pre-installed.

Similarly `apify/actor-node-playwright-firefox:24-1.54.1` runs on Node.js 24 and is pre-installed with the Firefox version that comes with v1.54.1.

Installing `apify/actor-node-puppeteer-chrome` (without a tag) will install the latest available version of Node.js and `puppeteer`.

### Finding available tags[](#finding-available-tags)

To see all available tags for an image, you can browse Docker Hub directly:

* [actor-node-playwright-chrome tags](https://hub.docker.com/r/apify/actor-node-playwright-chrome/tags)
* [actor-node-playwright tags](https://hub.docker.com/r/apify/actor-node-playwright/tags)
* [actor-node-playwright-firefox tags](https://hub.docker.com/r/apify/actor-node-playwright-firefox/tags)
* [actor-node-playwright-webkit tags](https://hub.docker.com/r/apify/actor-node-playwright-webkit/tags)
* [actor-node-puppeteer-chrome tags](https://hub.docker.com/r/apify/actor-node-puppeteer-chrome/tags)
* [actor-node tags](https://hub.docker.com/r/apify/actor-node/tags)

You can also query available tags programmatically using the Docker Hub API:

```
curl -s "https://registry.hub.docker.com/v2/repositories/apify/actor-node-playwright-chrome/tags?page_size=50" | jq '.results[].name'
```

### Pre-release tags[](#pre-release-tags)

We also build pre-release versions of the images to test the changes we make. Those are typically denoted by a `beta` suffix, but it can vary depending on our needs. If you need to try a pre-release version, you can do it like this:

```
# Without library version.

FROM apify/actor-node:24-beta
```

```
# With library version.

FROM apify/actor-node-puppeteer-chrome:24-24.14.0-beta
```

## Best practices[](#best-practices)

For production Actors, we recommend pinning both the Node.js version and the automation library version in your Dockerfile, and matching the library version in your `package.json`. This ensures reproducible builds across environments.

### Recommended approach (pinned versions)[](#recommended-approach-pinned-versions)

Pin the automation library version in both your Dockerfile tag and `package.json`:

```
FROM apify/actor-node-playwright-chrome:24-1.52.0
```

```
{

    "dependencies": {

        "crawlee": "^3.0.0",

        "playwright": "1.52.0"

    }

}
```

Version matching is important

The version in `package.json` **must match** the version in the Docker image tag. If they don't match, npm will attempt to install a different version, which can cause compatibility issues with the pre-installed browser binaries.

### Alternative approach (using `*`)[](#alternative-approach-using-)

You can use `*` as the automation library version in `package.json` to automatically use whatever version is pre-installed in the Docker image:

```
FROM apify/actor-node-playwright-chrome:24
```

```
{

    "dependencies": {

        "crawlee": "^3.0.0",

        "playwright": "*"

    }

}
```

This approach prevents the library from being re-installed on build, which is useful because the pre-installed libraries are only guaranteed to work with the specific browser versions bundled in the image. However, it provides less control over exactly which version you're running, since the Docker image tag without a library version may be updated over time.

### Warning about image size[](#warning-about-image-size)

Browsers are huge. If you don't need them all in your image, it's better to use a smaller image with only the one browser you need.

You should also be careful when installing new dependencies. Nothing prevents you from installing Playwright into the`actor-node-puppeteer-chrome` image, but the resulting image will be about 3 times larger and extremely slow to download and build.

When you use only what you need, you'll be rewarded with reasonable build and start times.

## Apify Docker Images[](#apify-docker-images)

### actor-node[](#actor-node)

This is the smallest image we have based on Alpine Linux. It does not include any browsers, and it's therefore best used with [`CheerioCrawler`](https://crawlee.dev/api/cheerio-crawler/class/CheerioCrawler). It benefits from lightning fast builds and container startups.

[`PuppeteerCrawler`](https://crawlee.dev/api/puppeteer-crawler/class/PuppeteerCrawler)

,

[`PlaywrightCrawler`](https://crawlee.dev/api/playwright-crawler/class/PlaywrightCrawler)

and other browser based features will **NOT** work with this image.

```
FROM apify/actor-node:24
```

### actor-node-puppeteer-chrome[](#actor-node-puppeteer-chrome)

This image includes Puppeteer (Chromium) and the Chrome browser. It can be used with [`CheerioCrawler`](https://crawlee.dev/api/cheerio-crawler/class/CheerioCrawler) and [`PuppeteerCrawler`](https://crawlee.dev/api/puppeteer-crawler/class/PuppeteerCrawler), but **NOT** with [`PlaywrightCrawler`](https://crawlee.dev/api/playwright-crawler/class/PlaywrightCrawler).

The image supports XVFB by default, so you can run both `headless` and `headful` browsers with it.

```
FROM apify/actor-node-puppeteer-chrome:24
```

### actor-node-playwright[](#actor-node-playwright)

A very large and slow image that can run all Playwright browsers: Chromium, Chrome, Firefox, WebKit. Everything is installed. If you need to develop or test with multiple browsers, this is the image to choose, but in most cases, it's better to use the specialized images below.

```
FROM apify/actor-node-playwright:24
```

### actor-node-playwright-chrome[](#actor-node-playwright-chrome)

Similar to [`actor-node-puppeteer-chrome`](#actor-node-puppeteer-chrome), but for Playwright. You can run [`CheerioCrawler`](https://crawlee.dev/api/cheerio-crawler/class/CheerioCrawler) and [`PlaywrightCrawler`](https://crawlee.dev/api/playwright-crawler/class/PlaywrightCrawler), but **NOT** [`PuppeteerCrawler`](https://crawlee.dev/api/puppeteer-crawler/class/PuppeteerCrawler).

It uses the [`PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD`](https://playwright.dev/docs/library#browser-downloads) environment variable to block installation of more browsers into the image to keep it small. If you want more browsers, either use the [`actor-node-playwright`](#actor-node-playwright) image override this env var.

The image supports XVFB by default, so we can run both `headless` and `headful` browsers with it.

```
FROM apify/actor-node-playwright-chrome:24
```

### actor-node-playwright-firefox[](#actor-node-playwright-firefox)

Same idea as [`actor-node-playwright-chrome`](#actor-node-playwright-chrome), but with Firefox pre-installed.

```
FROM apify/actor-node-playwright-firefox:24
```

### actor-node-playwright-webkit[](#actor-node-playwright-webkit)

Same idea as [`actor-node-playwright-chrome`](#actor-node-playwright-chrome), but with WebKit pre-installed.

```
FROM apify/actor-node-playwright-webkit:24
```

## Example Dockerfile[](#example-dockerfile)

To use the above images, it's necessary to have a [`Dockerfile`](https://docs.docker.com/engine/reference/builder/). You can either use this example, or bootstrap your projects with the [Crawlee CLI](https://crawlee.dev/docs/introduction/setting-up) which automatically adds the correct Dockerfile into our project folder.

* Node+JavaScript
* Node+TypeScript
* Browser+JavaScript
* Browser+TypeScript

```
# Specify the base Docker image. You can read more about

# the available images at https://crawlee.dev/docs/guides/docker-images

# You can also use any other image from Docker Hub.

FROM apify/actor-node:16



# Copy just package.json and package-lock.json

# to speed up the build using Docker layer cache.

COPY package*.json ./



# Install NPM packages, skip optional and development dependencies to

# keep the image small. Avoid logging too much and print the dependency

# tree for debugging

RUN npm --quiet set progress=false \

    && npm install --omit=dev --omit=optional \

    && echo "Installed NPM packages:" \

    && (npm list --omit=dev --all || true) \

    && echo "Node.js version:" \

    && node --version \

    && echo "NPM version:" \

    && npm --version



# Next, copy the remaining files and directories with the source code.

# Since we do this after NPM install, quick build will be really fast

# for most source file changes.

COPY . ./





# Run the image.

CMD npm start --silent
```

```
# Specify the base Docker image. You can read more about

# the available images at https://crawlee.dev/docs/guides/docker-images

# You can also use any other image from Docker Hub.

FROM apify/actor-node:16 AS builder



# Copy just package.json and package-lock.json

# to speed up the build using Docker layer cache.

COPY package*.json ./



# Install all dependencies. Don't audit to speed up the installation.

RUN npm install --include=dev --audit=false



# Next, copy the source files using the user set

# in the base image.

COPY . ./



# Install all dependencies and build the project.

# Don't audit to speed up the installation.

RUN npm run build



# Create final image

FROM apify/actor-node:16



# Copy only built JS files from builder image

COPY --from=builder /usr/src/app/dist ./dist



# Copy just package.json and package-lock.json

# to speed up the build using Docker layer cache.

COPY package*.json ./



# Install NPM packages, skip optional and development dependencies to

# keep the image small. Avoid logging too much and print the dependency

# tree for debugging

RUN npm --quiet set progress=false \

    && npm install --omit=dev --omit=optional \

    && echo "Installed NPM packages:" \

    && (npm list --omit=dev --all || true) \

    && echo "Node.js version:" \

    && node --version \

    && echo "NPM version:" \

    && npm --version



# Next, copy the remaining files and directories with the source code.

# Since we do this after NPM install, quick build will be really fast

# for most source file changes.

COPY . ./





# Run the image.

CMD npm run start:prod --silent
```

This example is for Playwright. If you want to use Puppeteer, simply replace **playwright** with **puppeteer** in the `FROM` declaration.

```
# Specify the base Docker image. You can read more about

# the available images at https://crawlee.dev/docs/guides/docker-images

# You can also use any other image from Docker Hub.

FROM apify/actor-node-playwright-chrome:16



# Copy just package.json and package-lock.json

# to speed up the build using Docker layer cache.

COPY --chown=myuser package*.json ./



# Install NPM packages, skip optional and development dependencies to

# keep the image small. Avoid logging too much and print the dependency

# tree for debugging

RUN npm --quiet set progress=false \

    && npm install --omit=dev --omit=optional \

    && echo "Installed NPM packages:" \

    && (npm list --omit=dev --all || true) \

    && echo "Node.js version:" \

    && node --version \

    && echo "NPM version:" \

    && npm --version



# Next, copy the remaining files and directories with the source code.

# Since we do this after NPM install, quick build will be really fast

# for most source file changes.

COPY --chown=myuser . ./





# Run the image.

CMD npm start --silent
```

This example is for Playwright. If you want to use Puppeteer, simply replace **playwright** with **puppeteer** in both `FROM` declarations.

```
# Specify the base Docker image. You can read more about

# the available images at https://crawlee.dev/docs/guides/docker-images

# You can also use any other image from Docker Hub.

FROM apify/actor-node-playwright-chrome:16 AS builder



# Copy just package.json and package-lock.json

# to speed up the build using Docker layer cache.

COPY --chown=myuser package*.json ./



# Install all dependencies. Don't audit to speed up the installation.

RUN npm install --include=dev --audit=false



# Next, copy the source files using the user set

# in the base image.

COPY --chown=myuser . ./



# Install all dependencies and build the project.

# Don't audit to speed up the installation.

RUN npm run build



# Create final image

FROM apify/actor-node-playwright-chrome:16



# Copy only built JS files from builder image

COPY --from=builder --chown=myuser /home/myuser/dist ./dist



# Copy just package.json and package-lock.json

# to speed up the build using Docker layer cache.

COPY --chown=myuser package*.json ./



# Install NPM packages, skip optional and development dependencies to

# keep the image small. Avoid logging too much and print the dependency

# tree for debugging

RUN npm --quiet set progress=false \

    && npm install --omit=dev --omit=optional \

    && echo "Installed NPM packages:" \

    && (npm list --omit=dev --all || true) \

    && echo "Node.js version:" \

    && node --version \

    && echo "NPM version:" \

    && npm --version



# Next, copy the remaining files and directories with the source code.

# Since we do this after NPM install, quick build will be really fast

# for most source file changes.

COPY --chown=myuser . ./





# Run the image. If you know you won't need headful browsers,

# you can remove the XVFB start script for a micro perf gain.

CMD ./start_xvfb_and_run_cmd.sh && npm run start:prod --silent
```
