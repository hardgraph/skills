> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.telegram.md).

# Telegram

Use the Telegram node to automate work in [Telegram](https://telegram.org/) and integrate Telegram with other applications. n8n has built-in support for a wide range of Telegram features, including getting files as well as deleting and editing messages.

On this page, you'll find a list of operations the Telegram node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Telegram credentials](/integrations/builtin/credentials/telegram.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* [**Chat** operations](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/chat-operations.md)
  * [**Get**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/chat-operations.md#get-chat) up-to-date information about a chat.
  * [**Get Administrators**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/chat-operations.md#get-administrators): Get a list of all administrators in a chat.
  * [**Get Member**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/chat-operations.md#get-chat-member): Get the details of a chat member.
  * [**Leave**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/chat-operations.md#leave-chat) a chat.
  * [**Set Description**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/chat-operations.md#set-description) of a chat.
  * [**Set Title**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/chat-operations.md#set-title) of a chat.
* [**Callback** operations](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/callback-operations.md)
  * [**Answer Query**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/callback-operations.md#answer-query): Send answers to callback queries sent from [inline keyboards](https://core.telegram.org/bots/features#inline-keyboards).
  * [**Answer Inline Query**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/callback-operations.md#answer-inline-query): Send answers to callback queries sent from inline queries.
* [**File** operations](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/file-operations.md)
  * [**Get File**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/file-operations.md#get-file) from Telegram.
* [**Message** operations](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md)<br>

  * [**Delete Chat Message**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#delete-chat-message).
  * [**Edit Message Text**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#edit-message-text): Edit the text of an existing message.
  * [**Pin Chat Message**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#pin-chat-message) for the chat.
  * [**Send Animation**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#send-animation) to the chat.
    * For use with GIFs or H.264/MPEG-4 AVC videos without sound up to 50 MB in size.
  * [**Send Audio**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#send-audio) file to the chat and display it in the music player.
  * [**Send Chat Action**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#send-chat-action): Tell the user that something is happening on the bot's side. The status is set for 5 seconds or less.
  * [**Send Document**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#send-document) to the chat.
  * [**Send Location**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#send-location): Send a geolocation to the chat.
  * [**Send Media Group**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#send-media-group): Send a group of photos and/or videos.
  * [**Send Message**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#send-message) to the chat.
  * [**Send Photo**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#send-photo) to the chat.
  * [**Send Sticker**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#send-sticker) to the chat.
    * For use with static .WEBP, animated .TGS, or video .WEBM stickers.
  * [**Send Video**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#send-video) to the chat.
  * [**Unpin Chat Message**](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/message-operations.md#unpin-chat-message) from the chat.

  <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p><strong>Add bot to channel</strong></p><p>To use most of the <strong>Message</strong> operations, you must add your bot to a channel so that it can send messages to that channel. Refer to <a href="/pages/hV4AHvh69ve544XxxWS3#add-a-bot-to-a-telegram-channel">Common Issues | Add a bot to a Telegram channel</a> for more information.</p></div>

  ## Templates and examples

[Browse n8n-nodes-base.telegram integration templates](https://n8n.io/integrations/telegram) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Telegram's API documentation](https://core.telegram.org/bots/api) for more information about the service.

n8n provides a trigger node for Telegram. Refer to the trigger node docs [here](/integrations/builtin/trigger-nodes/n8n-nodes-base.telegramtrigger.md) for more information.

## Common issues <a href="#common-issues" id="common-issues"></a>

For common errors or issues and suggested resolution steps, refer to [Common Issues](/integrations/builtin/app-nodes/n8n-nodes-base.telegram/common-issues.md).
