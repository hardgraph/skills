> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/install-options/install-with-npm.md).

# Install with npm

npm is a quick way to get started with n8n on your local machine. You must have [Node.js](https://nodejs.org/en/) installed. n8n requires a Node.js version between 20.19 and 24.x, inclusive.

{% hint style="info" %}
**Stable and Beta versions**

n8n releases a new minor version most weeks. The `stable` version is for production use. `beta` is the most recent release. The `beta` version may be unstable. To report issues, use the [forum](https://community.n8n.io/c/questions/12).

Current `stable`: 2.33.7 Current `beta`: 2.34.4
{% endhint %}

## Try n8n with npx <a href="#try-n8n-with-npx" id="try-n8n-with-npx"></a>

You can try n8n without installing it using npx.

From the terminal, run:

```bash
npx n8n
```

This command will download everything that's needed to start n8n. You can then access n8n and start building workflows by opening <http://localhost:5678>.

## Install globally with npm <a href="#install-globally-with-npm" id="install-globally-with-npm"></a>

To install n8n globally, use npm:

```bash
npm install n8n -g
```

To install or update to a specific version of n8n use the `@` syntax to specify the version. For example:

```bash
npm install -g n8n@0.126.1
```

To install `next`:

```bash
npm install -g n8n@next
```

After the installation, start n8n by running:

```bash
n8n
# or <a href="#or" id="or"></a>
n8n start
```

### Next steps <a href="#next-steps" id="next-steps"></a>

Try out n8n using the [Quickstarts](/build-your-first-workflow.md).

## Updating <a href="#updating" id="updating"></a>

To update your n8n instance to the `latest` version, run:

```bash
npm update -g n8n
```

To install the `next` version:

```bash
npm install -g n8n@next
```

### n8n with tunnel&#x20;

{% hint style="danger" %}
Use this for local development and testing. It isn't safe to use it in production.
{% endhint %}

{% hint style="warning" %}
**Development tooling**

The tunnel feature is a convenience tool for local development. The underlying implementation may change between n8n versions.
{% endhint %}

To use webhooks for trigger nodes of external services like GitHub, n8n has to be reachable from the web. n8n provides a tunnel service using [cloudflared](https://github.com/cloudflare/cloudflared) that redirects requests from the web to your local n8n instance. Docker must be installed for the tunnel to work.

There are two ways to use the tunnel, depending on how you run n8n:

{% hint style="info" %}
**Docker required**

The tunnel uses cloudflared, which runs as a Docker container. Make sure [Docker](https://docs.docker.com/get-docker/) is installed on your machine, even when running n8n via npm.
{% endhint %}

For npm installations, use the **services only** approach. Start cloudflared as a standalone service, then run n8n locally:

```bash
# Terminal 1: Start the cloudflared tunnel service <a href="#terminal-1-start-the-cloudflared-tunnel-service" id="terminal-1-start-the-cloudflared-tunnel-service"></a>
pnpm --filter n8n-containers services --services cloudflared

# Terminal 2: Start n8n locally <a href="#terminal-2-start-n8n-locally" id="terminal-2-start-n8n-locally"></a>
pnpm dev
```

The `services` command starts cloudflared, fetches the public tunnel URL, and writes a `.env` file to `packages/cli/bin/.env` with `WEBHOOK_URL` and `N8N_PROXY_HOPS=1`. n8n picks up this `.env` automatically on startup.

Clean up when done:

```bash
pnpm --filter n8n-containers services:clean
```

For the full stack approach (n8n and cloudflared both in containers), refer to the [Docker tunnel setup](/deploy/host-n8n/install-options/install-with-docker.md#n8n-with-tunnel).

## Reverting an upgrade <a href="#reverting-an-upgrade" id="reverting-an-upgrade"></a>

Install the older version that you want to go back to.

If the upgrade involved a database migration:

1. Check the feature documentation and release notes to see if there are any manual changes you need to make.
2. Run `n8n db:revert` on your current version to roll back the database. If you want to revert more than one database migration, you need to repeat this process.

## Windows troubleshooting <a href="#windows-troubleshooting" id="windows-troubleshooting"></a>

If you are experiencing issues running n8n on Windows, make sure your Node.js environment is correctly set up. Follow Microsoft's guide to [Install NodeJS on Windows](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-windows).
