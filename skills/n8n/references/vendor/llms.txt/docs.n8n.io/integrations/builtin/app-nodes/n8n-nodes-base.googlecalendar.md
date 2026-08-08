> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.googlecalendar.md).

# Google Calendar

Use the Google Calendar node to automate work in Google Calendar, and integrate Google Calendar with other applications. n8n has built-in support for a wide range of Google Calendar features, including adding, retrieving, deleting and updating calendar events.

On this page, you'll find a list of operations the Google Calendar node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Google Calendar credentials](/integrations/builtin/credentials/google.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* **Calendar**
  * [**Availability**](/integrations/builtin/app-nodes/n8n-nodes-base.googlecalendar/calendar-operations.md#availability): If a time-slot is available in a calendar
* **Event**
  * [**Create**](/integrations/builtin/app-nodes/n8n-nodes-base.googlecalendar/event-operations.md#create): Add an event to calendar
  * [**Delete**](/integrations/builtin/app-nodes/n8n-nodes-base.googlecalendar/event-operations.md#delete): Delete an event
  * [**Get**](/integrations/builtin/app-nodes/n8n-nodes-base.googlecalendar/event-operations.md#get): Retrieve an event
  * [**Get Many**](/integrations/builtin/app-nodes/n8n-nodes-base.googlecalendar/event-operations.md#get-many): Retrieve all events from a calendar
  * [**Update**](/integrations/builtin/app-nodes/n8n-nodes-base.googlecalendar/event-operations.md#update): Update an event

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse n8n-nodes-base.googlecalendar integration templates](https://n8n.io/integrations/google-calendar) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

n8n provides a trigger node for Google Calendar. You can find the trigger node docs [here](/integrations/builtin/trigger-nodes/n8n-nodes-base.googlecalendartrigger.md).

Refer to [Google Calendar's documentation](https://developers.google.com/calendar/api/v3/reference) for more information about the service.

View [example workflows and related content](https://n8n.io/integrations/google-calendar/) on n8n's website.
