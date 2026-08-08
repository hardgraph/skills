# Introduction
URL: /tap/docs/overview/introduction

State management based on React Hooks.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

**tap is a state management library based on React Hooks.**

It lets you build and manage your application state with the APIs and mental model you already use in React, organized around one new primitive: the **Resource**.

A Resource does for state what a component does for UI. It turns stateful behavior into a reusable, composable building block—but returns state and methods instead of JSX.

> **Resources organize like components, but execute like Hooks.**

## Resources are the unit of state composition

### 1. Write a Hook

```jsx
import { useState } from "react";

const useCounter = () => {
  const [count, setCount] = useState(0);
  const increment = () => setCount((value) => value + 1);
  return { count, increment };
};
```

### 2. Create a Resource

```jsx
import { useState } from "react";
import { resource } from "@assistant-ui/tap";

const useCounter = () => {
  const [count, setCount] = useState(0);
  const increment = () => setCount((value) => value + 1);
  return { count, increment };
};

const Counter = resource(useCounter);
```

### 3. Use it in React

```jsx
import { useState } from "react";
import { resource, useResource } from "@assistant-ui/tap";

const useCounter = () => {
  const [count, setCount] = useState(0);
  const increment = () => setCount((value) => value + 1);
  return { count, increment };
};

const Counter = resource(useCounter);

function CounterButton() {
  const { count, increment } = useResource(Counter());

  return <button onClick={increment}>{count}</button>;
}
```

### 4. Swap the implementation

```jsx
import { useState } from "react";
import { resource, useResource } from "@assistant-ui/tap";
import { PersistentCounter } from "./persistent-counter";

const useCounter = () => {
  const [count, setCount] = useState(0);
  const increment = () => setCount((value) => value + 1);
  return { count, increment };
};

const Counter = resource(useCounter);

function CounterButton({ id }) {
  const { count, increment } = useResource(
    id ? PersistentCounter(id) : Counter(),
  );

  return <button onClick={increment}>{count}</button>;
}
```

### 5. Accept an implementation

```jsx
import { useState } from "react";
import { resource, useResource } from "@assistant-ui/tap";
import { PersistentCounter } from "./persistent-counter";

const useCounter = () => {
  const [count, setCount] = useState(0);
  const increment = () => setCount((value) => value + 1);
  return { count, increment };
};

const Counter = resource(useCounter);

function CounterButton({ counter = Counter() }) {
  const { count, increment } = useResource(counter);

  return <button onClick={increment}>{count}</button>;
}

function App() {
  return <CounterButton counter={PersistentCounter("main")} />;
}
```

### 6. Render a dynamic collection

```jsx
import { useResources, withKey } from "@assistant-ui/tap";
import { Counter } from "./counter";

function CounterList({ ids }) {
  const counters = useResources(
    ids.map((id) => withKey(id, Counter())),
  );

  return ids.map((id, index) => {
    const { count, increment } = counters[index];

    return (
      <button key={id} onClick={increment}>
        {id}: {count}
      </button>
    );
  });
}
```

### 7. Run hooks outside React

```js
import { createTapRoot, flushTapSync } from "@assistant-ui/tap";
import { useCounter } from "./counter";

const root = createTapRoot(function CounterRoot() {
  return useCounter();
});

const unsubscribe = root.subscribe(() => {
  console.log(root.getValue().count); // 1
});

flushTapSync(() => root.getValue().increment());

unsubscribe();
root.unmount();
```

`resource()` turns a Hook into a Resource. Use [`useResource`](/tap/docs/tap/resources#hosting-a-resource) to compose one Resource or [`useResources`](/tap/docs/tap/composition#useresources) to compose a keyed collection.

Nested Resources behave as if their Hook logic were inlined, so effects follow declaration order—not component-tree order. See [Differences from React](/tap/docs/tap/differences-from-react) for details.

## Next steps

- [Resources](/tap/docs/tap/resources) —

  Package Hooks into reusable, configurable state.

- [Composition](/tap/docs/tap/composition) —

  Build Resource graphs and dynamic keyed collections.

- [API Reference](/tap/docs/tap/api-reference) —

  Every export, in one place.