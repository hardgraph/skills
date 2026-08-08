# Eve Runtime
URL: /docs/runtimes/eve/overview

Connect an Eve agent to assistant-ui with useEveAgentRuntime, eve/next, durable sessions, streaming messages, and human-in-the-loop approvals.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

`@assistant-ui/eve` integrates assistant-ui with [Eve](https://eve.dev/), Vercel's filesystem-first framework for durable agents. It wraps Eve's `useEveAgent` hook and exposes it as an assistant-ui `ExternalStoreRuntime`, so Eve owns the session stream while assistant-ui renders messages, reasoning, dynamic tool calls, and approval requests.

## When to use it

Pick the Eve runtime when:

- You want your agent implementation to live under `agent/` and be served by `eve/next`.
- You want Eve sessions, continuation tokens, NDJSON streaming, and local development tooling.
- You want assistant-ui to render Eve messages and human-in-the-loop tool approvals without writing a custom runtime adapter.

## Architecture

The Next.js app mounts Eve with `withEve()` and assistant-ui's registry transform with `withAui()`:

```
import { withAui } from "@assistant-ui/next";
import type { NextConfig } from "next";
import { withEve } from "eve/next";

const nextConfig: NextConfig = {};

export default withEve(withAui(nextConfig));
```

On the client, `useEveAgentRuntime()` calls Eve's React hook and converts Eve message parts into assistant-ui thread messages:

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

The `custom` bag of a per-turn `runConfig` is forwarded to Eve as `clientContext`, so `runConfig: { custom: { page: "/pricing" } }` arrives as `clientContext: { page: "/pricing" }`. An empty or absent bag is omitted, and every value must be JSON-serializable.

Eve turns that object into a model context message, which is why the assistant-ui `custom` envelope is unwrapped: the envelope would put a literal `custom` key in the prompt. It is page or client context rather than run configuration, and it cannot select a model or change how the turn executes. Keep secrets, credentials, and personal data out of it, since the model reads it as message text.

## Connector authorization

When Eve pauses for connector authorization, the runtime emits an assistant data part named `authorization`. Its data includes `state`, `name`, and the available display fields such as `displayName`, `description`, `url`, `userCode`, `instructions`, and `expiresAt`. Completed parts may also include `outcome` and `reason`. `url` is present only when the connector supplied an `http(s)` address; the adapter drops other schemes so a renderer can link to it directly.

The Eve template ships this renderer as `components/eve-authorization.tsx` and mounts it next to `<Thread />`. To add it to an existing app, register a data UI for the part and type it with the exported `EveAuthorizationData`:

```
import type { EveAuthorizationData } from "@assistant-ui/eve";
import { makeAssistantDataUI } from "@assistant-ui/react";

export const AuthorizationUI = makeAssistantDataUI<EveAuthorizationData>({
  name: "authorization",
  render: ({ data }) => (
    <div>
      {data.state === "required" ? (
        <>
          <p>{data.instructions ?? `Sign in to ${data.displayName ?? data.name}`}</p>
          {data.userCode && <code>{data.userCode}</code>}
          {data.url && <a href={data.url}>Continue to sign in</a>}
        </>
      ) : (
        <p>{data.outcome ?? "Authorization completed"}</p>
      )}
    </div>
  ),
});
```

Mount `<AuthorizationUI />` inside the `AssistantRuntimeProvider` tree. The standard thread intentionally leaves unmatched data parts unrendered.

## Requirements

- Node.js 24 or higher.
- React 18 or 19.
- An Eve app mounted with `eve/next`.
- A model credential for the model configured in `agent/agent.ts`.

Eve's default gateway model ids route through the [Vercel AI Gateway](https://vercel.com/docs/ai-gateway). Use `AI_GATEWAY_API_KEY` or Vercel OIDC, or configure a direct provider `LanguageModel`. For local development, a direct `LanguageModel` can also run on a ChatGPT Plus or Pro plan without any API key; see [ChatGPT Subscription](/docs/guides/chatgpt-subscription).

## Install

**React**

```bash
npm install @assistant-ui/react @assistant-ui/eve eve
```

## Auth note

Eve's built-in `eve` channel accepts localhost during development and trusted Vercel OIDC callers. It does not automatically admit browser users in production. Before deploying a public app, add `agent/channels/eve.ts` and wire the channel to your application auth.

## Next

- [Quickstart](/docs/runtimes/eve/quickstart) — Scaffold the Eve template or add Eve to an existing assistant-ui app.
- [Eve channel](https://eve.dev/docs/channels/eve) — Routes, auth, session creation, and stream events in the default Eve HTTP channel.