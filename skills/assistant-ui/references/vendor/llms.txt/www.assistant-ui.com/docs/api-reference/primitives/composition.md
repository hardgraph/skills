# Composition
URL: /docs/api-reference/primitives/composition

How to compose primitives with custom components using asChild.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

`assistant-ui` primitives are composable. This means that all props are forwarded, classes are merged, and event handlers are chained.

Most primitives come with a default HTML element (usually `div` or `button`). If you already have a custom component, you can use the `asChild` prop to replace it:

```
// use the primitive's <button> element
<Composer.Send>Send</Composer.Send>;

// use your own <Button> component
<Composer.Send asChild>
  <Button>Send</Button>
</Composer.Send>;
```

Learn more on [Radix's composition guide](https://www.radix-ui.com/primitives/docs/guides/composition).