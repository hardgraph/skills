# Quickstart
URL: /docs/runtimes/eve/quickstart

Template, Eve CLI, and manual setup paths to a working Eve agent chat in assistant-ui.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Three paths to a running Eve-powered assistant-ui app. The template is fastest, `eve add` drops assistant-ui into an Eve app you already have, and the manual path is what you adapt when integrating into an existing Next.js project.

## From the template

**React**

```
npx create-assistant-ui@latest -t eve my-app
cd my-app
```

Set a model credential:

```
AI_GATEWAY_API_KEY=your-api-key
```

```
npm run dev
```

Open <http://localhost:3000> and send a message. The template includes:

- `agent/agent.ts` for Eve runtime config.
- `agent/instructions.md` for the always-on system prompt.
- `next.config.ts` with `withEve(withAui(nextConfig))`.
- `app/page.tsx` with `useEveAgentRuntime()`.

## With the Eve CLI

For an Eve app that already exists, register the assistant-ui registry once and install the chat page from it:

```
eve registry add @assistant-ui=https://r.assistant-ui.com/{name}.json
eve add @assistant-ui/eve-chat --overwrite
```

`eve registry add` records the namespace in your `package.json`. `eve add` then installs `@assistant-ui/react` and `@assistant-ui/eve`, writes `app/page.tsx`, and writes the thread component together with everything it imports into `components/assistant-ui` and `components/ui`. It expects an Eve app that carries the Next.js Web Chat scaffold, because the installed files import `@/lib/utils` through the `@/*` alias that `eve init --channel-web-nextjs` sets up.

`--overwrite` is global rather than per file. It is what lets the item replace the scaffold's own `app/page.tsx`, and it also replaces any `components/assistant-ui` or `components/ui` file already in the project, including `avatar`, `button`, `collapsible`, `dialog`, and `tooltip`, so install on a clean tree and read the diff before keeping it. A plain `eve add` skips every pre-existing file instead, which leaves `app/page.tsx` untouched. Inspect an item before installing it with `eve registry view @assistant-ui/eve-chat`, and browse the rest of the registry with `eve registry list --registry @assistant-ui`.

That namespace serves the Radix flavor, which matches the shadcn components in Eve's Next.js scaffold. Point it at `https://r.assistant-ui.com/base/{name}.json` instead for a Base UI project.

The Eve CLI writes registry files without editing CSS, so add the styles the reasoning and tool components animate with to `app/globals.css` yourself:

```
@import "tw-shimmer";

@custom-variant data-open (&:where([data-state="open"], [data-open]:not([data-open="false"])));
@custom-variant data-closed (&:where([data-state="closed"], [data-closed]:not([data-closed="false"])));

@theme inline {
  @keyframes collapsible-down {
    from {
      height: 0;
    }
    to {
      height: var(
        --radix-collapsible-content-height,
        var(--collapsible-panel-height, auto)
      );
    }
  }
  @keyframes collapsible-up {
    from {
      height: var(
        --radix-collapsible-content-height,
        var(--collapsible-panel-height, auto)
      );
    }
    to {
      height: 0;
    }
  }
}
```

## Manual setup in an existing app

1. ### Install dependencies

   ```bash
   npm install @assistant-ui/react @assistant-ui/eve eve
   ```

2. ### Mount Eve in Next.js

   ```
   import { withAui } from "@assistant-ui/next";
   import type { NextConfig } from "next";
   import { withEve } from "eve/next";

   const nextConfig: NextConfig = {};

   export default withEve(withAui(nextConfig));
   ```

3. ### Add an Eve agent

   ```
   import { defineAgent } from "eve";

   export default defineAgent({
     model: "anthropic/claude-sonnet-4.6",
   });
   ```

   ```
   You are a concise assistant. Use tools when they are available.
   ```

4. ### Create the runtime

   ```
   "use client";

   import { Thread } from "@/components/assistant-ui/thread";
   import { useEveAgentRuntime } from "@assistant-ui/eve";
   import { AssistantRuntimeProvider } from "@assistant-ui/react";

   export default function Home() {
     const runtime = useEveAgentRuntime();

     return (
       <AssistantRuntimeProvider runtime={runtime}>
         <Thread />
       </AssistantRuntimeProvider>
     );
   }
   ```

## Production auth

The default Eve channel is convenient for local development. For production browser users, define `agent/channels/eve.ts` and replace the default auth policy with your app's auth.

```
import { localDev, vercelOidc } from "eve/channels/auth";
import { eveChannel } from "eve/channels/eve";

export default eveChannel({
  auth: [localDev(), vercelOidc()],
});
```

That example keeps the development defaults. Swap in your Clerk, Auth.js, OIDC, or JWT verification before going live.

## Next

- [Eve overview](/docs/runtimes/eve/overview) — Architecture, requirements, and runtime behavior.
- [API reference](/docs/api-reference/integrations/eve) — useEveAgentRuntime and message conversion helpers.