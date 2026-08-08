# LangGraph + assistant-ui
URL: /docs/cloud/langgraph

Integrate cloud persistence and thread management with LangGraph Cloud.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## Overview

This guide shows how to integrate Assistant Cloud with [LangGraph Cloud](https://docs.langchain.com/langsmith/deploy-to-cloud-overview) using assistant-ui's runtime system and pre-built UI components.

## Prerequisites

> [!info]
>
> You need an assistant-cloud account to follow this guide. [Sign up here](https://cloud.assistant-ui.com/) to get started.

## Setup Guide

1. ### Create a Cloud Project

   Create a new project in the [assistant-cloud dashboard](https://cloud.assistant-ui.com/) and from the settings page, copy:

   - **Frontend API URL**: `https://proj-[ID].assistant-api.com`
   - **API Key**: For server-side operations

2. ### Configure Environment Variables

   Add the following environment variables to your project:

   **React**

   ```
   # Frontend API URL from your cloud project settings
   NEXT_PUBLIC_ASSISTANT_BASE_URL=https://proj-[YOUR-ID].assistant-api.com

   # API key for server-side operations
   ASSISTANT_API_KEY=your-api-key-here
   ```

3. ### Install Dependencies

   Install the required packages:

   **React**

   ```bash
   npm install @assistant-ui/react @assistant-ui/react-langgraph
   ```

4. ### Create the Runtime Provider

   Create a runtime provider that integrates LangGraph with assistant-cloud. Choose between anonymous mode for demos/prototypes or authenticated mode for production:

   **React**

   Choose one:

   **Anonymous**

   ```
   "use client";

   import {
     AssistantCloud,
     AssistantRuntimeProvider,
   } from "@assistant-ui/react";
   import { useLangGraphRuntime } from "@assistant-ui/react-langgraph";
   import { createThread, deleteThread, getThreadState, sendMessage } from "@/lib/chatApi";
   import { LangChainMessage } from "@assistant-ui/react-langgraph";
   import { useMemo } from "react";

   export function MyRuntimeProvider({
     children,
   }: Readonly<{
     children: React.ReactNode;
   }>) {
     const cloud = useMemo(
       () =>
         new AssistantCloud({
           baseUrl: process.env.NEXT_PUBLIC_ASSISTANT_BASE_URL!,
           anonymous: true, // Creates browser session-based user ID
         }),
       [],
     );

     const runtime = useLangGraphRuntime({
       cloud,
       stream: async function* (messages, { initialize }) {
         const { externalId } = await initialize();
         if (!externalId) throw new Error("Thread not found");

         return sendMessage({
           threadId: externalId,
           messages,
         });
       },
       create: async () => {
         const { thread_id } = await createThread();
         return { externalId: thread_id };
       },
       load: async (externalId) => {
         const state = await getThreadState(externalId);
         return {
           messages:
             (state.values as { messages?: LangChainMessage[] }).messages ?? [],
         };
       },
       delete: async (externalId) => {
         await deleteThread(externalId);
       },
     });

     return (
       <AssistantRuntimeProvider runtime={runtime}>
         {children}
       </AssistantRuntimeProvider>
     );
   }
   ```

   **Authenticated (Clerk)**

   ```
   "use client";

   import {
     AssistantCloud,
     AssistantRuntimeProvider,
   } from "@assistant-ui/react";
   import { useLangGraphRuntime } from "@assistant-ui/react-langgraph";
   import { createThread, deleteThread, getThreadState, sendMessage } from "@/lib/chatApi";
   import { LangChainMessage } from "@assistant-ui/react-langgraph";
   import { useAuth } from "@clerk/nextjs";
   import { useMemo } from "react";

   export function MyRuntimeProvider({
     children,
   }: Readonly<{
     children: React.ReactNode;
   }>) {
     const { getToken } = useAuth();

     const cloud = useMemo(
       () =>
         new AssistantCloud({
           baseUrl: process.env.NEXT_PUBLIC_ASSISTANT_BASE_URL!,
           authToken: async () => getToken({ template: "assistant-ui" }),
         }),
       [getToken],
     );

     const runtime = useLangGraphRuntime({
       cloud,
       stream: async function* (messages, { initialize }) {
         const { externalId } = await initialize();
         if (!externalId) throw new Error("Thread not found");

         return sendMessage({
           threadId: externalId,
           messages,
         });
       },
       create: async () => {
         const { thread_id } = await createThread();
         return { externalId: thread_id };
       },
       load: async (externalId) => {
         const state = await getThreadState(externalId);
         return {
           messages:
             (state.values as { messages?: LangChainMessage[] }).messages ?? [],
         };
       },
       delete: async (externalId) => {
         await deleteThread(externalId);
       },
     });

     return (
       <AssistantRuntimeProvider runtime={runtime}>
         {children}
       </AssistantRuntimeProvider>
     );
   }
   ```

   > [!info]
   >
   > For Clerk authentication, configure the `"assistant-ui"` token template in your Clerk dashboard.

   > [!info]
   >
   > The `useLangGraphRuntime` hook accepts `cloud`, `create`, `load`, and `delete` parameters for simplified thread management. The runtime handles the thread lifecycle internally.
   >
   > - **`create`**: Called when creating a new thread. Returns `{ externalId }` with your backend's thread ID.
   > - **`load`**: Called when switching to an existing thread. Returns the thread's messages (and optionally interrupts).
   > - **`delete`**: Called when deleting a thread. Receives the thread's `externalId`. When provided, users can delete threads from the thread list UI.

5. ### Add Thread UI Components

   Install the thread list component:

   **React**

   Choose one:

   **assistant-ui**

   ```
   npx assistant-ui@latest add thread-list
   ```

   **shadcn (namespace)**

   ```
   npx shadcn@latest add @assistant-ui/thread-list
   ```

   The `@assistant-ui` namespace resolves through the registry entry configured in [Manual Setup](/docs/installation#manual-setup).

   **shadcn**

   ```
   npx shadcn@latest add https://r.assistant-ui.com/thread-list.json
   ```

   Then add it to your application layout:

   ```
   import { Thread } from "@/components/assistant-ui/thread";
   import { ThreadList } from "@/components/assistant-ui/thread-list";

   export default function ChatPage() {
     return (
       <div className="grid h-dvh grid-cols-[250px_1fr] gap-x-2">
         <ThreadList />
         <Thread />
       </div>
     );
   }
   ```

## Authentication

The React runtime-provider tab shows two authentication modes:

- **Anonymous**: Suitable for React demos and prototypes. Creates a browser session-based user ID.
- **Authenticated**: For production use with user accounts. The authenticated example uses [Clerk](https://clerk.com/), but you can integrate any auth provider.

The React Native and React Ink tabs use explicit `authToken` callbacks, which is the recommended shape outside browser-session-based demos.

For other authentication providers or custom implementations, see the [Cloud Authorization](/docs/cloud/authorization) guide.

## Next Steps

- Learn about [LangGraph runtime setup](/docs/runtimes/langgraph/overview) for your application
- Explore [ThreadListRuntime](/docs/api-reference/runtimes/thread-list-runtime) for advanced thread management
- Check out the [LangGraph example](https://github.com/assistant-ui/assistant-ui/tree/main/examples/with-langgraph) on GitHub