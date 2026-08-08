---
modificationDate: May 22, 2026
title: Testing configuration for Expo Router
description: Learn how to create integration tests for your app when using Expo Router.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/router/reference/testing/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/router/reference/testing/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Testing](https://docs.expo.dev/router/reference/testing.md) (this page)
- [Troubleshooting](https://docs.expo.dev/router/reference/troubleshooting.md)
- [Reserved paths](https://docs.expo.dev/router/reference/reserved-paths.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Testing configuration for Expo Router

Learn how to create integration tests for your app when using Expo Router.

Expo Router relies on your file system, which can present challenges when setting up mocks for integration tests. Expo Router's submodule, `expo-router/testing-library`, is a set of testing utilities built on top of the popular [`@testing-library/react-native`](https://callstack.github.io/react-native-testing-library/) and allows you to quickly create in-memory Expo Router apps that are pre-configured for testing.

## Configuration

Before you proceed, ensure you have set up `jest-expo` according to the [Unit testing with Jest](/develop/unit-testing.md) and [`@testing-library/react-native`](https://callstack.github.io/react-native-testing-library/docs/start/quick-start) in your project.

> **Note**: When using Expo Router, do not put your test files inside the **app** directory. All files inside your **app** directory must be either routes or layout files. Instead, use the **__tests__** directory or a separate directory. This approach is explained in [Unit testing with Jest](/develop/unit-testing.md#structure-your-tests).

## `renderRouter`

`renderRouter` extends the functionality of [`render`](https://callstack.github.io/react-native-testing-library/docs/api#render) to simplify testing with Expo Router. It returns the same query object as [`render`](https://callstack.github.io/react-native-testing-library/docs/api#render), and is compatible with [`screen`](https://callstack.github.io/react-native-testing-library/docs/api#screen), allowing you to use the standard [query API](https://callstack.github.io/react-native-testing-library/docs/api/queries) to locate components.

`renderRouter` accepts the same [options](https://callstack.github.io/react-native-testing-library/docs/api#render-options) as `render` and introduces an additional option `initialUrl`, which sets an initial route for simulating deep-linking.

### `Inline file system`

`renderRouter(mock: Record<string, ReactComponent>, options: RenderOptions)`

`renderRouter` can provide inline-mocking of a file system by passing an object to this function as the first parameter. The keys of the object are the mock filesystem paths. **Do not use leading relative (`./`) or absolute (`/`) notation when defining these paths and exclude file extension.**

```tsx
import { renderRouter, screen } from 'expo-router/testing-library';
import { View } from 'react-native';

it('my-test', async () => {
  const MockComponent = jest.fn(() => <View />);

  renderRouter(
    {
      index: MockComponent,
      'directory/a': MockComponent,
      '(group)/b': MockComponent,
    },
    {
      initialUrl: '/directory/a',
    }
  );

  expect(screen).toHavePathname('/directory/a');
});
```

### ``Inline file system with `null` components``

`renderRouter(mock: string[], options: RenderOptions)`

Providing an array of strings to `renderRouter` will create an inline mock filesystem with `null` components (`{ default: () => null }`). This is useful for testing scenarios where you do not need to test the output of a route.

```tsx
import { renderRouter, screen } from 'expo-router/testing-library';

it('my-test', async () => {
  renderRouter(['index', 'directory/a', '(group)/b'], {
    initialUrl: '/directory/a',
  });

  expect(screen).toHavePathname('/directory/a');
});
```

### `Path to fixture`

`renderRouter(fixturePath: string, options: RenderOptions)`

`renderRouter` can accept a directory path to mock an existing fixture. Ensure that the provided path is relative to the current test file.

```tsx
import { renderRouter } from 'expo-router/testing-library';
import { View } from 'react-native';

it('my-test', async () => {
  const MockComponent = jest.fn(() => <View />);
  renderRouter('./my-test-fixture');
});
```

### `Path to the fixture with overrides`

`renderRouter({ appDir: string, overrides: Record<string, ReactComponent>}, options: RenderOptions)`

For more intricate testing scenarios, `renderRouter` can leverage both directory path and inline-mocking methods simultaneously. The `appDir` parameter takes a string representing a pathname to a directory. The overrides parameter is an inline mock that can be used to override specific paths within the `appDir`. This combination allows for fine-tuned control over the mock environment.

```tsx
import { renderRouter } from 'expo-router/testing-library';
import { View } from 'react-native';

it('my-test', async () => {
  const MockAuthLayout = jest.fn(() => <View />);
  renderRouter({
    appDir: './my-test-fixture',
    overrides: {
      'directory/(auth)/_layout': MockAuthLayout,
    },
  });
});
```

## Jest matchers

The following matches have been added to `expect` and can be used to assert values on `screen`.

### `toHavePathname()`

Assert the current pathname against a given string. The matcher uses the value of the [`usePathname`](/versions/latest/sdk/router.md#usepathname) hook on the current `screen`.

```tsx
expect(screen).toHavePathname('/my-router');
```

### `toHavePathnameWithParams()`

Assert the current pathname, including URL parameters, against a given string. This is useful to assert the appearance of URL in a web browser.

```tsx
expect(screen).toHavePathnameWithParams('/my-router?hello=world');
```

### `toHaveSegments()`

Assert the current segments against an array of strings. The matcher uses the value of the [`useSegments`](/versions/latest/sdk/router.md#usesegments) hook on the current `screen`.

```tsx
expect(screen).toHaveSegments(['[id]']);
```

### `useLocalSearchParams()`

Assert the current local URL parameters against an object. The matcher uses the value of the [`useLocalSearchParams`](/versions/latest/sdk/router.md#uselocalsearchparams) hook on the current `screen`.

```tsx
expect(screen).useLocalSearchParams({ first: 'abc' });
```

### `useGlobalSearchParams()`

Assert the current global URL parameters against an object. The matcher uses the value of the [`useGlobalSearchParams`](/versions/latest/sdk/router.md#useglobalsearchparams) hook on the current `screen`.

```tsx
expect(screen).useGlobalSearchParams({ first: 'abc' });
```

### `toHaveRouterState()`

An advanced matcher that asserts the current router state against an object.

```tsx
expect(screen).toHaveRouterState({
  routes: [{ name: 'index', path: '/' }],
});
```
