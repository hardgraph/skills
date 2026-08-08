# Installation
URL: /docs/installation

Get assistant-ui running in 5 minutes with npm and your first chat component.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## Quick Start

The fastest way to get started with assistant-ui.

![animated gif showing the steps to create a new project](/_next/static/immutable/media/assistant-ui-starter.1xsy-u7090fnq.gif)

1. ### Initialize assistant-ui

   **Create a new project:**

   ```
   npx assistant-ui@latest create
   ```

   Or start from a template:

   ```
   npx assistant-ui@latest create -t cloud
   ```

   The [CLI reference](/docs/cli#create) lists every template and example, and what each one includes.

   **Add to an existing project:**

   ```
   npx assistant-ui@latest init
   ```

2. ### Add API key

   Create a `.env` file with your API key:

   ```
   OPENAI_API_KEY="sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

   Developing locally with a ChatGPT Plus or Pro plan? You can skip the API key and run on your subscription instead; see [ChatGPT Subscription](/docs/guides/chatgpt-subscription).

3. ### Start the app

   ```
   npm run dev
   ```

## Manual Setup

If you prefer not to use the CLI, you can install components manually.

Add the assistant-ui registry to your `components.json` so the shadcn CLI resolves components for your style. Styles whose name starts with `base-` receive Base UI flavored components; all other styles receive the Radix flavored ones. `npx shadcn init` defaults to Base UI styles (for example `base-nova`), so new projects need this style-aware URL for components that compile against that stack.

```
{
  "registries": {
    "@assistant-ui": "https://r.assistant-ui.com/styles/{style}/{name}.json"
  }
}
```

Existing Radix projects can keep the plain form `https://r.assistant-ui.com/{name}.json` as a fallback (for example `npx shadcn@latest add https://r.assistant-ui.com/thread.json`).

1. ### Add assistant-ui

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/thread @assistant-ui/thread-list
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/thread.json https://r.assistant-ui.com/base/thread-list.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react @assistant-ui/react-markdown class-variance-authority remark-gfm tw-shimmer zustand
   ```

   ```bash
   npx shadcn@latest add button dialog tooltip avatar collapsible input skeleton
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/thread.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/thread.tsx)
   - [components/assistant-ui/thread-list.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/thread-list.tsx)
   - [components/assistant-ui/attachment.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/attachment.tsx)
   - [components/assistant-ui/tooltip-icon-button.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx)
   - [components/assistant-ui/follow-up-suggestions.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/follow-up-suggestions.tsx)
   - [components/assistant-ui/markdown-text.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/markdown-text.tsx)
   - [components/assistant-ui/reasoning.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/reasoning.tsx)
   - [components/assistant-ui/tool-fallback.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tool-fallback.tsx)
   - [components/assistant-ui/tool-group.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tool-group.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/thread.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/thread.tsx \
     -o components/assistant-ui/thread-list.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/thread-list.tsx \
     -o components/assistant-ui/attachment.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/attachment.tsx \
     -o components/assistant-ui/tooltip-icon-button.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx \
     -o components/assistant-ui/follow-up-suggestions.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/follow-up-suggestions.tsx \
     -o components/assistant-ui/markdown-text.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/markdown-text.tsx \
     -o components/assistant-ui/reasoning.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/reasoning.tsx \
     -o components/assistant-ui/tool-fallback.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tool-fallback.tsx \
     -o components/assistant-ui/tool-group.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tool-group.tsx
   ```

2. ### Setup Backend Endpoint

   Install provider SDK:

   Choose one:

   **OpenAI**

   ```bash
   npm install ai @assistant-ui/react-ai-sdk @ai-sdk/openai
   ```

   **Anthropic**

   ```bash
   npm install ai @assistant-ui/react-ai-sdk @ai-sdk/anthropic
   ```

   **Azure**

   ```bash
   npm install ai @assistant-ui/react-ai-sdk @ai-sdk/azure
   ```

   **AWS**

   ```bash
   npm install ai @assistant-ui/react-ai-sdk @ai-sdk/amazon-bedrock
   ```

   **Gemini**

   ```bash
   npm install ai @assistant-ui/react-ai-sdk @ai-sdk/google
   ```

   **GCP**

   ```bash
   npm install ai @assistant-ui/react-ai-sdk @ai-sdk/google-vertex
   ```

   **Groq**

   ```bash
   npm install ai @assistant-ui/react-ai-sdk @ai-sdk/groq
   ```

   **Fireworks**

   ```bash
   npm install ai @assistant-ui/react-ai-sdk @ai-sdk/fireworks
   ```

   **Cohere**

   ```bash
   npm install ai @assistant-ui/react-ai-sdk @ai-sdk/cohere
   ```

   **Ollama**

   ```bash
   npm install ai @assistant-ui/react-ai-sdk ollama-ai-provider-v2
   ```

   **Chrome AI**

   ```bash
   npm install ai @assistant-ui/react-ai-sdk chrome-ai
   ```

   Add an API endpoint:

   Choose one:

   **OpenAI**

   ```
   import { openai } from "@ai-sdk/openai";
   import { frontendTools } from "@assistant-ui/react-ai-sdk";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages, system, tools } = await req.json();
     const result = streamText({
       model: openai("gpt-5.6-luna"),
       system,
       messages: await convertToModelMessages(messages),
       tools: frontendTools(tools),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   **Anthropic**

   ```
   import { anthropic } from "@ai-sdk/anthropic";
   import { frontendTools } from "@assistant-ui/react-ai-sdk";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages, system, tools } = await req.json();
     const result = streamText({
       model: anthropic("claude-sonnet-4-6"),
       system,
       messages: await convertToModelMessages(messages),
       tools: frontendTools(tools),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   **Azure**

   ```
   import { azure } from "@ai-sdk/azure";
   import { frontendTools } from "@assistant-ui/react-ai-sdk";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages, system, tools } = await req.json();
     const result = streamText({
       model: azure("your-deployment-name"),
       system,
       messages: await convertToModelMessages(messages),
       tools: frontendTools(tools),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   **AWS**

   ```
   import { bedrock } from "@ai-sdk/amazon-bedrock";
   import { frontendTools } from "@assistant-ui/react-ai-sdk";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages, system, tools } = await req.json();
     const result = streamText({
       model: bedrock("anthropic.claude-sonnet-4-6-v1:0"),
       system,
       messages: await convertToModelMessages(messages),
       tools: frontendTools(tools),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   **Gemini**

   ```
   import { google } from "@ai-sdk/google";
   import { frontendTools } from "@assistant-ui/react-ai-sdk";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages, system, tools } = await req.json();
     const result = streamText({
       model: google("gemini-2.0-flash"),
       system,
       messages: await convertToModelMessages(messages),
       tools: frontendTools(tools),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   **GCP**

   ```
   import { vertex } from "@ai-sdk/google-vertex";
   import { frontendTools } from "@assistant-ui/react-ai-sdk";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages, system, tools } = await req.json();
     const result = streamText({
       model: vertex("gemini-2.0-flash"),
       system,
       messages: await convertToModelMessages(messages),
       tools: frontendTools(tools),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   **Groq**

   ```
   import { groq } from "@ai-sdk/groq";
   import { frontendTools } from "@assistant-ui/react-ai-sdk";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages, system, tools } = await req.json();
     const result = streamText({
       model: groq("llama-3.3-70b-versatile"),
       system,
       messages: await convertToModelMessages(messages),
       tools: frontendTools(tools),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   **Fireworks**

   ```
   import { fireworks } from "@ai-sdk/fireworks";
   import { frontendTools } from "@assistant-ui/react-ai-sdk";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages, system, tools } = await req.json();
     const result = streamText({
       model: fireworks("accounts/fireworks/models/llama-v3p3-70b-instruct"),
       system,
       messages: await convertToModelMessages(messages),
       tools: frontendTools(tools),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   **Cohere**

   ```
   import { cohere } from "@ai-sdk/cohere";
   import { frontendTools } from "@assistant-ui/react-ai-sdk";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages, system, tools } = await req.json();
     const result = streamText({
       model: cohere("command-r-plus"),
       system,
       messages: await convertToModelMessages(messages),
       tools: frontendTools(tools),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   **Ollama**

   ```
   import { ollama } from "ollama-ai-provider-v2";
   import { frontendTools } from "@assistant-ui/react-ai-sdk";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages, system, tools } = await req.json();
     const result = streamText({
       model: ollama("llama3"),
       system,
       messages: await convertToModelMessages(messages),
       tools: frontendTools(tools),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   **Chrome AI**

   ```
   import { chromeai } from "chrome-ai";
   import { frontendTools } from "@assistant-ui/react-ai-sdk";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages, system, tools } = await req.json();
     const result = streamText({
       model: chromeai(),
       system,
       messages: await convertToModelMessages(messages),
       tools: frontendTools(tools),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   Define environment variables:

   Choose one:

   **OpenAI**

   ```
   OPENAI_API_KEY="sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

   **Anthropic**

   ```
   ANTHROPIC_API_KEY="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

   **Azure**

   ```
   AZURE_RESOURCE_NAME="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   AZURE_API_KEY="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

   **AWS**

   ```
   AWS_ACCESS_KEY_ID="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   AWS_SECRET_ACCESS_KEY="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   AWS_REGION="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

   **Gemini**

   ```
   GOOGLE_GENERATIVE_AI_API_KEY="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

   **GCP**

   ```
   GOOGLE_VERTEX_PROJECT="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   GOOGLE_VERTEX_LOCATION="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   GOOGLE_APPLICATION_CREDENTIALS="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

   **Groq**

   ```
   GROQ_API_KEY="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

   **Fireworks**

   ```
   FIREWORKS_API_KEY="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

   **Cohere**

   ```
   COHERE_API_KEY="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

   **Ollama**

   ```
   <none>
   ```

   **Chrome AI**

   ```
   <none>
   ```

   If you aren't using Next.js, you can also deploy this endpoint to Cloudflare Workers, or any other serverless platform.

3. ### Use it in your app

   Choose one:

   **Thread**

   ```
   import { AssistantRuntimeProvider } from "@assistant-ui/react";
   import { useChatRuntime, AssistantChatTransport } from "@assistant-ui/react-ai-sdk";
   import { ThreadList } from "@/components/assistant-ui/thread-list";
   import { Thread } from "@/components/assistant-ui/thread";

   export default function MyApp() {
     const runtime = useChatRuntime({
       transport: new AssistantChatTransport({
         api: "/api/chat",
       }),
     });

     return (
       <AssistantRuntimeProvider runtime={runtime}>
         <div>
           <ThreadList />
           <Thread />
         </div>
       </AssistantRuntimeProvider>
     );
   }
   ```

   **AssistantModal**

   ```
   // run `npx shadcn@latest add @assistant-ui/assistant-modal`

   import { AssistantRuntimeProvider } from "@assistant-ui/react";
   import { useChatRuntime, AssistantChatTransport } from "@assistant-ui/react-ai-sdk";
   import { AssistantModal } from "@/components/assistant-ui/assistant-modal";

   export default function MyApp() {
     const runtime = useChatRuntime({
       transport: new AssistantChatTransport({
         api: "/api/chat",
       }),
     });

     return (
       <AssistantRuntimeProvider runtime={runtime}>
         <AssistantModal />
       </AssistantRuntimeProvider>
     );
   }
   ```

## What's Next?

- [Pick a Runtime](/docs/runtimes/pick-a-runtime) — Choose the right runtime for your needs
- [Generative UI](/docs/tools/tool-ui) — Create rich UI components for tool executions
- [Add Persistence](/docs/cloud) — Save and restore chat conversations
- [Examples](https://github.com/assistant-ui/assistant-ui/tree/main/examples) — Explore full implementations and demos