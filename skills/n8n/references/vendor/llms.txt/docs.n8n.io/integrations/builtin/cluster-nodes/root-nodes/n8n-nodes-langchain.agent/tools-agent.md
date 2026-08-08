> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/tools-agent.md).

# Tools Agent

The Tools Agent uses external tools[^1] and APIs to perform actions and retrieve information. It can understand the capabilities of different tools and determine which tool to use depending on the task. This agent helps integrate LLMs with various external services and databases.

This agent has an enhanced ability to work with tools and can ensure a standard output format.

The Tools Agent implements [Langchain's tool calling](https://js.langchain.com/docs/concepts/tool_calling/) interface. This interface describes available tools and their schemas. The agent also has improved output parsing capabilities, as it passes the parser to the model as a formatting tool.

Refer to [AI Agent](/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent.md) for more information on the AI Agent node itself.

You can use this agent with the [Chat Trigger](https://app.gitbook.com/s/BKcbOzIWja8NfqKDcqHc/builtin/core-nodes/n8n-nodes-base.compression/n8n-nodes-base.compression) node. Attach a memory sub-node so that users can have an ongoing conversation with multiple queries. Memory doesn't persist between sessions.

This agent supports the following chat models:

* [OpenAI Chat Model](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmchatopenai.md)
* [Groq Chat Model](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmchatgroq.md)
* [Mistral Cloud Chat Model](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmchatmistralcloud.md)
* [Anthropic Chat Model](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmchatanthropic.md)
* [Azure OpenAI Chat Model](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmchatazureopenai.md)

<details>

<summary>The Tools Agent can use the following tools...</summary>

* [Call n8n Workflow](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.toolworkflow.md)
* [Code](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.toolcode.md)
* [HTTP Request](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md)
* [Action Network](/integrations/builtin/app-nodes/n8n-nodes-base.actionnetwork.md)
* [ActiveCampaign](/integrations/builtin/app-nodes/n8n-nodes-base.activecampaign.md)
* [Affinity](/integrations/builtin/app-nodes/n8n-nodes-base.affinity.md)
* [Agile CRM](/integrations/builtin/app-nodes/n8n-nodes-base.agilecrm.md)
* [Airtable](/integrations/builtin/app-nodes/n8n-nodes-base.airtable.md)
* [APITemplate.io](/integrations/builtin/app-nodes/n8n-nodes-base.apitemplateio.md)
* [Asana](/integrations/builtin/app-nodes/n8n-nodes-base.asana.md)
* [AWS Lambda](/integrations/builtin/app-nodes/n8n-nodes-base.awslambda.md)
* [AWS S3](/integrations/builtin/app-nodes/n8n-nodes-base.awss3.md)
* [AWS SES](/integrations/builtin/app-nodes/n8n-nodes-base.awsses.md)
* [AWS Textract](/integrations/builtin/app-nodes/n8n-nodes-base.awstextract.md)
* [AWS Transcribe](/integrations/builtin/app-nodes/n8n-nodes-base.awstranscribe.md)
* [Baserow](/integrations/builtin/app-nodes/n8n-nodes-base.baserow.md)
* [Bubble](/integrations/builtin/app-nodes/n8n-nodes-base.bubble.md)
* [Calculator](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.toolcalculator.md)
* [ClickUp](/integrations/builtin/app-nodes/n8n-nodes-base.clickup.md)
* [CoinGecko](/integrations/builtin/app-nodes/n8n-nodes-base.coingecko.md)
* [Compression](/integrations/builtin/core-nodes/n8n-nodes-base.compression.md)
* [Crypto](/integrations/builtin/core-nodes/n8n-nodes-base.crypto.md)
* [DeepL](/integrations/builtin/app-nodes/n8n-nodes-base.deepl.md)
* [DHL](/integrations/builtin/app-nodes/n8n-nodes-base.dhl.md)
* [Discord](/integrations/builtin/app-nodes/n8n-nodes-base.discord.md)
* [Dropbox](/integrations/builtin/app-nodes/n8n-nodes-base.dropbox.md)
* [Elasticsearch](/integrations/builtin/app-nodes/n8n-nodes-base.elasticsearch.md)
* [ERPNext](/integrations/builtin/app-nodes/n8n-nodes-base.erpnext.md)
* [Facebook Graph API](/integrations/builtin/app-nodes/n8n-nodes-base.facebookgraphapi.md)
* [FileMaker](/integrations/builtin/app-nodes/n8n-nodes-base.filemaker.md)
* [Ghost](/integrations/builtin/app-nodes/n8n-nodes-base.ghost.md)
* [Git](/integrations/builtin/core-nodes/n8n-nodes-base.git.md)
* [GitHub](/integrations/builtin/app-nodes/n8n-nodes-base.github.md)
* [GitLab](/integrations/builtin/app-nodes/n8n-nodes-base.gitlab.md)
* [Gmail](/integrations/builtin/app-nodes/n8n-nodes-base.gmail.md)
* [Google Analytics](/integrations/builtin/app-nodes/n8n-nodes-base.googleanalytics.md)
* [Google BigQuery](/integrations/builtin/app-nodes/n8n-nodes-base.googlebigquery.md)
* [Google Calendar](/integrations/builtin/app-nodes/n8n-nodes-base.googlecalendar.md)
* [Google Chat](/integrations/builtin/app-nodes/n8n-nodes-base.googlechat.md)
* [Google Cloud Firestore](/integrations/builtin/app-nodes/n8n-nodes-base.googlecloudfirestore.md)
* [Google Cloud Realtime Database](/integrations/builtin/app-nodes/n8n-nodes-base.googlecloudrealtimedatabase.md)
* [Google Contacts](/integrations/builtin/app-nodes/n8n-nodes-base.googlecontacts.md)
* [Google Docs](/integrations/builtin/app-nodes/n8n-nodes-base.googledocs.md)
* [Google Drive](/integrations/builtin/app-nodes/n8n-nodes-base.googledrive.md)
* [Google Sheets](/integrations/builtin/app-nodes/n8n-nodes-base.googlesheets.md)
* [Google Slides](/integrations/builtin/app-nodes/n8n-nodes-base.googleslides.md)
* [Google Tasks](/integrations/builtin/app-nodes/n8n-nodes-base.googletasks.md)
* [Google Translate](/integrations/builtin/app-nodes/n8n-nodes-base.googletranslate.md)
* [Google Workspace Admin](/integrations/builtin/app-nodes/n8n-nodes-base.gsuiteadmin.md)
* [Gotify](/integrations/builtin/app-nodes/n8n-nodes-base.gotify.md)
* [Grafana](/integrations/builtin/app-nodes/n8n-nodes-base.grafana.md)
* [GraphQL](/integrations/builtin/core-nodes/n8n-nodes-base.graphql.md)
* [Hacker News](/integrations/builtin/app-nodes/n8n-nodes-base.hackernews.md)
* [Home Assistant](/integrations/builtin/app-nodes/n8n-nodes-base.homeassistant.md)
* [HubSpot](/integrations/builtin/app-nodes/n8n-nodes-base.hubspot.md)
* [Jenkins](/integrations/builtin/app-nodes/n8n-nodes-base.jenkins.md)
* [Jira Software](/integrations/builtin/app-nodes/n8n-nodes-base.jira.md)
* [JWT](/integrations/builtin/core-nodes/n8n-nodes-base.jwt.md)
* [Kafka](/integrations/builtin/app-nodes/n8n-nodes-base.kafka.md)
* [LDAP](/integrations/builtin/core-nodes/n8n-nodes-base.ldap.md)
* [Line](/integrations/builtin/app-nodes/n8n-nodes-base.line.md)
* [LinkedIn](/integrations/builtin/app-nodes/n8n-nodes-base.linkedin.md)
* [Mailcheck](/integrations/builtin/app-nodes/n8n-nodes-base.mailcheck.md)
* [Mailgun](/integrations/builtin/app-nodes/n8n-nodes-base.mailgun.md)
* [Mattermost](/integrations/builtin/app-nodes/n8n-nodes-base.mattermost.md)
* [Mautic](/integrations/builtin/app-nodes/n8n-nodes-base.mautic.md)
* [Medium](/integrations/builtin/app-nodes/n8n-nodes-base.medium.md)
* [Microsoft Excel (OneDrive)](/integrations/builtin/app-nodes/n8n-nodes-base.microsoftexcel.md)
* [Microsoft OneDrive](/integrations/builtin/app-nodes/n8n-nodes-base.microsoftonedrive.md)
* [Microsoft Outlook](/integrations/builtin/app-nodes/n8n-nodes-base.microsoftoutlook.md)
* [Microsoft SQL](/integrations/builtin/app-nodes/n8n-nodes-base.microsoftsql.md)
* [Microsoft Teams](/integrations/builtin/app-nodes/n8n-nodes-base.microsoftteams.md)
* [Microsoft To Do](/integrations/builtin/app-nodes/n8n-nodes-base.microsofttodo.md)
* [Monday.com](/integrations/builtin/app-nodes/n8n-nodes-base.mondaycom.md)
* [MongoDB](/integrations/builtin/app-nodes/n8n-nodes-base.mongodb.md)
* [MQTT](/integrations/builtin/app-nodes/n8n-nodes-base.mqtt.md)
* [MySQL](/integrations/builtin/app-nodes/n8n-nodes-base.mysql.md)
* [NASA](/integrations/builtin/app-nodes/n8n-nodes-base.nasa.md)
* [Nextcloud](/integrations/builtin/app-nodes/n8n-nodes-base.nextcloud.md)
* [NocoDB](/integrations/builtin/app-nodes/n8n-nodes-base.nocodb.md)
* [Notion](/integrations/builtin/app-nodes/n8n-nodes-base.notion.md)
* [Odoo](/integrations/builtin/app-nodes/n8n-nodes-base.odoo.md)
* [OpenWeatherMap](/integrations/builtin/app-nodes/n8n-nodes-base.openweathermap.md)
* [Pipedrive](/integrations/builtin/app-nodes/n8n-nodes-base.pipedrive.md)
* [Postgres](/integrations/builtin/app-nodes/n8n-nodes-base.postgres.md)
* [Pushover](/integrations/builtin/app-nodes/n8n-nodes-base.pushover.md)
* [QuickBooks Online](/integrations/builtin/app-nodes/n8n-nodes-base.quickbooks.md)
* [QuickChart](/integrations/builtin/app-nodes/n8n-nodes-base.quickchart.md)
* [RabbitMQ](/integrations/builtin/app-nodes/n8n-nodes-base.rabbitmq.md)
* [Reddit](/integrations/builtin/app-nodes/n8n-nodes-base.reddit.md)
* [Redis](/integrations/builtin/app-nodes/n8n-nodes-base.redis.md)
* [RocketChat](/integrations/builtin/app-nodes/n8n-nodes-base.rocketchat.md)
* [S3](/integrations/builtin/app-nodes/n8n-nodes-base.s3.md)
* [Salesforce](/integrations/builtin/app-nodes/n8n-nodes-base.salesforce.md)
* [Send Email](/integrations/builtin/core-nodes/n8n-nodes-base.sendemail.md)
* [SendGrid](/integrations/builtin/app-nodes/n8n-nodes-base.sendgrid.md)
* [SerpApi (Google Search)](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.toolserpapi.md)
* [Shopify](/integrations/builtin/app-nodes/n8n-nodes-base.shopify.md)
* [Slack](/integrations/builtin/app-nodes/n8n-nodes-base.slack.md)
* [Spotify](/integrations/builtin/app-nodes/n8n-nodes-base.spotify.md)
* [Stripe](/integrations/builtin/app-nodes/n8n-nodes-base.stripe.md)
* [Supabase](/integrations/builtin/app-nodes/n8n-nodes-base.supabase.md)
* [Telegram](/integrations/builtin/app-nodes/n8n-nodes-base.telegram.md)
* [Todoist](/integrations/builtin/app-nodes/n8n-nodes-base.todoist.md)
* [TOTP](/integrations/builtin/core-nodes/n8n-nodes-base.totp.md)
* [Trello](/integrations/builtin/app-nodes/n8n-nodes-base.trello.md)
* [Twilio](/integrations/builtin/app-nodes/n8n-nodes-base.twilio.md)
* [urlscan.io](/integrations/builtin/app-nodes/n8n-nodes-base.urlscanio.md)
* [Vector Store](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.toolvectorstore.md)
* [Webflow](/integrations/builtin/app-nodes/n8n-nodes-base.webflow.md)
* [Wikipedia](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.toolwikipedia.md)
* [Wolfram|Alpha](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.toolwolframalpha.md)
* [WooCommerce](/integrations/builtin/app-nodes/n8n-nodes-base.woocommerce.md)
* [Wordpress](/integrations/builtin/app-nodes/n8n-nodes-base.wordpress.md)
* [X (Formerly Twitter)](/integrations/builtin/app-nodes/n8n-nodes-base.twitter.md)
* [YouTube](/integrations/builtin/app-nodes/n8n-nodes-base.youtube.md)
* [Zendesk](/integrations/builtin/app-nodes/n8n-nodes-base.zendesk.md)
* [Zoho CRM](/integrations/builtin/app-nodes/n8n-nodes-base.zohocrm.md)
* [Zoom](/integrations/builtin/app-nodes/n8n-nodes-base.zoom.md)

</details>

## Node parameters <a href="#node-parameters" id="node-parameters"></a>

Configure the Tools Agent using the following parameters.

### Prompt <a href="#prompt" id="prompt"></a>

Select how you want the node to construct the prompt (also known as the user's query or input from the chat).

Choose from:

* **Take from previous node automatically**: If you select this option, the node expects an input from a previous node called `chatInput`.
* **Define below**: If you select this option, provide either static text or an expression for dynamic content to serve as the prompt in the **Prompt (User Message)** field.

### Require Specific Output Format <a href="#require-specific-output-format" id="require-specific-output-format"></a>

This parameter controls whether you want the node to require a specific output format. When turned on, n8n prompts you to connect one of these output parsers to the node:

* [Auto-fixing Output Parser](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.outputparserautofixing.md)
* [Item List Output Parser](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.outputparseritemlist.md)
* [Structured Output Parser](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.outputparserstructured.md)

## Node options <a href="#node-options" id="node-options"></a>

Refine the Tools Agent node's behavior using these options:

### System Message <a href="#system-message" id="system-message"></a>

If you'd like to send a message to the agent before the conversation starts, enter the message you'd like to send.

Use this option to guide the agent's decision-making.

### Max Iterations <a href="#max-iterations" id="max-iterations"></a>

Enter the number of times the model should run to try and generate a good answer from the user's prompt.

Defaults to `10`.

### Return Intermediate Steps <a href="#return-intermediate-steps" id="return-intermediate-steps"></a>

Select whether to include intermediate steps the agent took in the final output (turned on) or not (turned off).

This could be useful for further refining the agent's behavior based on the steps it took.

### Tracing Metadata <a href="#tracing-metadata" id="tracing-metadata"></a>

Add custom key-value metadata to tracing events for this agent. This is useful for filtering and debugging runs in tracing tools like [LangSmith](https://github.com/n8n-io/n8n-docs/blob/main/advanced-ai/langchain/langsmith.md).

Entries with empty keys or values are ignored.

### Automatically Passthrough Binary Images <a href="#automatically-passthrough-binary-images" id="automatically-passthrough-binary-images"></a>

Use this option to control whether binary images should be automatically passed through to the agent as image type messages (turned on) or not (turned off).

### Enable Streaming <a href="#enable-streaming" id="enable-streaming"></a>

When enabled, the AI Agent sends data back to the user in real-time as it generates the answer. This is useful for long-running generations. This is enabled by default.

{% hint style="info" %}
**Streaming requirements**

For streaming to work, your workflow must use a trigger that supports streaming responses, such as the [Chat Trigger](/integrations/builtin/core-nodes/n8n-nodes-langchain.chattrigger.md) or [Webhook](/integrations/builtin/core-nodes/n8n-nodes-base.webhook.md) node with **Response Mode** set to **Streaming**.
{% endhint %}

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

Refer to the main AI Agent node's [Templates and examples](/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent.md#templates-and-examples) section.

## Dynamic parameters for tools with `$fromAI()` <a href="#dynamic-parameters-for-tools-with-dollarfromai" id="dynamic-parameters-for-tools-with-dollarfromai"></a>

To learn how to dynamically populate parameters for app node tools, refer to [Let AI specify tool parameters with `$fromAI()`](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).

## Human review for tool calls <a href="#human-review-for-tool-calls" id="human-review-for-tool-calls"></a>

You can require human approval before the AI Agent executes specific tools. This is useful for tools that perform sensitive actions like sending messages, modifying records, or deleting data.

To add a human review step:

1. Click the tool connector on the AI Agent node.
2. In the Tools Panel, find the **Human review** section.
3. Select your preferred approval channel (Chat, Slack, Telegram, and more) and configure it.
4. Connect the tools that require approval to the human review step.

When the AI wants to use a gated tool, the workflow pauses and sends an approval request through your chosen channel. The recipient can approve (tool executes) or deny (action canceled).

For detailed setup instructions and best practices, refer to [Human-in-the-loop for AI tool calls](/build/integrate-ai/ai-examples/human-in-the-loop-for-tools.md).

## Common issues <a href="#common-issues" id="common-issues"></a>

For common questions or issues and suggested solutions, refer to [Common issues](/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/common-issues.md).

[^1]: In an AI context, a tool is an add-on resource that the AI can refer to for specific information or functionality when responding to a request. The AI model can use a tool to interact with external systems or complete specific, focused tasks.
