# MCP Config Dialog
URL: /docs/ui/mcp-config

Drop-in shadcn dialog that lists MCP connectors and custom servers, with inline OAuth/bearer auth controls and an add form.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

`McpConfigDialog` is a shadcn template that wraps the [`@assistant-ui/react-mcp`](/docs/tools/user-managed-mcp) primitives. Render it anywhere inside an `McpManagerResource`-mounted client and your users get a styled MCP server panel — no manual primitive composition.

## Getting Started

1. ### Install the package and the component

   ```bash
   npm install @assistant-ui/react-mcp
   ```

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

2. ### Mount the manager

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
     }),
   ];

   export function Providers({ children }: { children: React.ReactNode }) {
     const aui = useAui();
     const config = AuiConfig({ mcp: McpManagerResource({ connectors }) });
     return (
       <AuiProvider extends={aui} config={config}>
         {children}
       </AuiProvider>
     );
   }
   ```

3. ### Drop the dialog in

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

   That's it. The default trigger is an outline `"🔌 MCP servers"` button. Click it to open the dialog, which renders a **Connectors** list (your app-defined presets with connect/authorize/disconnect controls), a **Custom servers** list (user-added entries with the same controls plus a remove button), and an inline **Add server** form for URL + name + auth selection.

4. ### Override the trigger (optional)

   Pass children to swap the default button for your own:

   ```
   <McpConfigDialog>
     <Button variant="ghost">Servers</Button>
   </McpConfigDialog>
   ```

5. ### Wire up the OAuth callback

   Required for any OAuth-enabled connector or custom server. See the [OAuth callback section in the react-mcp guide](/docs/tools/user-managed-mcp#handle-the-oauth-callback) for the route handler.

## Related

- [User-managed MCP servers](/docs/tools/user-managed-mcp) — Full @assistant-ui/react-mcp guide — connectors, custom servers, OAuth, storage, custom primitives.
- [Server-side MCP](/docs/tools/mcp) — App-developer-controlled MCP servers wired into the API route.