# Custom Scrollbar
URL: /docs/ui/scrollbar

Replace the default scrollbar with a custom Radix UI scroll area.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

\[interactive preview omitted]

If you want to show a custom scrollbar UI of the `ThreadPrimitive.Viewport` in place of the system default, you can integrate `radix-ui`'s Scroll Area. An example implementation of this is [shadcn/ui's Scroll Area](https://ui.shadcn.com/docs/components/scroll-area).

1. ### Add shadcn Scroll Area

   ```
   npx shadcn@latest add scroll-area
   ```

2. ### Add Additional Styles

   The Radix UI Viewport component adds an intermediate `<div data-radix-scroll-area-content>` element. Add the following CSS to your `globals.css`:

   ```
   .thread-viewport > [data-radix-scroll-area-content] {
     @apply flex flex-col items-center self-stretch bg-inherit;
   }
   ```

3. ### Integrate with Thread

   - Wrap `ThreadPrimitive.Root` with `<ScrollAreaPrimitive.Root asChild>`
   - Wrap `ThreadPrimitive.Viewport` with `<ScrollAreaPrimitive.Viewport className="thread-viewport" asChild>`
   - Add shadcn's `<ScrollBar />` to `ThreadPrimitive.Root`

   The resulting MyThread component should look like this:

   ```
   import { ScrollArea as ScrollAreaPrimitive } from "radix-ui";
   import { ScrollBar } from "@/components/ui/scroll-area";

   const MyThread: FC = () => {
     return (
       <ScrollAreaPrimitive.Root asChild>
         <ThreadPrimitive.Root className="...">
           <ScrollAreaPrimitive.Viewport className="thread-viewport" asChild>
             <ThreadPrimitive.Viewport className="...">
               ...
             </ThreadPrimitive.Viewport>
           </ScrollAreaPrimitive.Viewport>
           <ScrollBar />
         </ThreadPrimitive.Root>
       </ScrollAreaPrimitive.Root>
     );
   };
   ```

## Related Components

- [Thread](/docs/ui/thread) - The main chat interface where the scrollbar is used