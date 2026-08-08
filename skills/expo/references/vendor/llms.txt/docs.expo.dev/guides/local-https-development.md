---
modificationDate: June 30, 2026
title: Using local HTTPS development
description: Learn how to set up local HTTPS for Expo web apps.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/local-https-development/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/local-https-development/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Web
Pages in this section:
- [Develop websites](https://docs.expo.dev/workflow/web.md)
- [Publish websites](https://docs.expo.dev/guides/publishing-websites.md)
- [DOM components](https://docs.expo.dev/guides/dom-components.md)
- [React Server Components](https://docs.expo.dev/guides/server-components.md)
- [Testing RSC](https://docs.expo.dev/guides/testing-rsc.md)
- [Progressive web apps](https://docs.expo.dev/guides/progressive-web-apps.md)
- [Tailwind CSS](https://docs.expo.dev/guides/tailwind.md)
- [Local HTTPS development](https://docs.expo.dev/guides/local-https-development.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Using local HTTPS development

Learn how to set up local HTTPS for Expo web apps.

When developing Expo web apps locally, you may need to use HTTPS with your local development environment for testing secure browser APIs. This guide shows you how to set up local HTTPS for Expo web apps.

#### Prerequisites

##### mkcert installed

`mkcert` is a tool for creating development certificates. For installation instructions, see the [`mkcert` GitHub repository](https://github.com/FiloSottile/mkcert#installation).

## Benefits

-   **Team scalability**: Same setup works for everyone
-   **Authentication support**: HTTP-Only Cookies and secure contexts
-   **Production parity**: Match your production HTTPS environment
-   **Easy sharing**: Consistent development URLs across the team

## Set up your project

Create or navigate to your Expo project:

```sh
# npm
npx create-expo-app@latest example-app --template default@sdk-57
cd example-app
cd your-expo-project

# yarn
yarn create expo-app example-app --template default@sdk-57
cd example-app
cd your-expo-project

# pnpm
pnpm create expo-app example-app --template default@sdk-57
cd example-app
cd your-expo-project

# bun
bun create expo example-app --template default@sdk-57
cd example-app
cd your-expo-project
```

Start your Expo development server:

```sh
# npm
npx expo start --web

# yarn
yarn expo start --web

# pnpm
pnpm expo start --web

# bun
bun expo start --web
```

Your app will be running on `http://localhost:8081`. Keep this terminal window open.

Use `mkcert` to generate a certificate for localhost. Run the following command in a new terminal window from your project's root directory:

```sh
mkcert localhost
```

> **Tip**: Ensure that after installing `mkcert`, you run `mkcert -install` to install the local certificate authority (CA).

This will generate two signed certificate files: `localhost.pem` (certificate) and `localhost-key.pem` (private key), inside your project's root directory.

Inside your project's root directory, run the following command to start the proxy:

```sh
# npm
npx local-ssl-proxy --source 443 --target 8081 --cert localhost.pem --key localhost-key.pem

# yarn
yarn dlx local-ssl-proxy --source 443 --target 8081 --cert localhost.pem --key localhost-key.pem

# pnpm
pnpm dlx local-ssl-proxy --source 443 --target 8081 --cert localhost.pem --key localhost-key.pem

# bun
bunx local-ssl-proxy --source 443 --target 8081 --cert localhost.pem --key localhost-key.pem
```

> **Tip**: [`local-ssl-proxy`](https://github.com/cameronhunter/local-ssl-proxy) is a tool that creates a proxy server that forwards HTTPS traffic from port 443 to your Expo dev server on port 8081.

This creates a proxy that forwards HTTPS traffic from port 443 to your Expo dev server on port 8081.

Open `https://localhost` in your browser to access your app. Your Expo app is now running with HTTPS.
