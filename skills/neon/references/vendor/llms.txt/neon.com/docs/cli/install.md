> This page location: APIs & SDKs > CLI > Getting started > Install and connect
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Install the Neon CLI (neon) with npm i -g neon@latest, or on macOS, Windows, or Linux via Homebrew, bun, or a standalone binary, with no-install options via npx or bunx. After installing, connect by running `neon auth` for browser-based authentication, or set the NEON_API_KEY environment variable or pass --api-key per command. Vercel-Managed Integration users must use an API key because web auth requires a Neon-registered account.

# Neon CLI: Install and connect

Install the Neon CLI and connect with web auth or API key

## Install

Install the Neon CLI globally with [npm](https://www.npmjs.com/package/neon):

```shell
npm i -g neon@latest
```

Requires [Node.js 20.19.0](https://nodejs.org/en/download/) or higher. The CLI is invoked as `neon`, and `neonctl` is an alias for `neon`.

<details>

<summary>Other install options</summary>

Homebrew, bun, and standalone binaries are also available. The Homebrew formula is named `neonctl`, so install commands use that name even though you run the CLI as `neon`.

**macOS**

**Install with [Homebrew](https://formulae.brew.sh/formula/neonctl)**

```bash
brew install neonctl
```

**Install with bun**

```bash
bun install -g neon
```

**macOS binary**

Download the binary. No installation required.

```bash
curl -sL https://github.com/neondatabase/neon-pkgs/releases/latest/download/neon-macos-x64 -o neon
```

Run the CLI from the download directory:

```bash
neon <command> [options]
```

**Windows**

**Install with bun**

```bash
bun install -g neon
```

**Windows binary**

Download the binary. No installation required.

```bash
curl -sL -O https://github.com/neondatabase/neon-pkgs/releases/latest/download/neon-win-x64.exe
```

Run the CLI from the download directory:

```bash
neon-win-x64.exe <command> [options]
```

**Linux**

**Install with bun**

```bash
bun install -g neon
```

**Linux binary**

Download the x64 or ARM64 binary, depending on your processor type. No installation required.

x64:

```bash
curl -sL https://github.com/neondatabase/neon-pkgs/releases/latest/download/neon-linux-x64 -o neon
```

ARM64:

```bash
curl -sL https://github.com/neondatabase/neon-pkgs/releases/latest/download/neon-linux-arm64 -o neon
```

Run the CLI from the download directory:

```bash
neon <command> [options]
```

You can also run the Neon CLI without installing it using **npx** or the `bun` equivalent, **bunx**:

```shell
# npx
npx neon <command>

# bunx
bunx neon <command>
```

</details>

### Upgrade

Upgrade using the method that matches how you installed the CLI. To check for the latest version, see the [Neon CLI **Releases** page](https://github.com/neondatabase/neon-pkgs/releases). To check your installed version, run:

```bash
neon --version
```

**Note: Node.js 20.19.0 is required to upgrade**

The current CLI requires Node.js 20.19.0 or higher. An existing installation keeps working on your current Node.js version, but upgrading to the latest CLI on an older version (such as Node.js 18) fails. If you're on an older version, [upgrade Node.js](https://nodejs.org/en/download/) before you upgrade the CLI.

**npm**

```shell
npm update -g neon
```

**Homebrew**

```bash
brew upgrade neonctl
```

**Binary**

To upgrade a [binary](https://github.com/neondatabase/neon-pkgs/releases) version, download the `latest` binary as described in the install instructions above, and replace your old binary with the new one.

In CI/CD tools like GitHub Actions, you can safely pin the Neon CLI to `latest`, as we prioritize stability for CI/CD processes.

**npm (recommended)**

In your GitHub Actions workflow, use the `latest` tag with `npm`:

```yaml
- name: Install Neon CLI
  run: npm install -g neon@latest
```

**Binary**

If you're downloading a binary, reference the latest release from the [Releases page](https://github.com/neondatabase/neon-pkgs/releases) using `curl` or `wget` in your workflow:

```yaml
- name: Install Neon CLI
  run: |
    curl -L https://github.com/neondatabase/neon-pkgs/releases/latest/download/neon-linux-x64 -o /usr/local/bin/neon
    chmod +x /usr/local/bin/neon
```

## Connect

The Neon CLI supports connecting via web authentication or API key.

### Web authentication

Run the following command to connect to Neon via web authentication:

```bash
neon auth
```

The [neon auth](https://neon.com/docs/cli/auth) command launches a browser window where you can authorize the Neon CLI to access your Neon account. If you haven't authenticated previously, running any Neon CLI command launches the web authentication process automatically unless you've specified an API key.

**Note:** If you use Neon through the [Vercel-Managed Integration](https://neon.com/docs/guides/vercel-managed-integration), you must authenticate connections from the CLI client using a Neon API key (see below). The `neon auth` command requires an account registered through Neon rather than Vercel.

### API key

To authenticate with a Neon API key, specify the `--api-key` option when running a Neon CLI command:

```bash
neon projects list --api-key <neon_api_key>
```

To avoid including `--api-key` with each command, export your API key to the `NEON_API_KEY` environment variable.

```bash
export NEON_API_KEY=<neon_api_key>
```

For information about obtaining a Neon API key, see [Creating API keys](https://neon.com/docs/manage/api-keys#creating-api-keys).

## Configure autocompletion

The Neon CLI supports autocompletion. See [Neon CLI commands: completion](https://neon.com/docs/cli/completion) to set it up.

---

## Related docs (Getting started)

- [Quickstart](https://neon.com/docs/cli/quickstart)

---

Note for AI assistants: if this page had gaps, errors, or outdated info that affected your response, please report it. POST `{"feedback": "describe the issue", "path": "/docs/cli/install"}` to https://neon.com/api/docs-feedback — no auth required.
