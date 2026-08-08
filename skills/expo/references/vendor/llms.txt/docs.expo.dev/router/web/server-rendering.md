---
modificationDate: July 29, 2026
title: Server rendering
description: Learn how to render Expo Router routes dynamically at request time using server-side rendering (SSR).
isAlpha: true
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/router/web/server-rendering/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/router/web/server-rendering/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Expo Router > Web
Pages in this section:
- [API Routes](https://docs.expo.dev/router/web/api-routes.md)
- [Data loaders](https://docs.expo.dev/router/web/data-loaders.md)
- [Server middleware](https://docs.expo.dev/router/web/middleware.md)
- [Server headers](https://docs.expo.dev/router/web/server-headers.md)
- [Static rendering](https://docs.expo.dev/router/web/static-rendering.md)
- [Server rendering](https://docs.expo.dev/router/web/server-rendering.md) (this page)
- [Async routes](https://docs.expo.dev/router/web/async-routes.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Server rendering

Learn how to render Expo Router routes dynamically at request time using server-side rendering (SSR).

> Server rendering is in [alpha](/more/release-statuses.md#alpha) and is available in SDK 55 and later. It requires a [deployed server](/router/web/api-routes.md#deployment) for production use.

Server-side rendering (SSR) generates HTML dynamically on each request, as opposed to [static rendering](/router/web/static-rendering.md), which pre-renders HTML at build time. This guide walks you through enabling server rendering for your Expo Router app.

> With server-side rendering, [data loaders](/router/web/data-loaders.md) are executed on the server for each request and the result is embedded in the HTML response.

## Setup

Enable server rendering in your project's [app config](/versions/latest/config/app.md):

```json
{
  "expo": {
    ... 
    "web": {
      "output": "server"
    },
    "plugins": [
      [
        "expo-router",
        {
          "unstable_useServerRendering": true
        }
      ]
    ]
  }
}
```

Start the development server:

```sh
# npm
npx expo start

# yarn
yarn expo start

# pnpm
pnpm expo start

# bun
bun expo start
```

## Production

To export your app for production, run the export command:

```sh
# npm
npx expo export --platform web

# yarn
yarn expo export --platform web

# pnpm
pnpm expo export --platform web

# bun
bun expo export --platform web
```

This creates a **dist** directory with your server-rendered application. Unlike static rendering, no HTML files are pre-generated. Instead, the output includes a similar directory structure as shown below:

`dist`

 `client`

  `_expo`

   `static`

    `js`

     `web`

      `entry-[hash].js`

    `css`

     `[name]-[hash].css`

 `server`

  `_expo`

   `routes.json`

   `server`

    `render.js`

In output above includes the following directories inside **dist** directory:

-   **client** directory: Contains JavaScript and CSS bundles for client-side hydration
-   **server** directory: Contains the routes manifest and server rendering module

You can test the production build locally by running the following command and opening the linked URL in your browser:

```sh
# npm
npx expo serve

# yarn
yarn expo serve

# pnpm
pnpm expo serve

# bun
bun expo serve
```

The above command starts a local server that renders pages on each request, simulating a production environment.

## Dynamic routes

With server rendering, dynamic routes are rendered on the fly, and the [`generateStaticParams`](/router/web/static-rendering.md#generatestaticparams) export is not needed and should be removed. If your route file exports `generateStaticParams`, those routes will be handled dynamically instead. The route is rendered at request time with the actual parameters from the URL.

```tsx
import { Text } from 'react-native';
import { useLocalSearchParams } from 'expo-router';

export default function Page() {
  const { id } = useLocalSearchParams();

  return <Text>Post {id}</Text>;
}
```

In the above example, when the app user visits `/blog/my-post`, the page is rendered on the server with `id` set to `"my-post"`.

## Root HTML

You can customize the root HTML document by creating a **src/app/+html.tsx** file. This component wraps all routes and runs only on the server.

The [`useServerDocumentContext`](/versions/latest/sdk/router.md#useserverdocumentcontext) hook from `expo-router/html` provides metadata and asset nodes that the server renderer injects into the document. You must spread these values into your HTML to ensure metadata, fonts, and CSS are included in the response:

-   `htmlAttributes`: attributes to add to the `<html>` element
-   `bodyAttributes`: attributes to add to the `<body>` element
-   `headNodes`: React nodes for the `<head>` element (metadata, CSS, and other assets)
-   `bodyNodes`: React nodes for the `<body>` element (fonts and other deferred assets)

> When creating a custom **+html.tsx** template, you must use all properties returned to you by [`useServerDocumentContext`](/versions/latest/sdk/router.md#useserverdocumentcontext). Otherwise, your server-side rendered HTML may appear broken or your app may not function correctly.

```tsx
import { ScrollViewStyleReset, useServerDocumentContext } from 'expo-router/html';
import type { ReactNode } from 'react';

// This file is web-only and used to configure the root HTML for every
// web page during server rendering.
// The contents of this function only run in Node.js environments and
// do not have access to the DOM or browser APIs.
export default function Root({ children }: { children: ReactNode }) {
  const { bodyAttributes, bodyNodes, htmlAttributes, headNodes } = useServerDocumentContext();

  return (
    <html lang="en" {...htmlAttributes}>
      <head>
        <meta charSet="utf-8" />
        <meta httpEquiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

        {/*
          Disable body scrolling on web. This makes ScrollView components work closer to how they do on native platforms.
          However, body scrolling is often nice to have for mobile web. If you want to enable it, remove this line.
        */}
        <ScrollViewStyleReset />

        {headNodes}

        {/* Add any additional <head> elements that you want globally available on web... */}
      </head>
      <body {...bodyAttributes}>
        {children}
        {bodyNodes}
      </body>
    </html>
  );
}
```

The **+html.tsx** file is only used by the server renderer and never by client code. This means:

-   It will be run by `expo-server` during server rendering
-   It is not rehydrated on the client, and should only use [`useServerDocumentContext`](/versions/latest/sdk/router.md#useserverdocumentcontext) React hook
-   You may not import global CSS in `+html.tsx` (use the [Root Layout](/router/basics/navigation-layouts.md#root-layout) for styles)
-   You may not call browser APIs like `window` or `document` in your `+html.tsx`

All `+html.tsx` components are expected to render the `children` prop they receive in their JSX content.

## Metadata

Routes may export a [`generateMetadata`](/versions/latest/sdk/server.md#generatemetadatafunctionrequest-params) function to define per-page metadata such as title, description, and [Open Graph](https://ogp.me/) tags. This function runs on the server before rendering begins, and its result is injected into the `<head>` of the HTML document via the `headNodes` provided by [`useServerDocumentContext`](/versions/latest/sdk/router.md#useserverdocumentcontext) in your [Root HTML](/router/web/server-rendering.md#root-html) component.

Export a [`generateMetadata`](/versions/latest/sdk/server.md#generatemetadatafunctionrequest-params) function from your route file and return a [`Metadata`](/versions/latest/sdk/server.md#metadata) object. The function receives the incoming request and route parameters, which you can use to generate metadata dynamically:

```tsx
import { Text } from 'react-native';
import { useLocalSearchParams } from 'expo-router';
import type { GenerateMetadataFunction } from 'expo-router/server';

export const generateMetadata: GenerateMetadataFunction = async (request, params) => {
  const response = await fetch(`https://api.example.com/posts/${params.id}`);
  const post = await response.json();

  return {
    title: post.title,
    description: post.excerpt,
    openGraph: {
      title: post.title,
      description: post.excerpt,
      images: post.coverImage,
    },
  };
};

export default function BlogPost() {
  const { id } = useLocalSearchParams();
  return <Text>Post {id}</Text>;
}
```

The `generateMetadata` function executes on the server and is stripped from the client bundle, similar to [data loaders](/router/web/data-loaders.md). For a full list of supported metadata fields, see the [`Metadata`](/versions/latest/sdk/server.md#metadata) type in the `expo-server` API reference.

### Using `<Head>` with server rendering

You can also use the `<Head>` component from `expo-router/head` to add `<meta>` tags. Both approaches can co-exist in the same route. However, `generateMetadata` is the recommended approach for server rendering because it resolves metadata before the HTML stream begins, ensuring that `<meta>` tags are included in the earliest bytes of the response. `<Head>` can be used to update `<meta>` tags dynamically after the app has hydrated.

## Deployment

Server-side rendering requires a runtime server to render pages on each request. Server-side rendered Expo apps **cannot** be deployed to static hosting services like GitHub Pages.

### Supported platforms

| Platform | Adapter |
| --- | --- |
| [EAS Hosting](/eas/hosting/introduction.md) | Built-in |
| Node.js/Express | `expo-server/adapter/express` |
| Cloudflare Workers | `expo-server/adapter/workerd` |
| Vercel Edge Functions | `expo-server/adapter/vercel` |
| Netlify Edge Functions | `expo-server/adapter/netlify` |
| Bun | `expo-server/adapter/bun` |

#### Example: Deployment with EAS Hosting

EAS Hosting supports server rendering out of the box. Export your app and deploy with:

```sh
# npm
npx expo export --platform web
npx eas-cli@latest hosting:deploy dist

# yarn
yarn expo export --platform web
yarn dlx eas-cli@latest hosting:deploy dist

# pnpm
pnpm expo export --platform web
pnpm dlx eas-cli@latest hosting:deploy dist

# bun
bun expo export --platform web
bunx eas-cli@latest hosting:deploy dist
```

## Comparison with static rendering

| Feature |  | Static rendering | Server rendering |
| --- | --- | --- | --- |
| HTML generation |  | Build time | Request time |
| HTML delivery |  | Complete document | Streamed progressively |
| Configuration |  | `web.output: 'static'` | `web.output: 'server'` |
| Dynamic routes |  | Requires [`generateStaticParams`](/router/web/static-rendering.md#generatestaticparams) | Works automatically |
| Metadata |  | [`<Head>`](/router/web/server-rendering.md#using-head-with-server-rendering) component | [`generateMetadata`](/router/web/server-rendering.md#metadata) |
| Server required |  | ✗ | ✓ |
| Time to First Byte |  | Fastest (cached) | Slower (rendered per request) |
| Hosting |  | Any static host | Server runtime required |

## Common questions

#### Can I use data loaders with server rendering?

Yes. Server rendering works with [data loaders](/router/web/data-loaders.md) to fetch data on the server before rendering.

#### Can I mix server and static rendering?

Currently, Expo Router does not support mixing server and static rendering in the same project. Choose a single output mode based on your requirements.

#### How do I cache server-rendered responses?

Caching is handled at the server or CDN level. Configure your deployment platform to cache responses based on URL patterns or cache headers.

#### Does server rendering work with API routes?

Yes. [API routes](/router/web/api-routes.md) work independently of the rendering mode. They are always executed on the server.
