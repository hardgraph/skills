> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Retell Website Widget

> Embed the Retell website widget on your site with a single script tag — supports chat, voice, and callback modes powered by your chat and voice agents.

## Overview

The Retell embeddable website widget is a production-ready, customizable, and secure widget for websites, powered by the Retell API. The widget is embeddable via a single `<script>` tag and uses the Retell public key system, allowing direct API calls from the frontend—no backend proxy required.

The widget supports two modes:

* **Chat & Voice Widget**: Text-based conversations and real-time voice calls, powered by chat and/or voice agents
* **Callback Widget**: Phone-based conversations using a voice agent

If you'd rather build your own call UI instead of embedding a widget, use the Web SDK — see [make a web call](/deploy/web-call).

<Frame>
  <img width="300" src="https://mintcdn.com/retellai/uiWtPSQXWyH0QdKf/images/retell_widget.png?fit=max&auto=format&n=uiWtPSQXWyH0QdKf&q=85&s=83a090e03c34b5a2397d78c6eaf53103" alt="Retell Website Widget" data-path="images/retell_widget.png" />
</Frame>

### Video tutorial

Walkthrough of embedding the widget on a site and configuring its chat, voice, and callback modes.

<iframe className="w-full aspect-video rounded-xl" src="https://www.youtube.com/embed/w8W4AuheIms" title="How to use Retell AI Widget" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen />

## Prerequisites

Before embedding either widget, you'll need:

1. **Create an Agent**:

* For **chat widget**: Create a chat agent using the [Create Chat Agent](/build/create-chat-agent) guide. Optionally, create a voice agent to enable voice calls within the chat widget.
* For **callback widget**: Create a voice agent to handle phone conversations

2. **Get Your Credentials**:

* Your [Retell Public Key](/accounts/public-keys)
* Your Retell agent ID (chat agent for text chat, voice agent for voice calls or callback)
* For callback widget: Your Retell phone number

## Chat Widget

The chat widget provides text-based conversations through a chat interface. When a voice agent is also configured, users can make real-time voice calls directly within the widget.

### Setup

Add the following script tag to your HTML, within the `<head>` tag:

```html theme={"dark"}
<script
    id="retell-widget"
    src="https://dashboard.retellai.com/retell-widget-v2.js"
    type="module"
    data-public-key="YOUR_RETELL_PUBLIC_KEY"
    data-agent-id="YOUR_CHAT_AGENT_ID"
    data-voice-public-key="YOUR_VOICE_PUBLIC_KEY"
    data-voice-agent-id="YOUR_VOICE_AGENT_ID"
    data-agent-version="YOUR_AGENT_VERSION"
    data-title="YOUR_CUSTOM_TITLE"
    data-logo-url="YOUR_LOGO_URL"
    data-color="YOUR_CUSTOM_COLOR"
    data-theme-color="YOUR_THEME_COLOR"
    data-component-color="YOUR_COMPONENT_COLOR"
    data-fab-text="YOUR_FAB_TEXT"
    data-bot-name="YOUR_BOT_NAME"
    data-popup-message="YOUR_POPUP_MESSAGE"
    data-show-ai-popup="true"
    data-show-ai-popup-time="5"
    data-auto-open="false"
    data-dynamic='{"key": "value"}'
    data-white-label="YOUR_WHITE_LABEL_TOKEN"
    data-recaptcha-key="YOUR_GOOGLE_RECAPTCHA_SITE_KEY"
></script>
```

### Chat Widget Attributes

**Required (at least one):**

* `data-public-key` - Your Retell public key (used for chat API)
* `data-voice-public-key` - Separate public key for voice/web call API. At least one of `data-public-key` or `data-voice-public-key` is required.

**Optional — Agent Configuration:**

* `data-agent-id` - Your chat agent ID (enables text chat)
* `data-voice-agent-id` - Your voice agent ID (enables real-time voice calls). When both `data-agent-id` and `data-voice-agent-id` are set, the widget displays a chooser screen letting users pick between voice and chat.
* `data-agent-version` - Agent version (if unset, uses latest version)
* `data-dynamic` - JSON string with dynamic variables for the chat agent (e.g., `'{"company": "Acme"}'`)

**Optional — Appearance:**

* `data-title` - Custom chat window title (default: `"Chat"`)
* `data-logo-url` - URL of your logo image
* `data-color` - Hex color code for widget theme (e.g., `"#FFA07A"`). Acts as a shorthand that sets both `data-theme-color` and `data-component-color`.
* `data-theme-color` - Hex color for the widget theme/background (default: `"#071a3e"`). Overrides `data-color` for the theme.
* `data-component-color` - Hex color for accent elements like buttons and links (default: `"#3E6AEF"`). Overrides `data-color` for components.
* `data-fab-text` - Text displayed on the floating action button (default: `"Need support?"`)

