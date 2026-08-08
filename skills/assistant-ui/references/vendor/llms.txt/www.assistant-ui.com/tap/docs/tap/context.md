# Context
URL: /tap/docs/tap/context

Pass values through resource boundaries without prop drilling.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

> [!info]
>
> Tap context is supported as of `@assistant-ui/tap` 0.9 and is no longer deprecated.

Context lets you pass values through the resource tree without threading props through every level, the same idea as React's `createContext` / `useContext`.

## createContext

Create a context with a default value.

```
import { createContext } from "react";

const ThemeContext = createContext("light");
```

## Reading context

Read the current value of a context inside a resource with `use` imported from `"react"`.

```
import { use } from "react";

const useButton = () => {
  const theme = use(ThemeContext);
  // ...
};

const Button = resource(useButton);
```

## useContextProvider

Provide a context value to all resources rendered within the callback.

```
import { useContextProvider, useResource } from "@assistant-ui/tap";

const useApp = () => {
  const child = useContextProvider(ThemeContext, "dark", () => {
    return useResource(Button());
  });

  return child;
};

const App = resource(useApp);
```

Any resource rendered inside the callback (including deeply nested children) reads `"dark"` from `ThemeContext`.