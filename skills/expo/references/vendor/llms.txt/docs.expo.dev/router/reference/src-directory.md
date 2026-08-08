---
modificationDate: July 28, 2026
title: Top-level src directory
description: Learn how to use a top-level src directory in your Expo Router project.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/router/reference/src-directory/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/router/reference/src-directory/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Top-level src directory](https://docs.expo.dev/router/reference/src-directory.md) (this page)
- [Testing](https://docs.expo.dev/router/reference/testing.md)
- [Troubleshooting](https://docs.expo.dev/router/reference/troubleshooting.md)
- [Reserved paths](https://docs.expo.dev/router/reference/reserved-paths.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Top-level src directory

Learn how to use a top-level src directory in your Expo Router project.

Projects created with the [default template](/get-started/create-a-project.md) on SDK 55 and later already include a top-level **src** directory that contains the **app**, **components**, **constants**, and **hooks** directories. No extra configuration is needed.

If you are using a [custom template](/more/create-expo.md#--template) or an existing project that doesn't include a **src** directory, follow the steps below to set it up.

## Using a top-level src directory

Move your **app** directory to **src/app**.

`src`

 `app`

  `_layout.tsx`

  `index.tsx`

 `components`

  `button.tsx`

`package.json`

Update [TypeScript path aliases](/guides/typescript.md#path-aliases-optional) in the **tsconfig.json** file to point to the **src** directory instead of the root directory. If you use the default `@/*` alias, set it to **./src/\***:

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

This keeps `@/` imports working after moving your app directory into **src**.

Restart your development server.

```sh
# npm
npx expo start
npx expo export

# yarn
yarn expo start
yarn expo export

# pnpm
pnpm expo start
pnpm expo export

# bun
bun expo start
bun expo export
```

### Notes

-   The config files (**app.config.ts**, **app.json**, **package.json**, **metro.config.js**, **tsconfig.json**) should remain in the root directory.
-   The **src/app** directory takes higher precedence than the root **app** directory. Only the **src/app** directory will be used if you have both.
-   The **public** directory should remain in the root directory.
-   Static rendering will automatically use the **src/app** directory if it exists.
-   You may consider updating any [type aliases](/guides/typescript.md#path-aliases-optional) to point to the **src** directory instead of the root directory.

## Custom directory

> Changing the default root directory is highly discouraged. We will not accept bug reports regarding projects with custom root directories.

You can dangerously customize the root directory using the Expo Router Config Plugin. The following will change the root directory to **src/routes**, relative to the project root.

```json
{
  "plugins": [
    [
      "expo-router",
      {
        "root": "./src/routes"
      }
    ]
  ]
}
```

This may lead to unexpected behavior. Many tools assume the root directory to be either **app** or **src/app**. Only tools in the exact version of Expo CLI will respect the config plugin.
