---
modificationDate: April 02, 2026
title: Testing React Server Components
description: Learn about writing unit tests for React Server Components in Expo.
platforms: ['android', 'ios', 'web']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/testing-rsc/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/testing-rsc/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Web
Pages in this section:
- [Develop websites](https://docs.expo.dev/workflow/web.md)
- [Publish websites](https://docs.expo.dev/guides/publishing-websites.md)
- [DOM components](https://docs.expo.dev/guides/dom-components.md)
- [React Server Components](https://docs.expo.dev/guides/server-components.md)
- [Testing RSC](https://docs.expo.dev/guides/testing-rsc.md) (this page)
- [Progressive web apps](https://docs.expo.dev/guides/progressive-web-apps.md)
- [Tailwind CSS](https://docs.expo.dev/guides/tailwind.md)
- [Local HTTPS development](https://docs.expo.dev/guides/local-https-development.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Testing React Server Components

Learn about writing unit tests for React Server Components in Expo.
Android, iOS, Web

> This guide refers to the [experimental](/more/release-statuses.md#experimental) feature React Server Components which is still in development.

React Server Components (RSC) is a new feature in React that allows you to build components that render on the server and can be hydrated on the client. This guide provides details on how to write unit tests for RSC in your project.

## Jest testing

React Server Components run on Node.js. This means Jest on its own can closely emulate the server-side rendering environment, in contrast with client-based tests that require a Jest preset to communicate between Node.js and a web browser.

### Setup

While standard server rendering is web-only, Expo's universal RSC bundles custom server renderers for each platform. This means platform-specific file extensions are supported. For example, when writing Server Components for an iOS app, platform-specific extensions such as **\*.ios.js** and **\*.native.ts** will be resolved.

`jest-expo` provides a couple different presets for testing Server Components:

| Runner | Description |
| --- | --- |
| `jest-expo/rsc/android` | An Android-only runner for RSC. Uses **\*.android.js**, **\*.native.js**, and **\*.js** files. |
| `jest-expo/rsc/ios` | An iOS-only runner for RSC. Uses **\*.ios.js**, **\*.native.js**, and **\*.js** files. |
| `jest-expo/rsc/web` | A web-only runner for RSC. Uses **\*.web.js** and **\*.js** files. |
| `jest-expo/rsc` | A multi-runner that combines the above runners. |

To configure Jest for RSC, create a **jest-rsc.config.js** file in your project's root:

```js
module.exports = require('jest-expo/rsc/jest-preset');
```

Then, you can add a script such as `test:rsc` to your **package.json**:

```json
{
  "scripts": {
    "test:rsc": "jest --config jest-rsc.config.js"
  }
}
```

### Writing tests

Tests should be written in a **__rsc_tests__** directory to prevent Jest from running your client tests on the server.

```tsx
/// <reference types="jest-expo/rsc/expect" />

import { LinearGradient } from 'expo-linear-gradient';

it(`renders to RSC`, async () => {
  const jsx = (
    <LinearGradient
      colors={['cyan', '#ff00ff', 'rgba(0,0,0,0)', 'rgba(0,255,255,0.5)']}
      testID="gradient"
    />
  );

  await expect(jsx).toMatchFlight(`1:I["src/LinearGradient.tsx",[],"LinearGradient"]
0:["$","$L1",null,{"colors":["cyan","#ff00ff","rgba(0,0,0,0)","rgba(0,255,255,0.5)"],"testID":"gradient"},null]`);
});
```

Any code you import in your test files will run in the server environment. You can import server-only modules like `react-server` and `server-only`. This is useful for determining if a library is compatible with RSC.

### Custom expect matchers

`jest-expo` for RSC adds a couple of custom matchers to Jest's `expect`:

-   `toMatchFlight`: Render a JSX element using a pseudo-implementation of the render in Expo CLI and compare to a flight string.
-   `toMatchFlightSnapshot`: Same as `toMatchFlight` but saves the flight string to a snapshot file.

Behind the scenes, these methods handle a part of the framework operation needed to render RSC. The component's render stream is buffered to a string and compared all at once. You can alternatively stream it manually to observe the rendering progress.

If a component fails to render, the matcher will throw an error to fail the test. In practice, the server renderer will generate an `E:` line, which will sent to the client to be thrown locally for the user.

### Running tests

You can run your tests with the `test:rsc` script:

```sh
yarn test:rsc --watch
```

If you're using the multi-runner, you can select a specific project using the `--selectProjects` flag. The following example only runs the web platform:

```sh
yarn test:rsc --watch --selectProjects rsc/web
```

### Environments

In an RSC bundling environment, you can import files like

## Tips

Use the `server-only` and `client-only` modules to assert that a module should not be imported on the client or server:

```js
import 'server-only';
```

RSC supports package exports by default. You can use the `react-server` condition to change what file is imported from a module:

```json
{
  "exports": {
    ".": {
      "react-server": "./index.react-server.js",
      "default": "./index.js"
    }
  }
}
```

When bundling for RSC, all modules are bundled in React Server mode and you can opt out with the `"use client"` directive. When `"use client"` is found, the module becomes an async reference to the client module.

`"use server"` is not the opposite of `"use client"`. It is instead used to define a React Server Functions file.
