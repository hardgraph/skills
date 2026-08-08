# Installation

Copy for LLM

Learn how to install Apify CLI.

## Installation scripts[](#installation-scripts)

Installation scripts use [Bun](https://bun.sh/) to create a standalone executable file, so you don't need Node.js.

This approach eliminates Node.js dependency management, which is useful if you're a Python developer or work in non-Node.js environments.

* MacOS/Linux
* Windows

```
curl -fsSL https://apify.com/install-cli.sh | bash
```

```
irm https://apify.com/install-cli.ps1 | iex
```

## Homebrew[](#homebrew)

Homebrew automatically installs Node.js as a dependency.

If you already have Node.js installed through another method, for example, `nvm`, it might create version conflicts. To fix it, modify your `PATH` environment variable to prioritize your preferred Node.js installation over Homebrew's version.

```
brew install apify-cli
```

## NPM[](#npm)

1. Make sure you have [Node.js](https://nodejs.org) version 22 or higher with NPM installed:

   ```
   node --version

   npm --version
   ```

2. Install or upgrade Apify CLI:

   ```
   npm install -g apify-cli
   ```

<br />

Troubleshooting

If you receive a permission error, read npm's [guide on installing packages globally](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally).

## Verify installation[](#verify-installation)

To verify the installation process, run:

```
apify --version
```

The output includes installation details, for example:

```
apify-cli/1.0.1 (0dfcfd8) running on darwin-arm64 with bun-1.2.19 (emulating node 24.3.0), installed via bundle
```

## Upgrade version[](#upgrade-version)

To upgrade Apify CLI to the latest version, run:

```
apify upgrade
```