**Optional — Behavior:**

* `data-bot-name` - Bot name for popup messages (default: `"AI Assistant"`)
* `data-popup-message` - Popup message before users open chat
* `data-show-ai-popup` - Set to `"true"` to enable popup messages, `"false"` to disable (default: `"true"`)
* `data-show-ai-popup-time` - Seconds to delay before showing popup (default: `5`)
* `data-auto-open` - Set to `"true"` to auto-open chat widget on page load (default: `"false"`)

**Optional — Security & Branding:**

* `data-recaptcha-key` - Google reCAPTCHA v3 site key for bot protection (**Note: Only reCAPTCHA v3 is supported**)
* `data-white-label` - White-label token to hide "Powered by Retell" branding. Contact Retell to obtain your token.

### Color Customization

The chat widget supports flexible color customization through a fallback chain:

| Attribute              | CSS Variable        | Default   | Description                                         |
| ---------------------- | ------------------- | --------- | --------------------------------------------------- |
| `data-theme-color`     | `--color-theme`     | `#071a3e` | Widget theme/background color                       |
| `data-component-color` | `--color-component` | `#3E6AEF` | Accent color for buttons, links, etc.               |
| `data-color`           | —                   | —         | Shorthand that sets both theme and component colors |

**Fallback logic:**

* `data-theme-color` falls back to `data-color` if not set
* `data-component-color` falls back to `data-color` if not set

This means you can use `data-color` alone for a simple single-color theme, or combine `data-theme-color` and `data-component-color` for fine-grained control.

### Voice + Chat Hybrid Mode

When both `data-agent-id` and `data-voice-agent-id` are provided, the widget enters **hybrid mode**:

1. Users see a **chooser screen** with "Voice Assistant" and "Chat Assistant" options
2. A **tab bar** at the bottom allows switching between voice and chat at any time
3. Voice calls use WebRTC for real-time audio with a built-in audio visualizer
4. Chat sessions are persisted locally and can be resumed

If only `data-agent-id` is set, the widget goes directly to the chat interface.
If only `data-voice-agent-id` is set, the widget goes directly to the voice call interface.

### reCAPTCHA Protection

The chat widget supports Google reCAPTCHA v3 for bot protection. **Important: Only reCAPTCHA v3 is supported.**

To enable reCAPTCHA:

1. Include the Google reCAPTCHA v3 script in your HTML `<head>` tag:

```html theme={"dark"}
<script src="https://www.google.com/recaptcha/api.js?render=YOUR_GOOGLE_RECAPTCHA_SITE_KEY"></script>
```

2. Add the `data-recaptcha-key` attribute to your widget script with your reCAPTCHA v3 site key
3. Enable reCAPTCHA protection for your public key in the [Retell Public Keys settings](/accounts/public-keys)

### How Chat Widget Works

1. User clicks the chat widget button (displays the FAB button with customizable text)
2. If voice agent is configured, a chooser screen appears; otherwise, chat opens directly
3. **Text chat**: User types messages and receives responses from the chat agent. Chat sessions are automatically persisted in the browser's localStorage and can be resumed later.
4. **Voice calls**: User clicks "Voice Assistant" to start a real-time WebRTC voice call with the agent. A built-in audio visualizer displays the conversation in real time.
5. If reCAPTCHA is enabled, bot protection is automatically applied to new chat sessions and voice calls

### Testing Chat Widget

After adding the widget to your website:

1. Load your website
2. Click the floating button (bottom right)
3. If both agents are configured, choose between voice or chat mode
4. Start a conversation with your agent

### Example: Chat Widget (Text Chat Only)

```html theme={"dark"}
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Retell Chat Widget Example</title>
    <script src="https://www.google.com/recaptcha/api.js?render=YOUR_GOOGLE_RECAPTCHA_SITE_KEY"></script>
    <script
      id="retell-widget"
      src="https://dashboard.retellai.com/retell-widget-v2.js"
      data-public-key="key_xxxxxxxxxxxxxxxxxxxxx"
      data-agent-id="agent_xxxxxxxxxxxxxxxxxxx"
      data-agent-version="0"
      data-title="Chat with us!"
      data-recaptcha-key="YOUR_GOOGLE_RECAPTCHA_SITE_KEY"
    ></script>
  </head>
  <body></body>
</html>
```

### Example: Chat Widget (Voice + Chat Hybrid)

