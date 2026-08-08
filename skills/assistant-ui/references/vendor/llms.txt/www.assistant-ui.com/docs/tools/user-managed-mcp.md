# User-managed MCP servers
URL: /docs/tools/user-managed-mcp

Let end users add and authenticate MCP servers from the browser with @assistant-ui/react-mcp.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

[`@assistant-ui/react-mcp`](https://npmjs.com/package/@assistant-ui/react-mcp) is the user-facing layer for MCP. Where the [server-side MCP guide](/docs/tools/mcp) wires a single fixed set of MCP servers into your API route, this package lets *end users* see a list of connectors, sign in via OAuth, paste a custom server URL, and have the resulting tool catalog flow into the chat — automatically.

Two ways a server reaches the user:

- **Connector** — a preset declared by the app developer with `defineConnector(...)`. The user just clicks Connect (and completes auth).
- **Custom server** — the user supplies the URL, name, and auth in `<McpAddFormPrimitive>`. Hide the add UI to disable.

Both flow through one connection lifecycle, one persisted state surface, and one tool registration path.

## How it works

```
AuiConfig({ mcp: McpManagerResource({ connectors }) })
        │
        ├─ Resource — connection lifecycle, server lookup, OAuth/bearer auth
        ├─ Auto-mounts the modelContext scope when no chat runtime provides one
        └─ Registers connected tools as frontend tools — your chat sees them automatically
```

The manager is a single resource. Mount it with `AuiProvider` like any other scope. OAuth (PKCE + RFC 7591 dynamic client registration), bearer, and "no auth" are first-class. Token refresh runs inside the MCP SDK on 401; this package mediates persistence and the redirect step.

## Setup

1. ### Install

   ```bash
   npm install @assistant-ui/react-mcp
   ```

2. ### Mount the manager

   Declare your connectors and provide the `mcp` scope via `AuiProvider`. No imperative hooks:

   ```
   "use client";

   import { AuiConfig, AuiProvider, useAui } from "@assistant-ui/react";
   import { McpManagerResource, defineConnector } from "@assistant-ui/react-mcp";

   const connectors = [
     defineConnector({
       id: "linear",
       name: "Linear",
       url: "https://mcp.linear.app",
       auth: { type: "oauth", scopes: ["read"] },
       icon: "/icons/linear.svg",
     }),
     defineConnector({
       id: "weather",
       name: "Weather",
       url: "https://mcp.example.com/weather",
       auth: { type: "none" },
       connectionTimeout: 10_000,
     }),
   ];

   export function Providers({ children }: { children: React.ReactNode }) {
     const aui = useAui();
     const config = AuiConfig({
       mcp: McpManagerResource({
         connectors,
         connectionTimeout: 15_000,
       }),
     });
     return (
       <AuiProvider extends={aui} config={config}>
         {children}
       </AuiProvider>
     );
   }
   ```

   Defaults and useful options:

   - `storage` — `McpLocalStorage()` (override for production; see [Storage](#storage))
   - `oauthRedirectUri` — `${window.location.origin}/mcp/callback`
   - `autoConnect` — `true` (connect on mount when usable auth is persisted)
   - `connectionTimeout` — optional timeout in milliseconds. Set it on the manager as a default or on a connector/custom server to bound the MCP readiness flow (`connect()` plus `listTools()`) with a clear error.
   - Connector `id` values must be unique. The id is used for server lookup, OAuth routing, and model-visible tool names such as `linear__search`.

3. ### Pick your UI

   You have two options:

   **Drop-in shadcn dialog** (recommended for most apps) — install the `mcp-config` component via the assistant-ui registry, then render `<McpConfigDialog />` anywhere inside the provider. You get a styled trigger, server cards, status badges, error banners, and the add form for free:

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/mcp-config
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/mcp-config.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react-mcp @assistant-ui/store
   ```

   ```bash
   npx shadcn@latest add badge button dialog input label separator
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/mcp-config.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/mcp-config.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/mcp-config.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/mcp-config.tsx
   ```

   ```
   import { McpConfigDialog } from "@/components/assistant-ui/mcp-config";

   export default function Page() {
     return (
       <header className="flex items-center justify-between">
         <h1>My app</h1>
         <McpConfigDialog />
       </header>
     );
   }
   ```

   Pass children to override the trigger:

   ```
   <McpConfigDialog>
     <Button variant="ghost">Servers</Button>
   </McpConfigDialog>
   ```

   **Compose your own from primitives.** Four namespaces are available, all unstyled and `data-*`-driven. The iteration primitives take a **render function** so the body re-runs per server with the right scope:

   ```
   "use client";

   import {
     McpManagerPrimitive,
     McpServerPrimitive,
   } from "@assistant-ui/react-mcp";

   const ServerCard = () => (
     <McpServerPrimitive.Root>
       <McpServerPrimitive.Icon />
       <McpServerPrimitive.Name />
       <McpServerPrimitive.Status />
       <McpServerPrimitive.ConnectButton>Connect</McpServerPrimitive.ConnectButton>
       <McpServerPrimitive.DisconnectButton>Disconnect</McpServerPrimitive.DisconnectButton>
       <McpServerPrimitive.OAuthLink>Authorize ↗</McpServerPrimitive.OAuthLink>
       <McpServerPrimitive.RemoveButton>Remove</McpServerPrimitive.RemoveButton>
       <McpServerPrimitive.Error />
     </McpServerPrimitive.Root>
   );

   export default function McpPage() {
     return (
       <McpManagerPrimitive.Root>
         <h2>Connectors</h2>
         <McpManagerPrimitive.Connectors>
           {() => <ServerCard />}
         </McpManagerPrimitive.Connectors>

         <h2>Your servers</h2>
         <McpManagerPrimitive.CustomServers>
           {() => <ServerCard />}
         </McpManagerPrimitive.CustomServers>

         <McpManagerPrimitive.AddCustomTrigger>
           Add custom server
         </McpManagerPrimitive.AddCustomTrigger>
       </McpManagerPrimitive.Root>
     );
   }
   ```

   To disable custom servers entirely, just don't render `AddCustomTrigger` and `CustomServers`.

   The iteration primitives wrap each item in an `McpServerByIdProvider`, so the nested `<McpServerPrimitive.*>` automatically reads the right scope. `<ConnectButton>`, `<DisconnectButton>`, `<OAuthLink>` and `<RemoveButton>` only render when the relevant state matches — no manual gating. `<RemoveButton>` is also hidden on connector items (which the user can't remove).

4. ### Add the custom-server form

   ```
   <McpAddFormPrimitive.Root onSubmitted={() => closeDialog()}>
     <McpAddFormPrimitive.NameField />
     <McpAddFormPrimitive.UrlField />
     <McpAddFormPrimitive.AuthSelect /> {/* none | bearer | oauth */}
     <McpAddFormPrimitive.AuthFields /> {/* token or scope input depending on selection */}
     <McpAddFormPrimitive.Error />
     <McpAddFormPrimitive.Submit>Add</McpAddFormPrimitive.Submit>
     <McpAddFormPrimitive.Cancel>Cancel</McpAddFormPrimitive.Cancel>
   </McpAddFormPrimitive.Root>
   ```

   The form owns its own draft state and submits via `aui.mcp.addCustomServer(...)`. Pass a render function to `AuthFields` to fully customize it.

5. ### Handle the OAuth callback

   Add a route at `/mcp/callback` (or whatever you set `oauthRedirectUri` to):

   ```
   "use client";

   import { McpOAuthCallback } from "@assistant-ui/react-mcp";
   import { useRouter } from "next/navigation";
   import { Providers } from "../../providers";

   export default function Callback() {
     const router = useRouter();
     return (
       <Providers>
         <McpOAuthCallback onComplete={() => router.replace("/mcp")} />
       </Providers>
     );
   }
   ```

   The callback reads `?state=...&code=...` from the URL, derives the target server id (encoded in the OAuth `state` parameter automatically), and calls `completeAuth` on the right server.

6. ### That's it — the chat sees your tools

   `McpManagerResource` registers connected tools as **frontend tools** with the `modelContext` scope. Any chat runtime mounted in the same store (e.g. `@assistant-ui/react-ai-sdk`'s `useChatRuntime`) sees them and exposes them to the model — no `useMcpTools` hook, no adapter call.

   ```
   "use client";
   import { lastAssistantMessageIsCompleteWithToolCalls } from "ai";
   import { useChatRuntime } from "@assistant-ui/react-ai-sdk";

   export function Chat() {
     const runtime = useChatRuntime({
       sendAutomaticallyWhen: lastAssistantMessageIsCompleteWithToolCalls,
     });
     /* … */
   }
   ```

   `sendAutomaticallyWhen` sends completed frontend tool results back to the server so the model can continue after a tool call. `useChatRuntime()` targets `/api/chat` by default; to point at a different endpoint, see [Custom transport](/docs/runtimes/ai-sdk/v7#custom-transport).

   Tool names are prefixed `serverId__toolName` to avoid collisions across connected servers. The toolkit re-registers whenever a server connects / disconnects or its tool list changes.

   If no chat runtime is mounted, `McpManagerResource` brings its own minimal `modelContext` along. Tools are still callable directly:

   ```
   // In an event handler, never in render.
   const aui = useAui();
   const out = await aui.mcp.server({ id: "linear" }).callTool("search", { q });
   ```

## Form elicitation

Connected servers can request structured user input through form-mode elicitation. Render `McpElicitationPrimitive.Items` inside a server-scoped subtree, then compose fields and response actions from the unstyled primitives:

Answering a request requires composing `McpElicitationPrimitive`, because the server waits for the form response otherwise. Set `elicitation: false` on a connector or custom server to opt it out of advertising the capability. Numeric fields can stay text inputs (parseable strings are coerced); boolean fields must compose a checkbox, because string drafts for booleans are flagged invalid rather than coerced. Clearing a field returns it to the unanswered state, so an empty text input is omitted from the response rather than submitted as `""` or flagged invalid, unless the schema admits `""` for that property through an `enum` member or a `""` default. Render `enum` properties as a select; when `""` is not a member, its blank option is the unanswered state.

```
import { McpElicitationPrimitive } from "@assistant-ui/react-mcp";

<McpElicitationPrimitive.Items>
  {() => (
    <McpElicitationPrimitive.Root>
      <McpElicitationPrimitive.Message />
      <McpElicitationPrimitive.Error />
      <McpElicitationPrimitive.Fields>
        {({ name, schema, value, setValue }) =>
          (schema as { type?: string } | undefined)?.type === "boolean" ? (
            <label>
              {name}
              <input
                type="checkbox"
                checked={value === true}
                onChange={(event) => setValue(event.target.checked)}
              />
            </label>
          ) : (
            <input
              name={name}
              value={typeof value === "string" ? value : ""}
              onChange={(event) => setValue(event.target.value)}
            />
          )
        }
      </McpElicitationPrimitive.Fields>
      <McpElicitationPrimitive.Accept>Submit</McpElicitationPrimitive.Accept>
      <McpElicitationPrimitive.Decline>Decline</McpElicitationPrimitive.Decline>
      <McpElicitationPrimitive.Cancel>Cancel</McpElicitationPrimitive.Cancel>
    </McpElicitationPrimitive.Root>
  )}
</McpElicitationPrimitive.Items>
```

Client-side validation errors keep the elicitation pending so the user can correct the form, and `McpElicitationPrimitive.Error` renders the resulting message.

## Storage

All persisted state — custom server records, OAuth tokens, PKCE verifiers, DCR client info — goes through a single `MCPStorage` resource. Three built-ins:

- `McpLocalStorage()` — default. Stores under the `aui-mcp:` prefix in `window.localStorage`.
- `McpMemoryStorage()` — in-process Map. Use for SSR/tests where localStorage is absent.
- `McpCustomStorage({...})` — bring your own load/save. Use for app-controlled backends (e.g. POST to your API).

```
import { McpManagerResource, McpCustomStorage } from "@assistant-ui/react-mcp";

AuiConfig({
  mcp: McpManagerResource({
    connectors,
    storage: McpCustomStorage({
      loadCustomServers: async () => fetch("/api/mcp/servers").then((r) => r.json()),
      saveCustomServers: async (records) =>
        fetch("/api/mcp/servers", { method: "PUT", body: JSON.stringify(records) }),
      loadAuthState: async (id) =>
        fetch(`/api/mcp/auth/${id}`).then((r) => (r.ok ? r.json() : null)),
      saveAuthState: async (id, state) =>
        fetch(`/api/mcp/auth/${id}`, { method: "PUT", body: JSON.stringify(state) }),
      clearAuthState: async (id) =>
        fetch(`/api/mcp/auth/${id}`, { method: "DELETE" }),
    }),
  }),
});
```

> [!warn]
>
> `McpLocalStorage` stores tokens in plain text and is XSS-exposed. For anything beyond local prototyping, use `McpCustomStorage` against an HTTP-only-cookie-backed endpoint, or wrap localStorage with your own encrypted serializer.

## Auth

Three modes, declared per-connector or per-custom-record:

```
{ type: "none" }                                    // no auth header
{ type: "bearer", token?: "…" }                     // Authorization: Bearer …
{ type: "oauth",                                    // PKCE + DCR + refresh
  scopes?: ["read"],
  authorizationEndpoint?: "…",                      // overrides RFC 8414 discovery
  tokenEndpoint?: "…",
  registrationEndpoint?: "…",
  clientId?: "…",                                   // skip DCR with a static client
  clientSecret?: "…",
}
```

The OAuth provider implements the MCP SDK's `OAuthClientProvider`. The SDK handles discovery (RFC 8414), DCR (RFC 7591), PKCE, token exchange, and refresh; this package mediates `MCPStorage` reads/writes and the redirect step. The server id is embedded in the OAuth `state` parameter so a single `/mcp/callback` route knows which server to complete.

## State & methods

Render-time state — `useAuiState`:

```
import { useAuiState } from "@assistant-ui/store";

const isHydrated = useAuiState((s) => s.mcp.isHydrated);
const connectionState = useAuiState((s) => s.mcpServer.connectionState);
//                                          ^ requires McpServerByIdProvider
```

Imperative methods: `useAui` + resolve in a callback (never during render):

```
const aui = useAui();

// inside an event handler:
await aui.mcp.addCustomServer({ name, url, auth: { type: "bearer", token } });
await aui.mcp.server({ id }).connect();
await aui.mcp.server({ id }).callTool("echo", { text: "hi" });

// Build a paginated resource browser/preview UI.
type ResourcePage = {
  resources: Array<{ uri: string; name?: string }>;
  nextCursor?: string;
};

const server = aui.mcp.server({ id });
const resources: ResourcePage["resources"] = [];
let nextCursor: string | undefined;

do {
  const page = (await (nextCursor === undefined
    ? server.listResources()
    : server.listResources({ cursor: nextCursor }))) as ResourcePage;
  resources.push(...page.resources);
  nextCursor = page.nextCursor;
} while (nextCursor !== undefined);

const firstResource = resources[0];
if (firstResource) {
  const preview = await server.readResource(firstResource.uri);
}
```

## v1 scope

What ships:

- Tool listing and invocation, auto-registered as frontend tools
- Resource listing and reads for app-built browsers or preview panes
- Form-mode elicitation with app-composed fields and response actions
- OAuth (PKCE + DCR), bearer, none
- StreamableHTTP transport
- Manual connect/disconnect

What's deferred:

- Prompts, sampling
- Auto-reconnect with backoff
- Per-tool enable/disable persistence
- Per-tool consent prompts
- Out-of-the-box token encryption (use `McpCustomStorage` against a server endpoint)

## Related

- [Server-side MCP](/docs/tools/mcp) — App-developer-controlled MCP servers wired into the API route.
- [Tools and tool UI](/docs/tools/defining-tools) — Build custom renderers for tool calls and approvals.