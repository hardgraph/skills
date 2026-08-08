# Voice
URL: /docs/ui/voice

Realtime voice session controls with connect, mute, and status indicator.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

A control bar for realtime bidirectional voice sessions with an animated orb indicator. Works with any `RealtimeVoiceAdapter` (LiveKit, ElevenLabs, etc.).

\[interactive preview omitted]

## Getting Started

1. ### Add the component

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/voice
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/voice.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react
   ```

   ```bash
   npx shadcn@latest add button tooltip
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/voice.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/voice.tsx)
   - [components/assistant-ui/tooltip-icon-button.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/voice.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/voice.tsx \
     -o components/assistant-ui/tooltip-icon-button.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx
   ```

   This adds `/components/assistant-ui/voice.tsx` to your project, which you can adjust as needed.

2. ### Configure a voice adapter

   Pass a `RealtimeVoiceAdapter` to your runtime. See the [Realtime Voice guide](/docs/guides/voice) for details.

   ```
   const runtime = useChatRuntime({
     adapters: {
       voice: myVoiceAdapter,
     },
   });
   ```

3. ### Use in your application

   ```
   import { Thread } from "@/components/assistant-ui/thread";
   import { VoiceControl } from "@/components/assistant-ui/voice";
   import { AuiIf } from "@assistant-ui/react";

   export default function Chat() {
     return (
       <div className="flex h-full flex-col">
         <AuiIf condition={(s) => s.thread.capabilities.voice}>
           <VoiceControl />
         </AuiIf>
         <div className="min-h-0 flex-1">
           <Thread />
         </div>
       </div>
     );
   }
   ```

## Anatomy

The `VoiceControl` component is built with the following hooks and conditionals:

```
import { AuiIf, useVoiceState, useVoiceControls } from "@assistant-ui/react";

<div className="aui-voice-control">
  <VoiceIndicator />

  <AuiIf condition={(s) => s.thread.voice == null}>
    <VoiceConnectButton />
  </AuiIf>

  <AuiIf condition={(s) => s.thread.voice?.status.type === "running"}>
    <VoiceMuteButton />
    <VoiceDisconnectButton />
  </AuiIf>
</div>
```

## Examples

### Conditionally show voice controls

Only render when a voice adapter is configured:

```
<AuiIf condition={(s) => s.thread.capabilities.voice}>
  <VoiceControl />
</AuiIf>
```

### Voice toggle in composer

Add a compact voice toggle button inside the composer action area:

```
function ComposerVoiceToggle() {
  const voiceState = useVoiceState();
  const { connect, disconnect } = useVoiceControls();
  const isActive =
    voiceState?.status.type === "running" ||
    voiceState?.status.type === "starting";

  return (
    <AuiIf condition={(s) => s.thread.capabilities.voice}>
      <button
        type="button"
        onClick={() => (isActive ? disconnect() : connect())}
        aria-label={isActive ? "End voice" : "Start voice"}
      >
        {isActive ? <PhoneOffIcon /> : <PhoneIcon />}
      </button>
    </AuiIf>
  );
}
```

### Custom indicator colors

Override the indicator styles by targeting the `aui-voice-indicator` class:

```
.aui-voice-indicator {
  /* Override active color */
  &.bg-green-500 {
    background: theme("colors.blue.500");
  }
}
```

## States

The `VoiceOrb` responds to five voice session states with distinct animations:

\[interactive preview omitted]

## Variants

Four built-in color palettes. Size is controlled via `className`.

\[interactive preview omitted]

## Sub-components

| Component               | Description                                                                                       |
| ----------------------- | ------------------------------------------------------------------------------------------------- |
| `VoiceOrb`              | Animated orb visual with gradient, glow, and ripple effects. Accepts `state` and `variant` props. |
| `VoiceControl`          | Control bar with status dot, connect/disconnect, and mute/unmute buttons.                         |
| `VoiceConnectButton`    | Calls `connect()`. Shown when no session is active.                                               |
| `VoiceMuteButton`       | Toggles `mute()`/`unmute()`. Shown when session is running.                                       |
| `VoiceDisconnectButton` | Calls `disconnect()`. Shown when session is active.                                               |

All sub-components are exported and can be used independently for custom layouts.

## State Selectors

Use these with `AuiIf` or `useAuiState` to build custom voice UI:

| Selector                      | Type                                 | Description                                                                    |
| ----------------------------- | ------------------------------------ | ------------------------------------------------------------------------------ |
| `s.thread.capabilities.voice` | `boolean`                            | Whether a voice adapter is configured                                          |
| `s.thread.voice`              | `VoiceSessionState \| undefined`     | `undefined` when no session                                                    |
| `s.thread.voice?.status.type` | `"starting" \| "running" \| "ended"` | Session phase                                                                  |
| `s.thread.voice?.isMuted`     | `boolean`                            | Microphone muted state                                                         |
| `s.thread.voice?.mode`        | `"listening" \| "speaking"`          | Who is currently active (user or agent)                                        |
| `useVoiceVolume()`            | `number`                             | Real-time audio level (0–1), separate from main state to avoid 20Hz re-renders |