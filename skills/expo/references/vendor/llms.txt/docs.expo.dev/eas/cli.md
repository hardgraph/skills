---
modificationDate: August 08, 2026
title: EAS CLI reference
description: EAS CLI is a command-line tool that allows you to interact with Expo Application Services (EAS) from your terminal.
cliVersion: 21.7.0
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/cli/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/cli/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS
Pages in this section:
- [Introduction](https://docs.expo.dev/eas.md)
- [Configuration with eas.json](https://docs.expo.dev/eas/json.md)
- [EAS CLI](https://docs.expo.dev/eas/cli.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# EAS CLI reference

EAS CLI is a command-line tool that allows you to interact with Expo Application Services (EAS) from your terminal.

CLI version:

21.7.0

CLI version 21.7.0

You can use EAS Command-Line Interface (CLI) to build, update, submit, deploy or use workflows in your Expo and React Native project from a terminal window.

## Installation

You need to install the EAS CLI globally on your machine. You do this by running the following command:

```sh
# npm
npm install --global eas-cli

# yarn
yarn global add eas-cli

# pnpm
pnpm add --global eas-cli

# bun
bun add --global eas-cli
```

Alternatively, you can use CLI tools provided by your package manager to run EAS CLI commands:

```sh
# npm
npx eas-cli@latest

# yarn
yarn dlx eas-cli@latest

# pnpm
pnpm dlx eas-cli@latest

# bun
bunx eas-cli@latest
```

## Commands

Use the EAS CLI by running one of the commands documented on this page, optionally followed by any flags or arguments. Flags customize the behavior of a command, and arguments are specific to the command.

### `eas account:audit [ACCOUNT_NAME]`

View the audit logs for an account.

Usage

```sh
eas account:audit [ACCOUNT_NAME] [--limit ] [--after ] [--json] [--non-interactive]
```

Argument

-   `[ACCOUNT_NAME]` Account name to view audit logs for. If not provided, the account will be selected interactively (or defaults to the only account if there is just one).

Flags

-   `--after=<value>` Cursor for pagination. Use the endCursor from a previous query to fetch the next page.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 50 and is capped at 100.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas account:login`

Log in with your Expo account.

Usage

```sh
eas account:login [-s] [-b]
```

Flags

-   `-b, --[no-]browser` Log in with your browser (default; use `--no-browser` for CLI-based login).
-   `-s, --sso` Log in with SSO.

Alias

```sh
eas login
```

### `eas account:logout`

Log out.

Usage

```sh
eas account:logout
```

Alias

```sh
eas logout
```

### `eas account:usage [ACCOUNT_NAME]`

View account usage and billing for the current cycle.

Usage

```sh
eas account:usage [ACCOUNT_NAME] [--json] [--non-interactive]
```

Argument

-   `[ACCOUNT_NAME]` Account name to view usage for. If not provided, the account will be selected interactively (or defaults to the only account if there is just one).

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas account:view`

Show the username you are logged in as.

Usage

```sh
eas account:view
```

Alias

```sh
eas whoami
```

### `eas analytics [STATUS]`

Display or change analytics settings.

Usage

```sh
eas analytics [STATUS]
```

### `eas autocomplete [SHELL]`

Display autocomplete installation instructions.

Usage

```sh
eas autocomplete [SHELL] [-r]
```

Argument

-   `[SHELL]` (zsh|bash|powershell) Shell type.

Flag

-   `-r, --refresh-cache` Refresh cache (ignores displaying instructions).

Examples

```sh
eas autocomplete
eas autocomplete bash
eas autocomplete zsh
eas autocomplete powershell
eas autocomplete --refresh-cache
```

### `eas branch:create [NAME]`

Create a branch.

Usage

```sh
eas branch:create [NAME] [--json] [--non-interactive]
```

Argument

-   `[NAME]` Name of the branch to create.

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas branch:delete [NAME]`

Delete a branch.

Usage

```sh
eas branch:delete [NAME] [--json] [--non-interactive]
```

Argument

-   `[NAME]` Name of the branch to delete.

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas branch:list`

List all branches.

Usage

```sh
eas branch:list [--offset ] [--limit ] [--json] [--non-interactive]
```

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 50 and is capped at 100.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--offset=<value>` Start queries from specified index. Use for paginating results. Defaults to 0.

### `eas branch:rename`

Rename a branch.

Usage

```sh
eas branch:rename [--from ] [--to ] [--json] [--non-interactive]
```

Flags

-   `--from=<value>` Current name of the branch.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--to=<value>` New name of the branch.

### `eas branch:view [NAME]`

View a branch.

Usage

```sh
eas branch:view [NAME] [--offset ] [--limit ] [--json] [--non-interactive]
```

Argument

-   `[NAME]` Name of the branch to view.

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 25 and is capped at 50.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--offset=<value>` Start queries from specified index. Use for paginating results. Defaults to 0.

### `eas browse [PAGE]`

Transition from the terminal to the web browser to view and interact with your project on [https://expo.dev](https://expo.dev).

Usage

```sh
eas browse [PAGE] [-n] [--json] [--non-interactive]
```

Argument

-   `[PAGE]` (build|builds|submit|submissions|update|updates|workflow|workflows|cicd|hosting|deployments|credentials|env|in sights|observe|settings) Project subpage to open. Defaults to the project dashboard.

Flags

-   `-n, --no-browser` Print the URL instead of opening it in a web browser.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas build`

Start a build.

Usage

```sh
eas build [-p android|ios|all] [-e PROFILE_NAME] [--local] [--output ] [--wait] [--clear-cache]
[-s | --auto-submit-with-profile PROFILE_NAME] [--what-to-test ] [-m ] [--build-logger-level
trace|debug|info|warn|error|fatal] [--freeze-credentials] [--refresh-ad-hoc-provisioning-profile] [--verbose-logs]
[--json] [--non-interactive]
```

Flags

-   `-e, --profile=PROFILE_NAME` Name of the build profile from **eas.json.** Defaults to "production" if defined in **eas.json.**
-   `-m, --message=<value>` A short message describing the build.
-   `-p, --platform=<option>` <options: android|ios|all>.
-   `-s, --auto-submit` Submit on build complete using the submit profile with the same name as the build profile.
-   `--auto-submit-with-profile=PROFILE_NAME` Submit on build complete using the submit profile with provided name.
-   `--build-logger-level=<option>` The level of logs to output during the build process. Defaults to "info". <options: trace|debug|info|warn|error|fatal>.
-   `--clear-cache` Clear cache before the build.
-   `--freeze-credentials` Prevent the build from updating credentials in non-interactive mode.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--local` Run build locally [experimental].
-   `--non-interactive` Run the command in non-interactive mode.
-   `--output=<value>` Output path for local build.
-   `--refresh-ad-hoc-provisioning-profile` Refresh managed ad-hoc provisioning profiles from App Store Connect before gathering build credentials.
-   `--verbose-logs` Use verbose logs for the build process.
-   `--[no-]wait` Wait for build(s) to complete.
-   `--what-to-test=<value>` Specify the "What to Test" information for the build in TestFlight (iOS-only). To be used with the `auto-submit` flag.

### `eas build:cancel [BUILD_ID]`

Cancel a build.

Usage

```sh
eas build:cancel [BUILD_ID] [--non-interactive] [-p android|ios|all] [-e PROFILE_NAME]
```

Flags

-   `-e, --profile=PROFILE_NAME` Filter builds by build profile if build ID is not provided.
-   `-p, --platform=<option>` Filter builds by the platform if build ID is not provided <options: android|ios|all>.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas build:configure`

Configure the project to support EAS Build.

Usage

```sh
eas build:configure [-p android|ios|all]
```

Flag

-   `-p, --platform=<option>` Platform to configure <options: android|ios|all>.

### `eas build:delete [BUILD_ID]`

Delete a build.

Usage

```sh
eas build:delete [BUILD_ID] [--non-interactive] [-p android|ios|all] [-e PROFILE_NAME]
```

Flags

-   `-e, --profile=PROFILE_NAME` Filter builds by build profile if build ID is not provided.
-   `-p, --platform=<option>` Filter builds by the platform if build ID is not provided <options: android|ios|all>.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas build:dev`

Run dev client simulator/emulator build with matching fingerprint or create a new one.

Usage

```sh
eas build:dev [-p ios|android] [-e PROFILE_NAME] [--skip-build-if-not-found] [--skip-bundler] [--simulator
]
```

Flags

-   `-e, --profile=PROFILE_NAME` Name of the build profile from **eas.json.** It must be a profile allowing to create emulator/simulator internal distribution dev client builds. The "development-simulator" build profile will be selected by default.
-   `-p, --platform=<option>` <options: ios|android>.
-   `--simulator=<value>` IOS simulator name or UDID to install and run the development build on. If no value is provided, you will be prompted to select a simulator.
-   `--skip-build-if-not-found` Skip build if no successful build with matching fingerprint is found.
-   `--skip-bundler` Install and run the development build without starting the bundler server.

### `eas build:download`

Download a simulator/emulator build by build ID or fingerprint hash.

Usage

```sh
eas build:download [--build-id  | --fingerprint  | -p ios|android | --dev-client]
[--all-artifacts] [--json] [--non-interactive]
```

Flags

-   `-p, --platform=<option>` <options: ios|android>.
-   `--all-artifacts` Download all available build artifacts (build artifacts archive, Xcode logs, etc.) in addition to the application archive. Without this flag, only the application archive is downloaded and the command errors if it is missing.
-   `--build-id=<value>` ID of the build to download. Mutually exclusive with `--fingerprint`, `--platform`, and `--dev-client`; the platform is derived from the build itself.
-   `--[no-]dev-client` Filter only dev-client builds.
-   `--fingerprint=<value>` Fingerprint hash of the build to download.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas build:inspect`

Inspect the state of the project at specific build stages, useful for troubleshooting.

Usage

```sh
eas build:inspect -p android|ios -s archive|pre-build|post-build -o OUTPUT_DIRECTORY [-e PROFILE_NAME]
[--force] [-v]
```

Flags

-   `-e, --profile=PROFILE_NAME` Name of the build profile from **eas.json.** Defaults to "production" if defined in **eas.json.**
-   `-o, --output=OUTPUT_DIRECTORY` (required) Output directory.
-   `-p, --platform=<option>` (required) <options: android|ios>.
-   `-s, --stage=<option>` (required) Stage of the build you want to inspect.
    -   `archive` Builds the project archive that would be uploaded to EAS when building.
    -   `pre-build` Prepares the project to be built with Gradle/Xcode. Does not run the native build.
    -   `post-build` Builds the native project and leaves the output directory for inspection <options: archive|pre-build|post-build>.
-   `-v, --verbose`
-   `--force` Delete OUTPUT_DIRECTORY if it already exists.

### `eas build:list`

List all builds for your project.

Usage

```sh
eas build:list [-p android|ios|all] [--status
new|in-queue|in-progress|pending-cancel|errored|finished|canceled] [--distribution store|internal|simulator]
[--channel ] [--app-version ] [--app-build-version ] [--sdk-version ] [--runtime-version
] [--app-identifier ] [-e ] [--git-commit-hash ] [--fingerprint-hash ] [--offset
] [--limit ] [--json] [--non-interactive] [--simulator]
```

Flags

-   `-e, --build-profile=<value>` Filter only builds created with the specified build profile.
-   `-p, --platform=<option>` <options: android|ios|all>.
-   `--app-build-version=<value>` Filter only builds created with the specified app build version.
-   `--app-identifier=<value>` Filter only builds created with the specified app identifier.
-   `--app-version=<value>` Filter only builds created with the specified main app version.
-   `--channel=<value>`
-   `--distribution=<option>` Filter only builds with the specified distribution type <options: store|internal|simulator>.
-   `--fingerprint-hash=<value>` Filter only builds with the specified fingerprint hash.
-   `--git-commit-hash=<value>` Filter only builds created with the specified git commit hash.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 10 and is capped at 50.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--offset=<value>` Start queries from specified index. Use for paginating results. Defaults to 0.
-   `--runtime-version=<value>` Filter only builds created with the specified runtime version.
-   `--sdk-version=<value>` Filter only builds created with the specified Expo SDK version.
-   `--simulator` Filter only iOS simulator builds. Can only be used with `--platform` flag set to "ios".
-   `--status=<option>` Filter only builds with the specified status <options: new|in-queue|in-progress|pending-cancel|errored|finished|canceled>.

### `eas build:resign`

Re-sign a build archive.

Usage

```sh
eas build:resign [-p android|ios] [-e PROFILE_NAME] [--source-profile PROFILE_NAME] [--wait] [--id ]
[--offset ] [--limit ] [--json] [--non-interactive]
```

Flags

-   `-e, --target-profile=PROFILE_NAME` Name of the target build profile from **eas.json.** Credentials and environment variables from this profile will be used when re-signing. Defaults to "production" if defined in **eas.json.**
-   `-p, --platform=<option>` <options: android|ios>.
-   `--id=<value>` ID of the build to re-sign.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 50 and is capped at 100.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--offset=<value>` Start queries from specified index. Use for paginating results. Defaults to 0.
-   `--source-profile=PROFILE_NAME` Name of the source build profile from **eas.json.** Used to filter builds eligible for re-signing.
-   `--[no-]wait` Wait for build(s) to complete.

### `eas build:run`

Run simulator/emulator builds from eas-cli.

Usage

```sh
eas build:run [--latest | --id  | --path  | --url ] [-p android|ios] [-e PROFILE_NAME]
[--simulator ] [--offset ] [--limit ]
```

Flags

-   `-e, --profile=PROFILE_NAME` Name of the build profile used to create the build to run. When specified, only builds created with the specified build profile will be queried.
-   `-p, --platform=<option>` <options: android|ios>.
-   `--id=<value>` ID of the simulator/emulator build to run.
-   `--latest` Run the latest simulator/emulator build for specified platform.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 50 and is capped at 100.
-   `--offset=<value>` Start queries from specified index. Use for paginating results. Defaults to 0.
-   `--path=<value>` Path to the simulator/emulator build archive or app.
-   `--simulator=<value>` IOS simulator name or UDID to install and run the build on. If no value is provided, you will be prompted to select a simulator.
-   `--url=<value>` Simulator/Emulator build archive url.

### `eas build:submit`

Submit app binary to App Store and/or Play Store.

Usage

```sh
eas build:submit [-p android|ios|all] [-e ] [--latest | --id  | --path  | --url ]
[--what-to-test ] [--verbose] [--wait] [--verbose-fastlane] [-g ...] [--non-interactive]
```

Flags

-   `-e, --profile=<value>` Name of the submit profile from **eas.json.** Defaults to "production" if defined in **eas.json.**
-   `-g, --groups=<value>...` Internal TestFlight testing groups to add the build to (iOS only). Learn more: [https://developer.apple.com/help/app-store-connect/test-a-beta-version/add-internal-testers](https://developer.apple.com/help/app-store-connect/test-a-beta-version/add-internal-testers).
-   `-p, --platform=<option>` <options: android|ios|all>.
-   `--id=<value>` ID of the build to submit.
-   `--latest` Submit the latest build for specified platform.
-   `--non-interactive` Run command in non-interactive mode.
-   `--path=<value>` Path to the **.apk**/**.aab**/**.ipa** file.
-   `--url=<value>` App archive url.
-   `--verbose` Always print logs from EAS Submit.
-   `--verbose-fastlane` Enable verbose logging for the submission process.
-   `--[no-]wait` Wait for submission to complete.
-   `--what-to-test=<value>` Sets the "What to test" information in TestFlight (iOS only).

Alias

```sh
eas build:submit
```

### `eas build:version:get`

Get the latest version from EAS servers.

Usage

```sh
eas build:version:get [-p android|ios|all] [-e PROFILE_NAME] [--json] [--non-interactive]
```

Flags

-   `-e, --profile=PROFILE_NAME` Name of the build profile from **eas.json.** Defaults to "production" if defined in **eas.json.**
-   `-p, --platform=<option>` <options: android|ios|all>.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas build:version:set`

Update version of an app.

Usage

```sh
eas build:version:set [-p android|ios] [-e PROFILE_NAME]
```

Flags

-   `-e, --profile=PROFILE_NAME` Name of the build profile from **eas.json.** Defaults to "production" if defined in **eas.json.**
-   `-p, --platform=<option>` <options: android|ios>.

### `eas build:version:sync`

Update a version in native code with a value stored on EAS servers.

Usage

```sh
eas build:version:sync [-p android|ios|all] [-e PROFILE_NAME]
```

Flags

-   `-e, --profile=PROFILE_NAME` Name of the build profile from **eas.json.** Defaults to "production" if defined in **eas.json.**
-   `-p, --platform=<option>` <options: android|ios|all>.

### `eas build:view [BUILD_ID]`

View a build for your project.

Usage

```sh
eas build:view [BUILD_ID] [--json]
```

Flag

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`.

### `eas channel:create [NAME]`

Create a channel.

Usage

```sh
eas channel:create [NAME] [--json] [--non-interactive]
```

Argument

-   `[NAME]` Name of the channel to create.

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas channel:delete [NAME]`

Delete a channel.

Usage

```sh
eas channel:delete [NAME] [--json] [--non-interactive]
```

Argument

-   `[NAME]` Name of the channel to delete.

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas channel:edit [NAME]`

Point a channel at a new branch.

Usage

```sh
eas channel:edit [NAME] [--branch ] [--json] [--non-interactive]
```

Argument

-   `[NAME]` Name of the channel to edit.

Flags

-   `--branch=<value>` Name of the branch to point to.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas channel:insights`

Display adoption, crash, and unique-user insights for a channel + runtime version.

Usage

```sh
eas channel:insights --channel  --runtime-version  [--days  | --start  | --end
] [--json] [--non-interactive]
```

Flags

-   `--channel=<value>` (required) Name of the channel.
-   `--days=<value>` Show insights from the last N days (default 7, mutually exclusive with `--start`/`--end`).
-   `--end=<value>` End of insights time range (ISO date).
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--runtime-version=<value>` (required) Runtime version to query insights for.
-   `--start=<value>` Start of insights time range (ISO date).

### `eas channel:list`

List all channels.

Usage

```sh
eas channel:list [--offset ] [--limit ] [--json] [--non-interactive]
```

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 10 and is capped at 25.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--offset=<value>` Start queries from specified index. Use for paginating results. Defaults to 0.

### `eas channel:pause [NAME]`

Pause a channel to stop it from sending updates.

Usage

```sh
eas channel:pause [NAME] [--branch ] [--json] [--non-interactive]
```

Argument

-   `[NAME]` Name of the channel to edit.

Flags

-   `--branch=<value>` Name of the branch to point to.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas channel:resume [NAME]`

Resume a channel to start sending updates.

Usage

```sh
eas channel:resume [NAME] [--branch ] [--json] [--non-interactive]
```

Argument

-   `[NAME]` Name of the channel to edit.

Flags

-   `--branch=<value>` Name of the branch to point to.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas channel:rollout [CHANNEL]`

Roll a new branch out on a channel incrementally.

Usage

```sh
eas channel:rollout [CHANNEL] [--action create|edit|end|view] [--percent ] [--outcome
republish-and-revert|revert] [--branch ] [--runtime-version ] [--private-key-path ] [--json]
[--non-interactive]
```

Argument

-   `[CHANNEL]` Channel on which the rollout should be done.

Flags

-   `--action=<option>` Rollout action to perform <options: create|edit|end|view>.
-   `--branch=<value>` Branch to roll out. Use with `--action=create`.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--outcome=<option>` End outcome of rollout. Use with `--action=end` <options: republish-and-revert|revert>.
-   `--percent=<value>` Percent of users to send to the new branch. Use with `--action=edit` or `--action=create`.
-   `--private-key-path=<value>` File containing the PEM-encoded private key corresponding to the certificate in expo-updates' configuration. Defaults to a file named "**private-key.pem**" in the certificate's directory. Only relevant if you are using code signing: [https://docs.expo.dev/eas-update/code-signing/](https://docs.expo.dev/eas-update/code-signing.md).
-   `--runtime-version=<value>` Runtime version to target. Use with `--action=create`.

### `eas channel:view [NAME]`

View a channel.

Usage

```sh
eas channel:view [NAME] [--json] [--non-interactive] [--offset ] [--limit ]
```

Argument

-   `[NAME]` Name of the channel to view.

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 50 and is capped at 100.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--offset=<value>` Start queries from specified index. Use for paginating results. Defaults to 0.

### `eas config`

Display project configuration (**app.json** + **eas.json**).

Usage

```sh
eas config [-p android|ios] [-e PROFILE_NAME] [--json] [--non-interactive]
```

Flags

-   `-e, --profile=PROFILE_NAME` Name of the build profile from **eas.json.** Defaults to "production" if defined in **eas.json.**
-   `-p, --platform=<option>` <options: android|ios>.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas credentials`

Manage credentials.

Usage

```sh
eas credentials [-p android|ios]
```

Flag

-   `-p, --platform=<option>` <options: android|ios>.

### `eas credentials:configure-build`

Set up credentials for building your project.

Usage

```sh
eas credentials:configure-build [-p android|ios] [-e PROFILE_NAME]
```

Flags

-   `-e, --profile=PROFILE_NAME` The name of the build profile in **eas.json.**
-   `-p, --platform=<option>` <options: android|ios>.

### `eas deploy [options]`

Deploy your Expo Router web build and API Routes.

Usage

```sh
eas deploy [options]
eas deploy --prod
eas deploy --non-interactive --dev-domain my-app
```

Flags

-   `--alias=name` Custom alias to assign to the new deployment.
-   `--dev-domain=name` Custom preview URL subdomain to assign to the project on its first deployment, for example, "my-app" for **my-app.expo.app.** Required with `--non-interactive` if you want to customize the preview URL.
-   `--dry-run` Outputs a tarball of the new deployment instead of uploading it.
-   `--environment=<value>` Environment variable's environment, for example, 'production', 'preview', 'development'.
-   `--export-dir=dir` [default: dist] Directory where the Expo project was exported.
-   `--id=xyz123` Custom unique identifier for the new deployment.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--prod` Create a new production deployment.
-   `--[no-]source-maps` Include source maps in the deployment.

Alias

```sh
eas worker:deploy
```

### `eas deploy:alias`

Assign deployment aliases.

Usage

```sh
eas deploy:alias [--prod] [--alias name] [--id xyz123] [--json] [--non-interactive]
```

Flags

-   `--alias=name` Custom alias to assign to the existing deployment.
-   `--id=xyz123` Unique identifier of an existing deployment.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--prod` Promote an existing deployment to production.

Aliases

```sh
eas worker:alias
eas deploy:promote
```

### `eas deploy:alias:delete [ALIAS_NAME]`

Delete deployment aliases.

Usage

```sh
eas deploy:alias:delete [ALIAS_NAME] [--json] [--non-interactive]
```

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

Alias

```sh
eas worker:alias:delete
```

### `eas deploy:delete [DEPLOYMENT_ID]`

Delete a deployment.

Usage

```sh
eas deploy:delete [DEPLOYMENT_ID] [--json] [--non-interactive]
```

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

Alias

```sh
eas worker:delete
```

### `eas deploy:promote`

Assign deployment aliases.

Usage

```sh
eas deploy:promote [--prod] [--alias name] [--id xyz123] [--json] [--non-interactive]
```

Flags

-   `--alias=name` Custom alias to assign to the existing deployment.
-   `--id=xyz123` Unique identifier of an existing deployment.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--prod` Promote an existing deployment to production.

Aliases

```sh
eas worker:alias
eas deploy:promote
```

### `eas device:create`

Register new Apple Devices to use for internal distribution.

Usage

```sh
eas device:create
```

### `eas device:delete`

Remove a registered device from your account.

Usage

```sh
eas device:delete [--apple-team-id ] [--udid ] [--json] [--non-interactive]
```

Flags

-   `--apple-team-id=<value>` The Apple team ID on which to find the device.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--udid=<value>` The Apple device ID to disable.

### `eas device:list`

List all registered devices for your account.

Usage

```sh
eas device:list [--apple-team-id ] [--offset ] [--limit ] [--json] [--non-interactive]
```

Flags

-   `--apple-team-id=<value>`
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 50 and is capped at 100.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--offset=<value>` Start queries from specified index. Use for paginating results. Defaults to 0.

### `eas device:rename`

Rename a registered device.

Usage

```sh
eas device:rename [--apple-team-id ] [--udid ] [--name ] [--json] [--non-interactive]
```

Flags

-   `--apple-team-id=<value>` The Apple team ID on which to find the device.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--name=<value>` The new name for the device.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--udid=<value>` The Apple device ID to rename.

### `eas device:view [UDID]`

View a device for your project.

Usage

```sh
eas device:view [UDID]
```

### `eas diagnostics`

Display environment info.

Usage

```sh
eas diagnostics
```

### `eas env:delete [ENVIRONMENT]`

Delete an environment variable for the current project or account.

Usage

```sh
eas env:delete [ENVIRONMENT] [--variable-name ] [--variable-environment ] [--scope
project|account] [--non-interactive]
```

Argument

-   `[ENVIRONMENT]` Current environment of the variable to delete. Default environments are 'production', 'preview', and 'development'.

Flags

-   `--non-interactive` Run the command in non-interactive mode.
-   `--scope=<option>` [default: project] Scope for the variable <options: project|account>.
-   `--variable-environment=<value>` Current environment of the variable to delete.
-   `--variable-name=<value>` Name of the variable to delete.

### `eas env:exec ENVIRONMENT BASH_COMMAND`

Execute a command with environment variables from the selected environment.

Usage

```sh
eas env:exec ENVIRONMENT BASH_COMMAND [--non-interactive]
```

Arguments

-   `ENVIRONMENT` Environment to execute the command in. Default environments are 'production', 'preview', and 'development'.
-   `BASH_COMMAND` Bash command to execute with the environment variables from the environment.

Flag

-   `--non-interactive` Run the command in non-interactive mode.

### `eas env:get [ENVIRONMENT]`

View an environment variable for the current project or account.

Usage

```sh
eas env:get [ENVIRONMENT] [--variable-name ] [--variable-environment ] [--format
long|short] [--scope project|account] [--non-interactive]
```

Argument

-   `[ENVIRONMENT]` Current environment of the variable. Default environments are 'production', 'preview', and 'development'.

Flags

-   `--format=<option>` [default: short] Output format <options: long|short>.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--scope=<option>` [default: project] Scope for the variable <options: project|account>.
-   `--variable-environment=<value>` Current environment of the variable.
-   `--variable-name=<value>` Name of the variable.

### `eas env:list [ENVIRONMENT]`

List environment variables for the current project or account.

Usage

```sh
eas env:list [ENVIRONMENT] [--include-sensitive] [--include-file-content] [--environment ...]
[--format long|short] [--scope project|account]
```

Argument

-   `[ENVIRONMENT]` Environment to list the variables from. Default environments are 'production', 'preview', and 'development'.

Flags

-   `--environment=<value>...` Environment variable's environment, for example, 'production', 'preview', 'development'.
-   `--format=<option>` [default: short] Output format <options: long|short>.
-   `--include-file-content` Display files content in the output.
-   `--include-sensitive` Display sensitive values in the output.
-   `--scope=<option>` [default: project] Scope for the variable <options: project|account>.

### `eas env:pull [ENVIRONMENT]`

Pull environment variables for the selected environment to **.env** file.

Usage

```sh
eas env:pull [ENVIRONMENT] [--non-interactive] [--environment ] [--path ]
```

Argument

-   `[ENVIRONMENT]` Environment to pull variables from. Default environments are 'production', 'preview', and 'development'.

Flags

-   `--environment=<value>` Environment variable's environment, for example, 'production', 'preview', 'development'.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--path=<value>` [default: **.env.local**] Path to the result `.env` file.

### `eas env:push [ENVIRONMENT]`

Push environment variables from **.env** file to the selected environment.

Usage

```sh
eas env:push [ENVIRONMENT] [--environment ...] [--path ] [--force]
```

Argument

-   `[ENVIRONMENT]` Environment to push variables to. Default environments are 'production', 'preview', and 'development'.

Flags

-   `--environment=<value>...` Environment variable's environment, for example, 'production', 'preview', 'development'.
-   `--force` Skip confirmation and automatically override existing variables.
-   `--path=<value>` [default: **.env.local**] Path to the input `.env` file.

### `eas env:set [ENVIRONMENT]`

Set (create or update) an environment variable on the current project or account.

Usage

```sh
eas env:set [ENVIRONMENT] [--name ] [--value ] [--type string|file] [--visibility
plaintext|sensitive|secret] [--scope project|account] [--environment ...] [--json] [--non-interactive]
```

Argument

-   `[ENVIRONMENT]` Environment to set the variable in. Default environments are 'production', 'preview', and 'development'.

Flags

-   `--environment=<value>...` Environment variable's environment, for example, 'production', 'preview', 'development'.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--name=<value>` Name of the variable.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--scope=<option>` [default: project] Scope for the variable <options: project|account>.
-   `--type=<option>` The type of variable <options: string|file>.
-   `--value=<value>` Text value of the variable.
-   `--visibility=<option>` Visibility of the variable <options: plaintext|sensitive|secret>.

### `eas fingerprint:compare [HASH1] [HASH2]`

Compare fingerprints of the current project, builds, and updates.

Usage

```sh
eas fingerprint:compare [HASH1...] [HASH2...] [--build-id ...] [--update-id ...] [--open]
[--environment ] [--json] [--non-interactive]
```

Arguments

-   `[HASH1...]` If provided alone, HASH1 is compared against the current project's fingerprint.
-   `[HASH2...]` If two hashes are provided, HASH1 is compared against HASH2.

Flags

-   `--build-id=<value>...` Compare the fingerprint with the build with the specified ID.
-   `--environment=<value>` If generating a fingerprint from the local directory, use the specified environment.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--open` Open the fingerprint comparison in the browser.
-   `--update-id=<value>...` Compare the fingerprint with the update with the specified ID.

Examples

```sh
eas fingerprint:compare 	 # Compare fingerprints in interactive mode
eas fingerprint:compare  	 # Compare fingerprint against local directory
eas fingerprint:compare   	 # Compare provided fingerprints
eas fingerprint:compare --build-id  	 # Compare fingerprint from build against local directory
eas fingerprint:compare --build-id  --environment production 	 # Compare fingerprint from build against local directory with the "production" environment
eas fingerprint:compare --build-id  --build-id 	 # Compare fingerprint from a build against another build
eas fingerprint:compare --build-id  --update-id 	 # Compare fingerprint from build against fingerprint from update
eas fingerprint:compare  --update-id  	 # Compare fingerprint from update against provided fingerprint
```

### `eas fingerprint:generate`

Generate fingerprints from the current project.

Usage

```sh
eas fingerprint:generate [-p android|ios] [--environment  | -e ] [--json] [--non-interactive]
```

Flags

-   `-e, --build-profile=<value>` Name of the build profile from **eas.json.**
-   `-p, --platform=<option>` <options: android|ios>.
-   `--environment=<value>` Environment variable's environment, for example, 'production', 'preview', 'development'.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

Examples

```sh
eas fingerprint:generate  	 # Generate fingerprint in interactive mode
eas fingerprint:generate --build-profile preview  	 # Generate a fingerprint using the "preview" build profile
eas fingerprint:generate --environment preview  	 # Generate a fingerprint using the "preview" environment
eas fingerprint:generate --json --non-interactive --platform android  	 # Output fingerprint json to stdout
```

### `eas help [COMMAND]`

Display help for eas.

Usage

```sh
eas help [COMMAND...] [-n]
```

Argument

-   `[COMMAND...]` Command to show help for.

Flag

-   `-n, --nested-commands` Include all nested commands in the output.

### `eas init`

Create or link an EAS project.

Usage

```sh
eas init [--account  | --id ] [--force] [--json] [--non-interactive]
```

Flags

-   `--account=<value>` Name of the account that will own the project.
-   `--force` Whether to create a new project/link an existing project without additional prompts or overwrite any existing project ID when running with `--id` flag.
-   `--id=<value>` ID of the EAS project to link.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

Alias

```sh
eas init
```

Examples

```sh
eas init  	 # Create or link a project interactively
eas init --id   	 # Link to the project with the given ID
eas init --account my-account --non-interactive  	 # Create or link @my-account/ without prompts
eas init --account my-account --json --non-interactive  	 # Same, and print the result as JSON to stdout
```

### `eas integrations:asc:connect`

Connect a project to an App Store Connect app.

Usage

```sh
eas integrations:asc:connect [--api-key-id ] [--asc-app-id ] [--bundle-id ] [--json]
[--non-interactive]
```

Flags

-   `--api-key-id=<value>` Apple App Store Connect API Key ID.
-   `--asc-app-id=<value>` App Store Connect app identifier.
-   `--bundle-id=<value>` Filter discovered apps by bundle identifier.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas integrations:asc:disconnect`

Disconnect the current project from its App Store Connect app.

Usage

```sh
eas integrations:asc:disconnect [--yes] [--json] [--non-interactive]
```

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--yes` Skip confirmation prompt.

### `eas integrations:asc:status`

Show the App Store Connect app link status for the current project.

Usage

```sh
eas integrations:asc:status [--json] [--non-interactive]
```

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas integrations:convex:connect`

Connect Convex to your Expo project.

Usage

```sh
eas integrations:convex:connect [--non-interactive] [--region aws-us-east-1|aws-eu-west-1] [--team-name ]
[--project-name ]
```

Flags

-   `--non-interactive` Run the command in non-interactive mode.
-   `--project-name=<value>` Name for the Convex project (defaults to app slug).
-   `--region=<option>` Convex deployment region (for example, aws-us-east-1, aws-eu-west-1) <options: aws-us-east-1|aws-eu-west-1>.
-   `--team-name=<value>` Name for the new Convex team (defaults to EAS account name).

### `eas integrations:convex:dashboard`

Open the Convex dashboard for the linked Convex project.

Usage

```sh
eas integrations:convex:dashboard
```

### `eas integrations:convex:project`

Display the Convex project linked to the current Expo app.

Usage

```sh
eas integrations:convex:project
```

### `eas integrations:convex:project:delete`

Remove the Convex project link for the current Expo app from EAS servers.

Usage

```sh
eas integrations:convex:project:delete [--non-interactive] [-y]
```

Flags

-   `-y, --yes` Skip confirmation prompt.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas integrations:convex:team`

Display Convex teams linked to the current Expo app's owner account.

Usage

```sh
eas integrations:convex:team
```

### `eas integrations:convex:team:delete [CONVEX_TEAM]`

Remove a Convex team link from the current Expo app owner account's EAS servers.

Usage

```sh
eas integrations:convex:team:delete [CONVEX_TEAM] [--non-interactive] [-y]
```

Argument

-   `[CONVEX_TEAM]` Slug of the Convex team to remove.

Flags

-   `-y, --yes` Skip confirmation prompt.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas integrations:convex:team:invite [CONVEX_TEAM]`

Send a Convex team invitation to your verified email address.

Usage

```sh
eas integrations:convex:team:invite [CONVEX_TEAM] [--non-interactive]
```

Argument

-   `[CONVEX_TEAM]` Slug of the Convex team to invite yourself to.

Flag

-   `--non-interactive` Run the command in non-interactive mode.

### `eas integrations:posthog:connect`

Connect PostHog to your Expo project.

Usage

```sh
eas integrations:posthog:connect [--json] [--non-interactive] [--region US|EU] [--session-replay] [--error-tracking]
[--posthog-cli-api-key ] [--overwrite]
```

Flags

-   `--[no-]error-tracking` Set up PostHog error tracking / source maps (requires a personal API key).
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--overwrite` Overwrite existing PostHog environment variables without prompting.
-   `--posthog-cli-api-key=<value>` PostHog personal API key for error-tracking source-map uploads (enables error tracking non-interactively).
-   `--region=<option>` PostHog region <options: US|EU>.
-   `--[no-]session-replay` Set up PostHog session replay (default: yes).

### `eas integrations:posthog:dashboard`

Open the PostHog dashboard for the linked PostHog project.

Usage

```sh
eas integrations:posthog:dashboard [--show-link] [--json] [--non-interactive]
```

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--show-link` Print the signed-in dashboard URL in addition to opening it. The URL contains a single-use login token.

### `eas integrations:posthog:disconnect`

Remove the PostHog project link for the current Expo app from EAS servers.

Usage

```sh
eas integrations:posthog:disconnect [--json] [--non-interactive] [-y]
```

Flags

-   `-y, --yes` Skip confirmation prompt.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas login`

Log in with your Expo account.

Usage

```sh
eas login [-s] [-b]
```

Flags

-   `-b, --[no-]browser` Log in with your browser (default; use `--no-browser` for CLI-based login).
-   `-s, --sso` Log in with SSO.

Alias

```sh
eas login
```

### `eas logout`

Log out.

Usage

```sh
eas logout
```

Alias

```sh
eas logout
```

### `eas metadata:lint`

Validate the local store configuration.

Usage

```sh
eas metadata:lint [--json] [--profile ]
```

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`.
-   `--profile=<value>` Name of the submit profile from **eas.json.** Defaults to "production" if defined in **eas.json.**

### `eas metadata:pull`

Generate the local store configuration from the app stores.

Usage

```sh
eas metadata:pull [-e ] [--non-interactive]
```

Flags

-   `-e, --profile=<value>` Name of the submit profile from **eas.json.** Defaults to "production" if defined in **eas.json.**
-   `--non-interactive` Run the command in non-interactive mode.

### `eas metadata:push`

Sync the local store configuration to the app stores.

Usage

```sh
eas metadata:push [-e ] [--non-interactive]
```

Flags

-   `-e, --profile=<value>` Name of the submit profile from **eas.json.** Defaults to "production" if defined in **eas.json.**
-   `--non-interactive` Run the command in non-interactive mode.

### `eas new [PATH]`

Create a new project configured with Expo Application Services (EAS).

Usage

```sh
eas new [PATH] [-p bun|npm|pnpm|yarn]
```

Argument

-   `[PATH]` Path to create the project (defaults to current directory).

Flag

-   `-p, --package-manager=<option>` [default: npm] Package manager to use for installing dependencies <options: bun|npm|pnpm|yarn>.

Alias

```sh
eas new
```

### `eas observe:events [EVENTNAME]`

Display individual events emitted by the app via `logEvent`, filtered by the event name in the argument. With no arguments, a list of the available event names and associated event counts is returned.

Usage

```sh
eas observe:events [EVENTNAME] [--platform android|ios] [--after ] [--limit ] [--start  |
--days ] [--end  | ] [--app-version ] [--update-id ] [--session-id ]
[--all-events] [--project-id ] [--json] [--non-interactive]
```

Argument

-   `[EVENTNAME]` Event name to filter by.

Flags

-   `--after=<value>` Cursor for pagination. Use the endCursor from a previous query to fetch the next page.
-   `--all-events` When no event name argument is provided, list all events across all event names instead of a summary of event names + counts.
-   `--app-version=<value>` Filter by app version.
-   `--days=<value>` Show results from the last N days (mutually exclusive with `--start`/`--end`).
-   `--end=<value>` End of time range (ISO date).
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 10 and is capped at 100.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--platform=<option>` Filter by platform <options: android|ios>.
-   `--project-id=<value>` EAS project ID (defaults to the project ID of the current directory).
-   `--session-id=<value>` Filter by session ID. When no event name is given, lists the events in the session instead of the event-name summary.
-   `--start=<value>` Start of time range (ISO date).
-   `--update-id=<value>` Filter by EAS update ID.

### `eas observe:metrics [METRIC]`

Display individual performance metric samples ordered by value.

Usage

```sh
eas observe:metrics [METRIC] [--sort slowest|fastest|newest|oldest] [--platform android|ios] [--after ]
[--limit ] [--start  | --days ] [--end  | ] [--app-version ] [--update-id
] [--project-id ] [--json] [--non-interactive]
```

Argument

-   `[METRIC]` (nav_cold_ttr|nav_warm_ttr|nav_tti|tti|ttr|cold_launch|warm_launch|bundle_load|update_download) Metric to query (for example, tti, cold_launch, nav_tti).

Flags

-   `--after=<value>` Cursor for pagination. Use the endCursor from a previous query to fetch the next page.
-   `--app-version=<value>` Filter by app version.
-   `--days=<value>` Show results from the last N days (mutually exclusive with `--start`/`--end`).
-   `--end=<value>` End of time range (ISO date).
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 10 and is capped at 100.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--platform=<option>` Filter by platform <options: android|ios>.
-   `--project-id=<value>` EAS project ID (defaults to the project ID of the current directory).
-   `--sort=<option>` [default: oldest] Sort order for events <options: slowest|fastest|newest|oldest>.
-   `--start=<value>` Start of time range (ISO date).
-   `--update-id=<value>` Filter by EAS update ID.

### `eas observe:metrics-summary`

Display aggregated performance metric statistics grouped by app version.

Usage

```sh
eas observe:metrics-summary [--platform android|ios] [--metric
nav_cold_ttr|nav_warm_ttr|nav_tti|tti|ttr|cold_launch|warm_launch|bundle_load|update_download...] [--stat
min|median|max|average|p80|p90|p99|eventCount...] [--start  | --days ] [--end  | ]
[--project-id ] [--json] [--non-interactive]
```

Flags

-   `--days=<value>` Show results from the last N days (mutually exclusive with `--start`/`--end`).
-   `--end=<value>` End of time range (ISO date).
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--metric=<option>...` Metric name to display (can be specified multiple times). <options: nav_cold_ttr|nav_warm_ttr|nav_tti|tti|ttr|cold_launch|warm_launch|bundle_load|update_download>.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--platform=<option>` Filter by platform <options: android|ios>.
-   `--project-id=<value>` EAS project ID (defaults to the project ID of the current directory).
-   `--start=<value>` Start of time range (ISO date).
-   `--stat=<option>...` Statistic to display per metric (can be specified multiple times) <options: min|median|max|average|p80|p90|p99|eventCount>.

### `eas observe:routes`

Display app navigation route metrics (Cold TTR, Warm TTR, TTI) grouped by route name.

Usage

```sh
eas observe:routes [--platform android|ios] [--metric nav_cold_ttr|nav_warm_ttr|nav_tti...] [--stat
median|med|p90|count|event_count|eventCount...] [--after ] [--limit ] [--start  | --days
] [--end  | ] [--app-version ] [--update-id ] [--build-number ] [--route-name
...] [--project-id ] [--json] [--non-interactive]
```

Flags

-   `--after=<value>` Cursor for pagination. Use the endCursor from a previous query to fetch the next page.
-   `--app-version=<value>` Filter by app version.
-   `--build-number=<value>` Filter by app build number.
-   `--days=<value>` Show results from the last N days (mutually exclusive with `--start`/`--end`).
-   `--end=<value>` End of time range (ISO date).
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 50 and is capped at 200.
-   `--metric=<option>...` Navigation metric to display (can be specified multiple times). Defaults to all three. <options: nav_cold_ttr|nav_warm_ttr|nav_tti>.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--platform=<option>` Filter by platform <options: android|ios>.
-   `--project-id=<value>` EAS project ID (defaults to the project ID of the current directory).
-   `--route-name=<value>...` Filter by route name (can be specified multiple times to include several routes).
-   `--start=<value>` Start of time range (ISO date).
-   `--stat=<option>...` Statistic to display per metric (can be specified multiple times) <options: median|med|p90|count|event_count|eventCount>.
-   `--update-id=<value>` Filter by EAS update ID.

### `eas observe:session [SESSIONID]`

Display the timeline of metric and log events for a specific session.

Usage

```sh
eas observe:session [SESSIONID] [--sort slowest|fastest|newest|oldest] [--event-name ] [--start  |
--days ] [--end  | ] [--project-id ] [--json] [--non-interactive]
```

Argument

-   `[SESSIONID]` Session ID to inspect (omit in interactive mode to pick one from a list).

Flags

-   `--days=<value>` Show results from the last N days (mutually exclusive with `--start`/`--end`).
-   `--end=<value>` End of time range (ISO date).
-   `--event-name=<value>` Metric or log event name to pick candidate sessions by (for example, tti, cold_launch, login_pressed). If omitted in interactive mode, you will be prompted.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--project-id=<value>` EAS project ID (defaults to the project ID of the current directory).
-   `--sort=<option>` Sort order for candidate events when picking a session (if omitted in interactive mode, you will be prompted) <options: slowest|fastest|newest|oldest>.
-   `--start=<value>` Start of time range (ISO date).

### `eas observe:versions`

Display app versions with build and update details.

Usage

```sh
eas observe:versions [--platform android|ios] [--start  | --days ] [--end  | ] [--project-id
] [--json] [--non-interactive]
```

Flags

-   `--days=<value>` Show results from the last N days (mutually exclusive with `--start`/`--end`).
-   `--end=<value>` End of time range (ISO date).
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--platform=<option>` Filter by platform <options: android|ios>.
-   `--project-id=<value>` EAS project ID (defaults to the project ID of the current directory).
-   `--start=<value>` Start of time range (ISO date).

### `eas project:delete [NAME]`

Delete a project.

Usage

```sh
eas project:delete [NAME] [--dangerously-confirm-deletion ] [--json] [--non-interactive]
```

Argument

-   `[NAME]` Full name (@account/slug) or ID of the project to delete. Defaults to the project in the current directory.

Flags

-   `--dangerously-confirm-deletion=<value>` The project's full name (@account/slug), to confirm deletion. Required in non-interactive mode.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas project:icon:set PATH`

Set the project icon displayed on the EAS dashboard.

Usage

```sh
eas project:icon:set PATH [--non-interactive]
```

Argument

-   `PATH` Path to the icon image (PNG or JPEG, at most 10 MB). Non-square images are center-cropped to a square.

Flag

-   `--non-interactive` Run the command in non-interactive mode.

### `eas project:info`

Information about the current project.

Usage

```sh
eas project:info
```

### `eas project:init`

Create or link an EAS project.

Usage

```sh
eas project:init [--account  | --id ] [--force] [--json] [--non-interactive]
```

Flags

-   `--account=<value>` Name of the account that will own the project.
-   `--force` Whether to create a new project/link an existing project without additional prompts or overwrite any existing project ID when running with `--id` flag.
-   `--id=<value>` ID of the EAS project to link.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

Alias

```sh
eas init
```

Examples

```sh
eas init  	 # Create or link a project interactively
eas init --id   	 # Link to the project with the given ID
eas init --account my-account --non-interactive  	 # Create or link @my-account/ without prompts
eas init --account my-account --json --non-interactive  	 # Same, and print the result as JSON to stdout
```

### `eas project:new [PATH]`

Create a new project configured with Expo Application Services (EAS).

Usage

```sh
eas project:new [PATH] [-p bun|npm|pnpm|yarn]
```

Argument

-   `[PATH]` Path to create the project (defaults to current directory).

Flag

-   `-p, --package-manager=<option>` [default: npm] Package manager to use for installing dependencies <options: bun|npm|pnpm|yarn>.

Alias

```sh
eas new
```

### `eas sim`

[EXPERIMENTAL] start a remote simulator session on EAS and get instructions to connect to it.

Usage

```sh
eas sim [-p android|ios] [--name ] [--type agent-device|argent|serve-sim] [--package-version
] [--max-duration-minutes ] [--force] [--out-config-type env|dotenv] [--json] [--non-interactive]
```

Flags

-   `-p, --platform=<option>` Device platform <options: android|ios>.
-   `--[no-]force` [default: true] Create a new simulator session even when an existing simulator session is present in the environment.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--max-duration-minutes=<value>` Maximum duration of the simulator session in minutes before it is automatically stopped. Only customizable on paid plans. Defaults to a value derived from the job run priority when omitted.
-   `--name=<value>` Human-readable name for the simulator session, shown in eas simulator:list and on **expo.dev.** Defaults to unnamed.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--out-config-type=<option>` [default: dotenv] How to output simulator connection configuration. Use "env" to print shell exports, or "dotenv" to write **.env.eas-simulator.** <options: env|dotenv>.
-   `--package-version=<value>` Version of the package backing the simulator session (for example, "**0.1.3-alpha.3**"). Defaults to "latest" when omitted.
-   `--type=<option>` [default: agent-device] Type of simulator session to create <options: agent-device|argent|serve-sim>.

Aliases

```sh
eas simulator:start
eas sim
eas sim:start
```

### `eas sim:availability`

[EXPERIMENTAL] check whether EAS Simulator is enabled for the current project account.

Usage

```sh
eas sim:availability [--json] [--non-interactive]
```

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

Alias

```sh
eas sim:availability
```

### `eas sim:events`

[EXPERIMENTAL] show activity events from a remote simulator session.

Usage

```sh
eas sim:events [--id ] [-f] [--json] [--non-interactive]
```

Flags

-   `-f, --follow` Keep watching for new events until the session ends.
-   `--id=<value>` Simulator session ID. Defaults to **.env.eas-simulator.**
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

Alias

```sh
eas sim:events
```

### `eas sim:exec`

[EXPERIMENTAL] execute a simulator command with **.env.eas-simulator** environment loaded.

Usage

```sh
eas sim:exec
```

Alias

```sh
eas sim:exec
```

### `eas sim:get`

[EXPERIMENTAL] get info about a remote simulator session on EAS by its simulator session ID.

Usage

```sh
eas sim:get [--id ] [--json] [--non-interactive]
```

Flags

-   `--id=<value>` Simulator session ID. Defaults to **.env.eas-simulator.**
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

Alias

```sh
eas sim:get
```

### `eas sim:list`

[EXPERIMENTAL] list remote simulator sessions for the current project.

Usage

```sh
eas sim:list [--status new|in-progress|stopped|errored...] [--type agent-device|argent|serve-sim...]
[--platform android|ios...] [--name ] [--limit ] [--after ] [--json] [--non-interactive]
```

Flags

-   `--after=<value>` Cursor for pagination. Use the endCursor from a previous query to fetch the next page.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 10 and is capped at 100.
-   `--name=<value>` Filter by session name (case-insensitive prefix match).
-   `--non-interactive` Run the command in non-interactive mode.
-   `--platform=<option>...` Filter by device platform (repeatable) <options: android|ios>.
-   `--status=<option>...` Filter by session status (repeatable) <options: new|in-progress|stopped|errored>.
-   `--type=<option>...` Filter by session type (repeatable) <options: agent-device|argent|serve-sim>.

Alias

```sh
eas sim:list
```

### `eas sim:start`

[EXPERIMENTAL] start a remote simulator session on EAS and get instructions to connect to it.

Usage

```sh
eas sim:start [-p android|ios] [--name ] [--type agent-device|argent|serve-sim] [--package-version
] [--max-duration-minutes ] [--force] [--out-config-type env|dotenv] [--json] [--non-interactive]
```

Flags

-   `-p, --platform=<option>` Device platform <options: android|ios>.
-   `--[no-]force` [default: true] Create a new simulator session even when an existing simulator session is present in the environment.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--max-duration-minutes=<value>` Maximum duration of the simulator session in minutes before it is automatically stopped. Only customizable on paid plans. Defaults to a value derived from the job run priority when omitted.
-   `--name=<value>` Human-readable name for the simulator session, shown in eas simulator:list and on **expo.dev.** Defaults to unnamed.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--out-config-type=<option>` [default: dotenv] How to output simulator connection configuration. Use "env" to print shell exports, or "dotenv" to write **.env.eas-simulator.** <options: env|dotenv>.
-   `--package-version=<value>` Version of the package backing the simulator session (for example, "**0.1.3-alpha.3**"). Defaults to "latest" when omitted.
-   `--type=<option>` [default: agent-device] Type of simulator session to create <options: agent-device|argent|serve-sim>.

Aliases

```sh
eas simulator:start
eas sim
eas sim:start
```

### `eas sim:stop`

[EXPERIMENTAL] stop a remote simulator session on EAS by its simulator session ID.

Usage

```sh
eas sim:stop [--id ] [--json] [--non-interactive]
```

Flags

-   `--id=<value>` Simulator session ID. Defaults to **.env.eas-simulator.**
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

Alias

```sh
eas sim:stop
```

### `eas simulator:start`

[EXPERIMENTAL] start a remote simulator session on EAS and get instructions to connect to it.

Usage

```sh
eas simulator:start [-p android|ios] [--name ] [--type agent-device|argent|serve-sim] [--package-version
] [--max-duration-minutes ] [--force] [--out-config-type env|dotenv] [--json] [--non-interactive]
```

Flags

-   `-p, --platform=<option>` Device platform <options: android|ios>.
-   `--[no-]force` [default: true] Create a new simulator session even when an existing simulator session is present in the environment.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--max-duration-minutes=<value>` Maximum duration of the simulator session in minutes before it is automatically stopped. Only customizable on paid plans. Defaults to a value derived from the job run priority when omitted.
-   `--name=<value>` Human-readable name for the simulator session, shown in eas simulator:list and on **expo.dev.** Defaults to unnamed.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--out-config-type=<option>` [default: dotenv] How to output simulator connection configuration. Use "env" to print shell exports, or "dotenv" to write **.env.eas-simulator.** <options: env|dotenv>.
-   `--package-version=<value>` Version of the package backing the simulator session (for example, "**0.1.3-alpha.3**"). Defaults to "latest" when omitted.
-   `--type=<option>` [default: agent-device] Type of simulator session to create <options: agent-device|argent|serve-sim>.

Aliases

```sh
eas simulator:start
eas sim
eas sim:start
```

### `eas submit`

Submit app binary to App Store and/or Play Store.

Usage

```sh
eas submit [-p android|ios|all] [-e ] [--latest | --id  | --path  | --url ]
[--what-to-test ] [--verbose] [--wait] [--verbose-fastlane] [-g ...] [--non-interactive]
```

Flags

-   `-e, --profile=<value>` Name of the submit profile from **eas.json.** Defaults to "production" if defined in **eas.json.**
-   `-g, --groups=<value>...` Internal TestFlight testing groups to add the build to (iOS only). Learn more: [https://developer.apple.com/help/app-store-connect/test-a-beta-version/add-internal-testers](https://developer.apple.com/help/app-store-connect/test-a-beta-version/add-internal-testers).
-   `-p, --platform=<option>` <options: android|ios|all>.
-   `--id=<value>` ID of the build to submit.
-   `--latest` Submit the latest build for specified platform.
-   `--non-interactive` Run command in non-interactive mode.
-   `--path=<value>` Path to the **.apk**/**.aab**/**.ipa** file.
-   `--url=<value>` App archive url.
-   `--verbose` Always print logs from EAS Submit.
-   `--verbose-fastlane` Enable verbose logging for the submission process.
-   `--[no-]wait` Wait for submission to complete.
-   `--what-to-test=<value>` Sets the "What to test" information in TestFlight (iOS only).

Alias

```sh
eas build:submit
```

### `eas submit:cancel [SUBMISSION_ID]`

Cancel a submission.

Usage

```sh
eas submit:cancel [SUBMISSION_ID] [--non-interactive] [-p android|ios|all]
```

Flags

-   `-p, --platform=<option>` Filter submissions by the platform if submission ID is not provided <options: android|ios|all>.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas submit:list`

List submissions for your project.

Usage

```sh
eas submit:list [-p android|ios|all] [--status awaiting-build|in-queue|in-progress|finished|errored|canceled]
[--offset ] [--limit ] [--json] [--non-interactive]
```

Flags

-   `-p, --platform=<option>` <options: android|ios|all>.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 10 and is capped at 50.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--offset=<value>` Start queries from specified index. Use for paginating results. Defaults to 0.
-   `--status=<option>` Filter only submissions with the specified status <options: awaiting-build|in-queue|in-progress|finished|errored|canceled>.

### `eas submit:retry [SUBMISSION_ID]`

Retry a failed submission.

Usage

```sh
eas submit:retry [SUBMISSION_ID] [-p android|ios|all] [--json] [--non-interactive]
```

Flags

-   `-p, --platform=<option>` Filter submissions by the platform if submission ID is not provided <options: android|ios|all>.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas submit:status`

Show the status of your app on the App Store: the live version and TestFlight builds.

Usage

```sh
eas submit:status [-p android|ios|all] [-e ] [--json] [--non-interactive]
```

Flags

-   `-e, --profile=<value>` Name of the submit profile from **eas.json** used to resolve the App Store Connect API key. Defaults to "production".
-   `-p, --platform=<option>` <options: android|ios|all>.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

Examples

```sh
eas submit:status  	 # live App Store version and TestFlight builds
eas submit:status --json --non-interactive  	 # machine-readable output
```

### `eas submit:view [SUBMISSION_ID]`

View a submission for your project.

Usage

```sh
eas submit:view [SUBMISSION_ID] [-p android|ios|all] [--json]
```

Flags

-   `-p, --platform=<option>` Show the most recent submission for the platform when submission ID is not provided <options: android|ios|all>.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`.

### `eas testflight:crashes [ID]`

Display crashes reported by TestFlight testers, or the full crash log of a single crash.

Usage

```sh
eas testflight:crashes [ID] [--type crash|screenshot] [-e ] [--offset ] [--limit ] [--json]
[--non-interactive]
```

Argument

-   `[ID]` ID or App Store Connect API URL of a single submission to show. Accepts ${{ **app_store_connect.beta_feedback.id** }} or ${{ **app_store_connect.beta_feedback.url** }} from an EAS workflow trigger.

Flags

-   `-e, --profile=<value>` Name of the submit profile from **eas.json** used to resolve the bundle identifier and App Store Connect API key. Defaults to "production".
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 20 and is capped at 200.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--offset=<value>` Start queries from specified index. Use for paginating results. Defaults to 0.
-   `--type=<option>` Kind of feedback the ID refers to. Only needed when passing a bare ID for screenshot feedback; a URL already encodes it. <options: crash|screenshot>.

Examples

```sh
eas testflight:crashes  	 # Show the most recent crashes
eas testflight:crashes --limit 50 --offset 20  	 # Page through crashes
eas testflight:crashes AAo2eIIfGzcb1BzuUv3xrh4  	 # Show the full crash log for one crash
eas testflight:crashes --json  	 # Print a page of crashes, with paging metadata, as JSON
eas testflight:crashes ${{ app_store_connect.beta_feedback.url }} --json  	 # Look up whatever an EAS workflow trigger reported
```

### `eas testflight:feedback [ID]`

Display screenshot feedback submitted by TestFlight testers, including their comments, device information, and screenshot URLs.

Usage

```sh
eas testflight:feedback [ID] [--type crash|screenshot] [-e ] [--offset ] [--limit ] [--json]
[--non-interactive]
```

Argument

-   `[ID]` ID or App Store Connect API URL of a single submission to show. Accepts ${{ **app_store_connect.beta_feedback.id** }} or ${{ **app_store_connect.beta_feedback.url** }} from an EAS workflow trigger.

Flags

-   `-e, --profile=<value>` Name of the submit profile from **eas.json** used to resolve the bundle identifier and App Store Connect API key. Defaults to "production".
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 20 and is capped at 200.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--offset=<value>` Start queries from specified index. Use for paginating results. Defaults to 0.
-   `--type=<option>` Kind of feedback the ID refers to. Only needed when passing a bare ID for a crash; a URL already encodes it. <options: crash|screenshot>.

Examples

```sh
eas testflight:feedback  	 # Show the most recent feedback submissions
eas testflight:feedback --limit 50 --offset 20  	 # Page through submissions
eas testflight:feedback --json  	 # Print a page of feedback, with paging metadata, as JSON
eas testflight:feedback AD8JvKbr0BK0Cj9OnM6WO6I  	 # Show a single submission by ID
eas testflight:feedback ${{ app_store_connect.beta_feedback.url }} --json  	 # Look up whatever an EAS workflow trigger reported
```

### `eas update`

Publish an update group.

Usage

```sh
eas update [--branch ] [--channel ] [-m ] [--input-dir ] [--skip-bundler]
[--clear-cache] [--emit-metadata] [--rollout-percentage ] [-p android|ios|all] [--auto] [--private-key-path
] [--environment ] [--json] [--non-interactive]
```

Flags

-   `-m, --message=<value>` A short message describing the update.
-   `-p, --platform=<option>` [default: all] <options: android|ios|all>.
-   `--auto` Use the current git branch and commit message for the EAS branch and update message.
-   `--branch=<value>` Branch to publish the update group on.
-   `--channel=<value>` Channel that the published update should affect.
-   `--clear-cache` Clear the bundler cache before publishing.
-   `--emit-metadata` Emit "**eas-update-metadata.json**" in the bundle folder with detailed information about the generated updates.
-   `--environment=<value>` Environment to use for the server-side defined EAS environment variables during command execution, for example, "production", "preview", "development". Required for projects using Expo SDK 55 or greater.
-   `--input-dir=<value>` [default: dist] Location of the bundle.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--private-key-path=<value>` File containing the PEM-encoded private key corresponding to the certificate in expo-updates' configuration. Defaults to a file named "**private-key.pem**" in the certificate's directory. Only relevant if you are using code signing: [https://docs.expo.dev/eas-update/code-signing/](https://docs.expo.dev/eas-update/code-signing.md).
-   `--rollout-percentage=<value>` Percentage of users this update should be immediately available to. Users not in the rollout will be served the previous latest update on the branch, even if that update is itself being rolled out. The specified number must be an integer between 1 and 100. When not specified, this defaults to 100.
-   `--skip-bundler` Skip running Expo CLI to bundle the app before publishing.

### `eas update:configure`

Configure the project to support EAS Update.

Usage

```sh
eas update:configure [-p android|ios|all] [--environment ] [--non-interactive]
```

Flags

-   `-p, --platform=<option>` [default: all] Platform to configure <options: android|ios|all>.
-   `--environment=<value>` Environment to use for the server-side defined EAS environment variables during command execution, for example, "production", "preview", "development".
-   `--non-interactive` Run the command in non-interactive mode.

### `eas update:delete GROUPID`

Delete all the updates in an update group.

Usage

```sh
eas update:delete GROUPID [--json] [--non-interactive]
```

Argument

-   `GROUPID` The ID of an update group to delete.

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas update:edit [GROUPID]`

Edit all the updates in an update group.

Usage

```sh
eas update:edit [GROUPID] [--rollout-percentage ] [--branch ] [--json] [--non-interactive]
```

Argument

-   `[GROUPID]` The ID of an update group to edit.

Flags

-   `--branch=<value>` Branch for which to list updates to select from.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--rollout-percentage=<value>` Rollout percentage to set for a rollout update. The specified number must be an integer between 1 and 100.

### `eas update:embedded:delete ID`

Delete an embedded update registered with EAS Update.

Usage

```sh
eas update:embedded:delete ID [--json] [--non-interactive]
```

Argument

-   `ID` The ID of the embedded update (manifest UUID from **app.manifest**).

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas update:embedded:list`

List embedded updates registered with EAS Update for this project.

Usage

```sh
eas update:embedded:list [-p ios|android] [--runtime-version ] [--channel ] [--limit ]
[--after-cursor ] [--json] [--non-interactive]
```

Flags

-   `-p, --platform=<option>` Filter by platform <options: ios|android>.
-   `--after-cursor=<value>` Return items after this cursor (for pagination).
-   `--channel=<value>` Filter by channel name (pass "all" to skip the channel prompt).
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 25 and is capped at 50.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--runtime-version=<value>` Filter by runtime version.

### `eas update:embedded:upload`

Upload the JS bundle embedded in a native build so EAS Update can generate bsdiff patches against it.

Usage

```sh
eas update:embedded:upload -p ios|android --bundle  --manifest  --channel  [--build-id ]
[--json] [--non-interactive]
```

Flags

-   `-p, --platform=<option>` (required) Platform of the embedded bundle <options: ios|android>.
-   `--build-id=<value>` EAS Build ID that produced this binary (required when invoked from EAS Build).
-   `--bundle=<value>` (required) Path to the embedded JS bundle file.
-   `--channel=<value>` (required) Channel name the embedded update should be associated with.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--manifest=<value>` (required) Path to the **app.manifest** file embedded in the build.
-   `--non-interactive` Run the command in non-interactive mode.

Examples

```sh
eas update:embedded:upload --platform ios --bundle ios/build/App.app/main.jsbundle --manifest ios/build/App.app/app.manifest --channel production
eas update:embedded:upload --platform android --bundle android/app/src/main/assets/index.android.bundle --manifest android/app/src/main/assets/app.manifest --channel production --build-id
```

### `eas update:embedded:view ID`

View details of an embedded update registered with EAS Update.

Usage

```sh
eas update:embedded:view ID [--json]
```

Argument

-   `ID` The ID of the embedded update (manifest UUID from **app.manifest**).

Flag

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`.

### `eas update:insights GROUPID`

Display launch, crash, unique-user, and size insights for an update group.

Usage

```sh
eas update:insights GROUPID [--platform ios|android] [--days  | --start  | --end ] [--json]
[--non-interactive]
```

Argument

-   `GROUPID` The ID of an update group.

Flags

-   `--days=<value>` Show insights from the last N days (default 7, mutually exclusive with `--start`/`--end`).
-   `--end=<value>` End of insights time range (ISO date).
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--platform=<option>` Filter to a single platform. <options: ios|android>.
-   `--start=<value>` Start of insights time range (ISO date).

### `eas update:list`

View the recent updates.

Usage

```sh
eas update:list [--branch  | --all] [-p android|ios|all] [--runtime-version ] [--offset
] [--limit ] [--json] [--non-interactive]
```

Flags

-   `-p, --platform=<option>` Filter updates by platform <options: android|ios|all>.
-   `--all` List updates on all branches.
-   `--branch=<value>` List updates only on this branch.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 25 and is capped at 50.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--offset=<value>` Start queries from specified index. Use for paginating results. Defaults to 0.
-   `--runtime-version=<value>` Filter updates by runtime version.

### `eas update:republish`

Roll back to an existing update.

Usage

```sh
eas update:republish [--channel  | --branch  | --group ] [--destination-channel  |
--destination-branch ] [-m ] [-p android|ios|all] [--private-key-path ] [--rollout-percentage
] [--json] [--non-interactive]
```

Flags

-   `-m, --message=<value>` Short message describing the republished update group.
-   `-p, --platform=<option>` [default: all] <options: android|ios|all>.
-   `--branch=<value>` Branch name to select an update group to republish from.
-   `--channel=<value>` Channel name to select an update group to republish from.
-   `--destination-branch=<value>` Branch name to republish to if republishing to a different branch.
-   `--destination-channel=<value>` Channel name to select a branch to republish to if republishing to a different branch.
-   `--group=<value>` Update group ID to republish.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--private-key-path=<value>` File containing the PEM-encoded private key corresponding to the certificate in expo-updates' configuration. Defaults to a file named "**private-key.pem**" in the certificate's directory. Only relevant if you are using code signing: [https://docs.expo.dev/eas-update/code-signing/](https://docs.expo.dev/eas-update/code-signing.md).
-   `--rollout-percentage=<value>` Percentage of users this update should be immediately available to. Users not in the rollout will be served the previous latest update on the branch, even if that update is itself being rolled out. The specified number must be an integer between 1 and 100. When not specified, this defaults to 100.

### `eas update:revert-update-rollout`

Revert a rollout update for a project.

Usage

```sh
eas update:revert-update-rollout [--channel  | --branch  | --group ] [-m ] [--private-key-path
] [--json] [--non-interactive]
```

Flags

-   `-m, --message=<value>` Short message describing the revert.
-   `--branch=<value>` Branch name to select an update group to revert the rollout update from.
-   `--channel=<value>` Channel name to select an update group to revert the rollout update from.
-   `--group=<value>` Rollout update group ID to revert.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--private-key-path=<value>` File containing the PEM-encoded private key corresponding to the certificate in expo-updates' configuration. Defaults to a file named "**private-key.pem**" in the certificate's directory. Only relevant if you are using code signing: [https://docs.expo.dev/eas-update/code-signing/](https://docs.expo.dev/eas-update/code-signing.md).

### `eas update:roll-back-to-embedded`

Roll back to the embedded update.

Usage

```sh
eas update:roll-back-to-embedded [--branch ] [--channel ] [--runtime-version ] [--message ] [-p
android|ios|all] [--private-key-path ] [--json] [--non-interactive]
```

Flags

-   `-p, --platform=<option>` [default: all] <options: android|ios|all>.
-   `--branch=<value>` Branch to publish the rollback to embedded update group on.
-   `--channel=<value>` Channel that the published rollback to embedded update should affect.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--message=<value>` A short message describing the rollback to embedded update.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--private-key-path=<value>` File containing the PEM-encoded private key corresponding to the certificate in expo-updates' configuration. Defaults to a file named "**private-key.pem**" in the certificate's directory. Only relevant if you are using code signing: [https://docs.expo.dev/eas-update/code-signing/](https://docs.expo.dev/eas-update/code-signing.md).
-   `--runtime-version=<value>` Runtime version that the rollback to embedded update should target.

### `eas update:rollback [GROUPID]`

Roll back to an embedded update or an existing update.

Usage

```sh
eas update:rollback [GROUPID] [-m ] [-p android|ios|all] [--private-key-path ] [--json]
[--non-interactive]
```

Argument

-   `[GROUPID]` The ID of the update group to roll back. Must be the latest update for its branch and runtime version. The update group published before it is republished; if there is none, a roll back to the embedded update is published. Required in non-interactive mode.

Flags

-   `-m, --message=<value>` Short message describing the rollback update.
-   `-p, --platform=<option>` [default: all] <options: android|ios|all>.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--private-key-path=<value>` File containing the PEM-encoded private key corresponding to the certificate in expo-updates' configuration. Defaults to a file named "**private-key.pem**" in the certificate's directory. Only relevant if you are using code signing: [https://docs.expo.dev/eas-update/code-signing/](https://docs.expo.dev/eas-update/code-signing.md).

### `eas update:view GROUPID`

Update group details.

Usage

```sh
eas update:view GROUPID [--insights] [--days  | --start  | --end ] [--json]
```

Argument

-   `GROUPID` The ID of an update group, or the ID of a platform-specific update.

Flags

-   `--days=<value>` Show insights from the last N days (default 7). Only used with `--insights`.
-   `--end=<value>` End of insights time range (ISO date). Only used with `--insights`.
-   `--insights` Also show insights (launches, crash rate, unique users, payload size) for the update group.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`.
-   `--start=<value>` Start of insights time range (ISO date). Only used with `--insights`.

### `eas upload`

Upload a local build and generate a sharable link.

Usage

```sh
eas upload [-p ios|android] [--build-path ] [--fingerprint ] [--json] [--non-interactive]
```

Flags

-   `-p, --platform=<option>` <options: ios|android>.
-   `--build-path=<value>` Path for the local build.
-   `--fingerprint=<value>` Fingerprint hash of the local build.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas webhook:create`

Create a webhook.

Usage

```sh
eas webhook:create [--event BUILD|SUBMIT] [--url ] [--secret ] [--non-interactive]
```

Flags

-   `--event=<option>` Event type that triggers the webhook <options: BUILD|SUBMIT>.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--secret=<value>` Secret used to create a hash signature of the request payload, provided in the 'Expo-Signature' header.
-   `--url=<value>` Webhook URL.

### `eas webhook:delete [ID]`

Delete a webhook.

Usage

```sh
eas webhook:delete [ID] [--non-interactive]
```

Argument

-   `[ID]` ID of the webhook to delete.

Flag

-   `--non-interactive` Run the command in non-interactive mode.

### `eas webhook:list`

List webhooks.

Usage

```sh
eas webhook:list [--event BUILD|SUBMIT] [--json]
```

Flags

-   `--event=<option>` Event type that triggers the webhook <options: BUILD|SUBMIT>.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`.

### `eas webhook:update`

Update a webhook.

Usage

```sh
eas webhook:update --id  [--event BUILD|SUBMIT] [--url ] [--secret ] [--non-interactive]
```

Flags

-   `--event=<option>` Event type that triggers the webhook <options: BUILD|SUBMIT>.
-   `--id=<value>` (required) Webhook ID.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--secret=<value>` Secret used to create a hash signature of the request payload, provided in the 'Expo-Signature' header.
-   `--url=<value>` Webhook URL.

### `eas webhook:view ID`

View a webhook.

Usage

```sh
eas webhook:view ID
```

Argument

-   `ID` ID of the webhook to view.

### `eas whoami`

Show the username you are logged in as.

Usage

```sh
eas whoami
```

Alias

```sh
eas whoami
```

### `eas worker:alias`

Assign deployment aliases.

Usage

```sh
eas worker:alias [--prod] [--alias name] [--id xyz123] [--json] [--non-interactive]
```

Flags

-   `--alias=name` Custom alias to assign to the existing deployment.
-   `--id=xyz123` Unique identifier of an existing deployment.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--prod` Promote an existing deployment to production.

Aliases

```sh
eas worker:alias
eas deploy:promote
```

### `eas worker:alias:delete [ALIAS_NAME]`

Delete deployment aliases.

Usage

```sh
eas worker:alias:delete [ALIAS_NAME] [--json] [--non-interactive]
```

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

Alias

```sh
eas worker:alias:delete
```

### `eas worker:delete [DEPLOYMENT_ID]`

Delete a deployment.

Usage

```sh
eas worker:delete [DEPLOYMENT_ID] [--json] [--non-interactive]
```

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`. Implies `--non-interactive`.
-   `--non-interactive` Run the command in non-interactive mode.

Alias

```sh
eas worker:delete
```

### `eas workflow:cancel`

Cancel one or more workflow runs. If no workflow run IDs are provided, you will be prompted to select IN_PROGRESS runs to cancel.

Usage

```sh
eas workflow:cancel [--non-interactive]
```

Flag

-   `--non-interactive` Run the command in non-interactive mode.

### `eas workflow:create [NAME]`

Create a new workflow configuration YAML file.

Usage

```sh
eas workflow:create [NAME] [--template build|update|deploy|custom] [--skip-validation]
```

Argument

-   `[NAME]` Name of the workflow file. When provided without `--template`, a placeholder workflow is created.

Flags

-   `--skip-validation` If set, the workflow file will not be validated before being created.
-   `--template=<option>` Template to use for the workflow file <options: build|update|deploy|custom>.

### `eas workflow:logs [ID]`

View logs for a workflow run, selecting a job and step to view. You can pass in either a workflow run ID or a job ID. If no ID is passed in, you will be prompted to select from recent workflow runs for the current project.

Usage

```sh
eas workflow:logs [ID] [--json] [--non-interactive] [--all-steps]
```

Argument

-   `[ID]` ID of the workflow run or workflow job to view logs for.

Flags

-   `--all-steps` Print all logs, rather than prompting for a specific step. This will be automatically set when in non-interactive mode.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`.
-   `--non-interactive` Run the command in non-interactive mode.

### `eas workflow:run FILE`

Run an EAS workflow. The entire local project directory will be packaged and uploaded to EAS servers for the workflow run, unless the `--ref` flag is used.

Usage

```sh
eas workflow:run FILE [--non-interactive] [--wait] [-F ...] [--ref ] [--json]
```

Argument

-   `FILE` Path to the workflow file to run.

Flags

-   `-F, --input=<value>...` Set workflow inputs.
-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--ref=<value>` Git reference to run the workflow on.
-   `--[no-]wait` Wait for workflow run to complete. Defaults to false.

### `eas workflow:runs`

List recent workflow runs for this project, with their IDs, statuses, and timestamps.

Usage

```sh
eas workflow:runs [--workflow ] [--status ACTION_REQUIRED|CANCELED|FAILURE|IN_PROGRESS|NEW|SUCCESS]
[--json] [--limit ]
```

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`.
-   `--limit=<value>` The number of items to fetch each query. Defaults to 10 and is capped at 100.
-   `--status=<option>` If present, filter the returned runs to select those with the specified status <options: ACTION_REQUIRED|CANCELED|FAILURE|IN_PROGRESS|NEW|SUCCESS>.
-   `--workflow=<value>` If present, the query will only return runs for the specified workflow file name.

### `eas workflow:status [WORKFLOW_RUN_ID]`

Show the status of an existing workflow run. If no run ID is provided, you will be prompted to select from recent workflow runs for the current project.

Usage

```sh
eas workflow:status [WORKFLOW_RUN_ID] [--non-interactive] [--wait] [--json]
```

Argument

-   `[WORKFLOW_RUN_ID]` A workflow run ID.

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`.
-   `--non-interactive` Run the command in non-interactive mode.
-   `--[no-]wait` Wait for workflow run to complete. Defaults to false.

### `eas workflow:validate PATH`

Validate a workflow configuration yaml file.

Usage

```sh
eas workflow:validate PATH [--non-interactive]
```

Argument

-   `PATH` Path to the workflow configuration YAML file (must end with **.yml** or **.yaml**).

Flag

-   `--non-interactive` Run the command in non-interactive mode.

### `eas workflow:view [ID]`

View details for a workflow run, including jobs. If no run ID is provided, you will be prompted to select from recent workflow runs for the current project.

Usage

```sh
eas workflow:view [ID] [--json] [--non-interactive]
```

Argument

-   `[ID]` ID of the workflow run to view.

Flags

-   `--json` Enable JSON output, non-JSON messages will be printed to `stderr`.
-   `--non-interactive` Run the command in non-interactive mode.
