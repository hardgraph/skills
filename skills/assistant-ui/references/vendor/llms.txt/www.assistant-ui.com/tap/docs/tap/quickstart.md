# Quickstart
URL: /tap/docs/tap/quickstart

Install tap and build your first Resource.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## Install

```
npm install @assistant-ui/tap
```

## Define a Resource

Write a `use`-prefixed Hook with the React Hooks you already know, then wrap it with `resource()`:

```
import { resource } from "@assistant-ui/tap";
import { useState } from "react";

const useCounter = () => {
  const [count, setCount] = useState(0);

  return {
    count,
    increment: () => setCount((value) => value + 1),
  };
};

const Counter = resource(useCounter);
```

The Resource returns state and methods instead of JSX.

## Use it in React

`useResource` evaluates a Resource and returns its current value:

```
import { useResource } from "@assistant-ui/tap";

function CounterButton() {
  const counter = useResource(Counter());

  return (
    <button onClick={counter.increment}>
      Count: {counter.count}
    </button>
  );
}
```

The component re-renders when the Resource changes and cleans it up when it unmounts.

## Next steps

- [Resources](/tap/docs/tap/resources) —

  Learn how Resources package state and act as composable configuration.

- [Composition](/tap/docs/tap/composition) —

  Compose individual Resources and dynamic keyed collections.