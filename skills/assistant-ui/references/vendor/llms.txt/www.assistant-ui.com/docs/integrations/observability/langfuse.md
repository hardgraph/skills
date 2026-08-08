# Langfuse
URL: /docs/integrations/observability/langfuse

Trace AI SDK calls into Langfuse via OpenTelemetry for tracing, evals, and prompt management.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

[Langfuse](https://langfuse.com/) is an open-source LLM observability platform. It gives you a hierarchical trace per request (planner → tool calls → final LLM step), prompt-level analytics, datasets, and LLM-as-judge evals. Self-hostable, OpenTelemetry-native.

Pick Langfuse when you want to see the agent's full call tree inside a single turn. It's complementary to [Helicone](/docs/integrations/observability/helicone), which proxies and logs individual provider calls; many teams run both, with Helicone capturing the request log and Langfuse capturing the trace.

## How it works

Langfuse subscribes to OpenTelemetry spans the AI SDK already emits when telemetry is enabled. No proxy, no wrapping; the SDK ships spans and Langfuse renders them.

## Setup

1. ### Get Langfuse credentials

   Sign up at [langfuse.com](https://langfuse.com/) (or self-host) and create a project. Copy the public and secret keys from project settings.

   ```
   LANGFUSE_PUBLIC_KEY=pk-lf-...
   LANGFUSE_SECRET_KEY=sk-lf-...
   LANGFUSE_BASE_URL=https://cloud.langfuse.com
   ```

   The base URL is region-specific: `https://cloud.langfuse.com` (EU), `https://us.cloud.langfuse.com`, or your self-hosted URL.

2. ### Install the OTel and Langfuse packages

   ```bash
   npm install @langfuse/tracing @langfuse/otel @opentelemetry/sdk-node
   ```

   `@langfuse/tracing` provides the helpers that label traces with user, session, and trace name. `@langfuse/otel` provides the span processor. `@opentelemetry/sdk-node` is the OTel SDK.

3. ### Initialize OpenTelemetry once at startup

   Create an instrumentation file that boots the OTel SDK and registers the Langfuse span processor. In Next.js this goes in `instrumentation.ts` so it runs once per server process. Export the processor at module scope so other code can call `forceFlush()` on it.

   ```
   import { NodeSDK } from "@opentelemetry/sdk-node";
   import { LangfuseSpanProcessor } from "@langfuse/otel";

   export const langfuseSpanProcessor = new LangfuseSpanProcessor();

   export async function register() {
     if (process.env.NEXT_RUNTIME !== "nodejs") return;

     const sdk = new NodeSDK({
       spanProcessors: [langfuseSpanProcessor],
     });
     sdk.start();
   }
   ```

   `NEXT_RUNTIME !== "nodejs"` skips the edge runtime, where OTel doesn't run. On Next.js 14 or earlier, also set `experimental.instrumentationHook = true` in `next.config.mjs`:

   ```
   const nextConfig = {
     experimental: { instrumentationHook: true },
   };
   export default nextConfig;
   ```

4. ### Wrap AI SDK calls with `propagateAttributes`

   Enable telemetry with `experimental_telemetry: { isEnabled: true }`, and wrap each call in `propagateAttributes` from `@langfuse/tracing` to set the trace name and group traces by user / session.

   ```
   import { openai } from "@ai-sdk/openai";
   import { streamText, convertToModelMessages } from "ai";
   import type { UIMessage } from "ai";
   import { propagateAttributes } from "@langfuse/tracing";

   export async function POST(req: Request) {
     const { messages }: { messages: UIMessage[] } = await req.json();

     const userId = "<resolve from your session>";
     const sessionId = "<resolve from your thread state>";

     const result = await propagateAttributes(
       { traceName: "chat-completion", userId, sessionId },
       async () =>
         streamText({
           model: openai("gpt-5.6-luna"),
           messages: await convertToModelMessages(messages),
           experimental_telemetry: { isEnabled: true },
         }),
     );

     return result.toUIMessageStreamResponse();
   }
   ```

   `traceName` becomes the trace label in the Langfuse dashboard. `userId` and `sessionId` are the canonical Langfuse filter dimensions; pass real values from your auth and thread state, not literal strings.

5. ### Run and verify

   Send a message through your assistant. Within a few seconds, a trace should appear in your Langfuse dashboard with:

   - The trace name set by `traceName`.
   - A child span per LLM call and per tool call.
   - Full prompt, completion, and token usage on each span.
   - The metadata you passed (user, session, custom keys) as filters.

   If nothing appears, check the server logs for OTel errors and confirm `LANGFUSE_PUBLIC_KEY` / `LANGFUSE_SECRET_KEY` are loaded in the runtime that handles the request.

## Notes

- **Serverless flush.** On serverless platforms the function exits before OTel flushes its buffer, dropping traces. Import the processor exported in the previous step and call `await langfuseSpanProcessor.forceFlush()` before responding, or use the runtime's `waitUntil` API. Langfuse's docs cover the deployment-specific patterns.
- **Self-hosting.** Point `LANGFUSE_BASE_URL` at your self-hosted instance. The integration is otherwise identical.
- **Sampling.** For high-traffic apps, configure OTel sampling on `NodeSDK` to keep cost predictable. Langfuse can also sample at the project level.
- **Pairing with Helicone.** They are complementary: Helicone proxies and logs every request; Langfuse traces the agent. Many teams use both.

## Related

- [LangSmith](/docs/integrations/observability/langsmith) — LangChain ecosystem alternative, uses wrapAISDK instead of OpenTelemetry.
- [Helicone](/docs/integrations/observability/helicone) — Proxy-based request logging with cost, latency, and prompt diffs per call.
- [AI SDK runtime](/docs/runtimes/ai-sdk/v7) — The runtime that emits the telemetry Langfuse consumes.