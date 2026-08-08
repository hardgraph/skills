# Quickstart
URL: /docs/runtimes/langgraph/quickstart

From-template and manual setup paths to a working LangGraph chat.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Two paths to a running chat against a LangGraph Cloud server. The template is fastest; the manual path is what you adapt when integrating into an existing project.

## From the template

**React**

```
npx create-assistant-ui@latest -t langchain my-app
cd my-app
```

Set environment variables:

```
# LANGCHAIN_API_KEY=your_api_key       # production
# LANGGRAPH_API_URL=your_api_url       # production
NEXT_PUBLIC_LANGGRAPH_API_URL=your_api_url           # development (no API key required)
NEXT_PUBLIC_LANGGRAPH_ASSISTANT_ID=your_graph_id
```

```
npm run dev
```

Skip ahead to [streaming](/docs/runtimes/langgraph/streaming) to start adding features.

## Manual setup in an existing project

1. ### Install dependencies

   **React**

   ```bash
   npm install @assistant-ui/react @assistant-ui/react-langgraph @langchain/langgraph-sdk
   ```

2. ### Create the LangGraph client helper

   **React**

   ```
   import { Client } from "@langchain/langgraph-sdk";

   export const createClient = () => {
     const apiUrl =
       process.env["NEXT_PUBLIC_LANGGRAPH_API_URL"] ||
       (typeof window !== "undefined"
         ? new URL("/api", window.location.href).href
         : "/api");
     return new Client({ apiUrl });
   };
   ```

3. ### Build the assistant component

   **React**

   ```
   "use client";

   import { useMemo } from "react";
   import { Thread } from "@/components/assistant-ui/thread";
   import { AssistantRuntimeProvider } from "@assistant-ui/react";
   import {
     unstable_createLangGraphStream,
     useLangGraphRuntime,
     type LangChainMessage,
   } from "@assistant-ui/react-langgraph";

   import { createClient } from "@/lib/chatApi";

   const ASSISTANT_ID = process.env["NEXT_PUBLIC_LANGGRAPH_ASSISTANT_ID"]!;

   export function MyAssistant() {
     const client = useMemo(() => createClient(), []);
     const stream = useMemo(
       () =>
         unstable_createLangGraphStream({
           client,
           assistantId: ASSISTANT_ID,
         }),
       [client],
     );

     const runtime = useLangGraphRuntime({
       unstable_allowCancellation: true,
       stream,
       create: async () => {
         const { thread_id } = await client.threads.create();
         return { externalId: thread_id };
       },
       load: async (externalId) => {
         const state = await client.threads.getState<{
           messages: LangChainMessage[];
         }>(externalId);
         return {
           messages: state.values.messages,
           interrupts: state.tasks[0]?.interrupts,
         };
       },
     });

     return (
       <AssistantRuntimeProvider runtime={runtime}>
         <Thread />
       </AssistantRuntimeProvider>
     );
   }
   ```

4. ### Mount the component

   **React**

   ```
   import { MyAssistant } from "@/components/MyAssistant";

   export default function Home() {
     return (
       <main className="h-dvh">
         <MyAssistant />
       </main>
     );
   }
   ```

5. ### Set environment variables

   **React**

   Use the same `.env.local` shape as the template path [above](#from-the-template).

6. ### Set up UI components

   **React**

   Follow the [UI Components guide](/docs/ui/thread) to wire up the Thread, composer, and supporting primitives.

## Production proxy backend

For development, the client above hits LangGraph Cloud directly using `NEXT_PUBLIC_LANGGRAPH_API_URL`. For production, proxy through your own backend so your API key never reaches the client. Limit the proxy to the endpoints you actually need.

```
import { NextRequest, NextResponse } from "next/server";

export const runtime = "edge";

function getCorsHeaders() {
  return {
    "Access-Control-Allow-Origin": "*",
    "Access-Control-Allow-Methods": "GET, POST, PUT, PATCH, DELETE, OPTIONS",
    "Access-Control-Allow-Headers": "*",
  };
}

async function handleRequest(req: NextRequest, method: string) {
  try {
    const path = req.nextUrl.pathname.replace(/^\/?api\//, "");
    const url = new URL(req.url);
    const searchParams = new URLSearchParams(url.search);
    searchParams.delete("_path");
    searchParams.delete("nxtP_path");
    const queryString = searchParams.toString()
      ? `?${searchParams.toString()}`
      : "";

    const options: RequestInit = {
      method,
      headers: { "x-api-key": process.env["LANGCHAIN_API_KEY"] ?? "" },
      signal: req.signal,
    };
    if (["POST", "PUT", "PATCH"].includes(method)) {
      options.body = await req.text();
    }

    const res = await fetch(
      `${process.env["LANGGRAPH_API_URL"]}/${path}${queryString}`,
      options,
    );
    const headers = new Headers(res.headers);
    headers.delete("content-encoding");
    headers.delete("content-length");
    headers.delete("transfer-encoding");
    for (const [key, value] of Object.entries(getCorsHeaders())) {
      headers.set(key, value);
    }
    return new NextResponse(res.body, {
      status: res.status,
      statusText: res.statusText,
      headers,
    });
  } catch (e: unknown) {
    if (e instanceof Error) {
      const typedError = e as Error & { status?: number };
      return NextResponse.json(
        { error: typedError.message },
        { status: typedError.status ?? 500 },
      );
    }
    return NextResponse.json({ error: "Unknown error" }, { status: 500 });
  }
}

export const GET = (req: NextRequest) => handleRequest(req, "GET");
export const POST = (req: NextRequest) => handleRequest(req, "POST");
export const PUT = (req: NextRequest) => handleRequest(req, "PUT");
export const PATCH = (req: NextRequest) => handleRequest(req, "PATCH");
export const DELETE = (req: NextRequest) => handleRequest(req, "DELETE");
export const OPTIONS = () =>
  new NextResponse(null, { status: 204, headers: getCorsHeaders() });
```

With this route in place, drop `NEXT_PUBLIC_LANGGRAPH_API_URL` from production env vars; the client helper falls back to the same-origin `/api` path. Set `LANGCHAIN_API_KEY` and `LANGGRAPH_API_URL` server-side instead.

## Next

- [Streaming](/docs/runtimes/langgraph/streaming) — Event handlers, message metadata, message conversion.
- [Generative UI](/docs/runtimes/langgraph/generative-ui) — Structured UI components emitted by your graph.
- [Interrupts](/docs/runtimes/langgraph/interrupts) — Interrupt persistence and checkpoint-based message editing.