```html theme={"dark"}
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Retell Voice + Chat Widget Example</title>
    <script src="https://www.google.com/recaptcha/api.js?render=YOUR_GOOGLE_RECAPTCHA_SITE_KEY"></script>
    <script
      id="retell-widget"
      src="https://dashboard.retellai.com/retell-widget-v2.js"
      data-public-key="key_xxxxxxxxxxxxxxxxxxxxx"
      data-voice-public-key="key_xxxxxxxxxxxxxxxxxxxxx"
      data-agent-id="agent_chat_xxxxxxxxxxxxxxx"
      data-voice-agent-id="agent_voice_xxxxxxxxxxx"
      data-title="How can we help?"
      data-color="#FFA07A"
      data-recaptcha-key="YOUR_GOOGLE_RECAPTCHA_SITE_KEY"
    ></script>
  </head>
  <body></body>
</html>
```

## Callback Widget

The callback widget collects user information and initiates a phone call instead of a chat session. This mode requires a voice agent to handle the phone conversation.

<Frame>
  <img width="300" src="https://mintcdn.com/retellai/uiWtPSQXWyH0QdKf/images/retell_callback_widget.png?fit=max&auto=format&n=uiWtPSQXWyH0QdKf&q=85&s=97440968fd8edff9a9efa0394d7cad18" alt="Retell Callback Widget" data-path="images/retell_callback_widget.png" />
</Frame>

### Setup

Add the following script tag to your HTML, within the `<head>` tag:

```html theme={"dark"}
<script
  id="retell-widget"
  src="https://dashboard.retellai.com/retell-widget-v2.js"
  type="module"
  data-public-key="YOUR_RETELL_PUBLIC_KEY"
  data-agent-id="YOUR_VOICE_AGENT_ID"
  data-widget="callback"
  data-phone-number="YOUR_RETELL_PHONE_NUMBER"
  data-title="Request a Call"
  data-countries="US,CA,GB"
  data-tc="https://yoursite.com/terms"
  data-color="#FFA07A"
  data-recaptcha-key="YOUR_GOOGLE_RECAPTCHA_SITE_KEY"
  data-fab-text="Get a call from us"
></script>
```

### Callback Widget Attributes

**Required:**

* `data-public-key` - Your Retell public key
* `data-agent-id` - Your voice agent ID (not chat agent)
* `data-widget="callback"` - Enables callback mode
* `data-phone-number` - Your Retell phone number that will make the outbound call

**Optional:**

* `data-title` - Custom widget title
* `data-color` - Hex color code for widget theme
* `data-countries` - Comma-separated country codes for country selector (e.g., "US,CA,GB")
* `data-tc` - URL to your terms and conditions page
* `data-recaptcha-key` - Google reCAPTCHA v3 site key for bot protection

### How Callback Widget Works

**Note:** The callback widget supports the same reCAPTCHA v3 protection as the chat widget. To enable it, follow the instructions in the [reCAPTCHA Protection](#recaptcha-protection) section above.

1. User clicks the callback widget button (displays a phone icon)
2. A form appears collecting:

* First name (required)
* Last name (required)
* Phone number (required)
* Privacy policy agreement checkbox (required)

3. User submits the form
4. If reCAPTCHA is enabled, the form submission is validated
5. The widget creates a phone call using the Retell API
6. User receives a call from your specified phone number
7. The conversation is handled by your configured voice agent

### Testing Callback Widget

After adding the widget to your website:

1. Load your website
2. Click the floating button (bottom right, phone icon)
3. Fill out the contact form
4. Submit and wait for the phone call

### Example: Callback Widget

```html theme={"dark"}
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Retell Callback Widget Example</title>
    <script src="https://www.google.com/recaptcha/api.js?render=YOUR_GOOGLE_RECAPTCHA_SITE_KEY"></script>
    <script
      id="retell-widget"
      src="https://dashboard.retellai.com/retell-widget-v2.js"
      data-public-key="key_xxxxxxxxxxxxxxxxxxxxx"
      data-agent-id="agent_xxxxxxxxxxxxxxxxxxx"
      data-widget="callback"
      data-phone-number="+15551234567"
      data-title="Request a Call"
      data-countries="US,CA,GB"
      data-tc="https://example.com/terms"
      data-color="#FFA07A"
      data-recaptcha-key="YOUR_GOOGLE_RECAPTCHA_SITE_KEY"
      data-fab-text="Get a call from us"
    ></script>
  </head>
  <body></body>
</html>
```

## Widget Behavior Summary

* **Chat Widget (text only)**: Shows FAB button, opens chat interface for text conversations with persisted chat history
* **Chat Widget (voice + chat)**: Shows FAB button, displays chooser screen for voice calls or text chat with tab switching
* **Callback Widget**: Shows phone icon, opens form to collect contact info and initiates phone call
