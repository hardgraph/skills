---
modificationDate: February 26, 2026
title: Reserved paths
description: URL paths reserved by Metro and Expo Router that you should avoid using for routes or static files.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/router/reference/reserved-paths/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/router/reference/reserved-paths/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Expo Router > Reference
Pages in this section:
- [Error handling and loading states](https://docs.expo.dev/router/error-handling.md)
- [URL parameters](https://docs.expo.dev/router/reference/url-parameters.md)
- [Color](https://docs.expo.dev/router/reference/color.md)
- [Sitemap](https://docs.expo.dev/router/reference/sitemap.md)
- [Redirects](https://docs.expo.dev/router/reference/redirects.md)
- [Link preview](https://docs.expo.dev/router/reference/link-preview.md)
- [Typed routes](https://docs.expo.dev/router/reference/typed-routes.md)
- [Screen tracking for analytics](https://docs.expo.dev/router/reference/screen-tracking.md)
- [Top-level src directory](https://docs.expo.dev/router/reference/src-directory.md)
- [Testing](https://docs.expo.dev/router/reference/testing.md)
- [Troubleshooting](https://docs.expo.dev/router/reference/troubleshooting.md)
- [Reserved paths](https://docs.expo.dev/router/reference/reserved-paths.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Reserved paths

URL paths reserved by Metro and Expo Router that you should avoid using for routes or static files.

If you create a route or place static files at certain URL paths, Metro or Expo Router will intercept the request instead of serving your content. Depending on the path, this can result in a "404 Asset not found" error or your page being silently replaced by an internal dev server response.

## `/assets/*`

Metro serves all bundled assets (images, fonts, and other files) at this path. If you create a route at **app/assets.tsx** or a directory at **public/assets/**, Metro intercepts the request and your content is never reached.

This applies to both top-level routes and static files:

`app`

 `assets.tsx``Conflicts with Metro`

 `assets`

  `index.tsx``Conflicts with Metro`

`public`

 `assets`

  `logo.png``Conflicts with Metro`

Rename your route or directory to avoid the conflict:

`app`

 `media.tsx``Works`

`public`

 `images`

  `logo.png``Works`

## `/_expo/*`

Expo Router uses this path for multiple internal middlewares, including dev tools and manifests. Do not create routes or static files under this path.

## `/_flight/*`

React Server Components use this path internally. Do not create routes or static files under this path.

## `/inspector`

React Native uses `/inspector/debug` and `/inspector/network` for the debugger. Avoid creating routes that match `/inspector` or its sub-paths.

## `/expo-dev-plugins/*`

Expo development tool plugins use this path. Do not create routes or static files under this path.

## `/manifest`

The dev server serves the native app manifest at this path. If you create a route at **app/manifest.tsx**, the dev server responds with manifest JSON instead of your page. Your route will appear to silently not load during development.

## `/_sitemap`

Expo Router automatically generates a sitemap route at this path for debugging. If you create a route at **app/_sitemap.tsx**, it will override the built-in sitemap. See [Sitemap](/router/reference/sitemap.md) for more details on this feature.

## `/public/*`

If your project has a **public** directory, the `/public` URL path may conflict with static file serving. Avoid creating a route at **app/public.tsx** or **app/public/index.tsx** since the path is implicitly reserved when the **public** directory exists.

## `/favicon.ico`

Unlike the paths above, `/favicon.ico` is safe to override. Expo CLI serves a default favicon when none is provided. You can replace it by placing a **favicon.ico** file in your **public** directory or by creating an [API route](/router/web/api-routes.md).
