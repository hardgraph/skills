> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Implement chat with create chat completion

> Step-by-step guide to implementing chat functionality with a Retell chat agent using the create chat completion API for web and SMS channels.

This guide explains how to implement chat functionality using Retell's Chat API. You'll learn how to start a chat session, generate responses, and end the session.

<Steps>
  <Step title="Create a Chat Agent">
    Before starting a chat session, you need a chat agent to handle the conversation.

    For detailed instructions on creating a chat agent, refer to the [Create Chat Agent](/build/create-chat-agent) guide.
  </Step>

  <Step title="Create a Chat Session">
    To start a chat session, use the `create-chat` API endpoint.

    The API will return a `chat_id` that you'll need for subsequent requests.

    For detailed API information, refer to the [Create Chat API Reference](/api-references/create-chat).
  </Step>

  <Step title="Create a Chat Completion">
    To generate a response from the chat agent, use the `create-chat-completion` API endpoint.

    The API will return the agent's response in the `messages` array. All conversation history is automatically stored in Retell's database, so you don't need to manage conversation context yourself.

    For detailed API information, refer to the [Create Chat Completion API Reference](/api-references/create-chat-completion).
  </Step>

  <Step title="Retrieve Chat Details">
    You can retrieve details about a chat session using the `get-chat` API endpoint.

    You can also list chat sessions using the `POST /v3/list-chats` endpoint.

    For detailed API information, refer to the [Get Chat API Reference](/api-references/get-chat) and [List Chats API Reference](/api-references/list-chats).
  </Step>

  <Step title="End Chat Session">
    When the conversation is complete, end the chat session using the `end-chat` API endpoint.

    If Auto-Close Inactive Chats is enabled, chats will automatically end when the timeout is triggered, or you can end them anytime with the `end-chat` API.

    For detailed API information, refer to the [End Chat API Reference](/api-references/end-chat).
  </Step>
</Steps>

## SMS Integration

Retell also supports Twilio SMS integration, allowing you to deploy your chat agents to receive and respond to text messages. To enable SMS functionality for your chat agents, refer to the [Enable SMS](/deploy/enable-sms) guide.

Once you complete the SMS integration setup, you'll have access to:

* **Make an outbound SMS** button to start a new SMS session
* Inbound SMS agent configuration
* Outbound SMS agent configuration
* Inbound webhook setup for receiving SMS messages

<Note>
  **SMS** conversations with your chat agent on a phone number can include **multimedia** (for example MMS). **Text chat** via the `create-chat-completion` API does not support images or other multimedia.
</Note>
