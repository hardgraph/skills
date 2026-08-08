> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create voice agent with TypeScript SDK

> Create a Retell AI phone agent with the TypeScript SDK in Node.js: configure its prompt, voice, webhook, call limits, testing, and published version.

Create a Retell voice agent with the TypeScript SDK by creating a Retell LLM response engine and attaching it to a voice configuration. This example builds an after-hours receptionist, returns the resource IDs, and leaves the first agent version as a draft for testing.

## When to use this workflow

Use this workflow when you manage agent configuration in source code or provision agents for multiple customers. If you already have an agent and only need to place calls, use the [create phone call API](/api-references/create-phone-call) instead.

The example creates an after-hours receptionist for a heating company. It collects the caller's name, callback number, address, and reason for calling without promising a price or appointment time.

## Before you start

You need:

* Node.js 18.10 or later.
* A Retell [API key](/accounts/manage-api-keys).
* A voice ID from the [voice library](/build/platform-voices).
* An HTTPS endpoint if you want to receive [call event webhooks](/features/webhook-overview). The webhook is optional in this example.

## Create the agent

<Steps>
  <Step title="Install the SDK and TypeScript runner">
    Create a Node.js project and install the dependencies:

    ```bash theme={"dark"}
    npm init -y
    npm install retell-sdk
    npm install --save-dev tsx
    ```
  </Step>

  <Step title="Set environment variables">
    Export the API key and voice ID you copied from the dashboard. Set the webhook URL only if your endpoint is ready to receive call events.

    ```bash theme={"dark"}
    export RETELL_API_KEY="<your Retell API key>"
    export RETELL_VOICE_ID="<your voice ID>"
    # Optional: uncomment after your webhook endpoint is ready.
    # export RETELL_WEBHOOK_URL="https://example.com/retell/webhook"
    ```

    Keep the API key in your server environment or secret manager. Do not expose it in browser code or commit it to source control.
  </Step>

  <Step title="Create the response engine and agent">
    Save this file as `create-agent.ts`:

    ```typescript theme={"dark"}
    import Retell from "retell-sdk";

    function requireEnv(name: "RETELL_API_KEY" | "RETELL_VOICE_ID"): string {
      const value = process.env[name];
      if (!value) {
        throw new Error(`Set ${name} before running this script.`);
      }
      return value;
    }

    const apiKey = requireEnv("RETELL_API_KEY");
    const voiceId = requireEnv("RETELL_VOICE_ID");
    const webhookUrl = process.env.RETELL_WEBHOOK_URL;

    const client = new Retell({ apiKey });

    async function main() {
      const llm = await client.llm.create({
        start_speaker: "agent",
        begin_message:
          "Thanks for calling Northstar Heating after hours. How can I help?",
        general_prompt: `
    You are the after-hours receptionist for Northstar Heating.

    Your job:
    - Ask for the caller's full name, callback number, service address, and reason for calling.
    - Ask one question at a time.
    - Read the details back and ask the caller to confirm them.
    - Do not promise pricing, appointment times, or technician availability.
    - Tell the caller that the office will follow up during business hours.
    - When the caller confirms there is nothing else they need, use the end_call tool.
        `.trim(),
        general_tools: [
          {
            type: "end_call",
            name: "end_call",
            description:
              "End the call after the caller confirms the collected details and has no more questions.",
          },
        ],
      });

      console.log(`Created Retell LLM: ${llm.llm_id}`);

      const agent = await client.agent.create({
        agent_name: "Northstar after-hours receptionist",
        response_engine: {
          type: "retell-llm",
          llm_id: llm.llm_id,
        },
        voice_id: voiceId,
        language: "en-US",
        max_call_duration_ms: 15 * 60 * 1_000,
        end_call_after_silence_ms: 2 * 60 * 1_000,
        ...(webhookUrl ? { webhook_url: webhookUrl } : {}),
      });

      console.log(`Created agent: ${agent.agent_id}`);
      console.log(`Draft version: ${agent.version}`);
    }

    main().catch((error) => {
      if (error instanceof Retell.APIError) {
        console.error(`Retell API error ${error.status}: ${error.message}`);
      } else {
        console.error(error);
      }
      process.exitCode = 1;
    });
    ```

    The example sets English explicitly, caps calls at 15 minutes, and ends a call after 2 minutes of silence. As of July 2026, `max_call_duration_ms` accepts 1 minute to 2 hours, while `end_call_after_silence_ms` has a 10-second minimum and a 10-minute default. Because `model` is omitted, the Retell LLM uses the endpoint's default text model.

    If `RETELL_WEBHOOK_URL` is set, the agent-level webhook replaces the account-level webhook for this agent.
  </Step>

  <Step title="Run the script">
    Run the TypeScript file:

    ```bash theme={"dark"}
    npx tsx create-agent.ts
    ```

    A successful run prints the Retell LLM ID, agent ID, and draft version:

    ```text theme={"dark"}
    Created Retell LLM: llm_...
    Created agent: agent_...
    Draft version: 0
    ```
  </Step>
</Steps>

## Test and publish the agent

Open the new agent in the dashboard and run a [web call test](/test/test-web). A draft can be tested without publishing or assigning a phone number.

When the draft is ready, publish it from the dashboard version panel or call the [publish agent API](/api-references/publish-agent) with the `agent_id` and `version` printed by the script. Published versions are immutable. See [agent versioning](/agent/version) before you attach a version or environment tag to a [phone number](/deploy/purchase-number).

If you configured a webhook, [verify its signature](/features/secure-webhook) before processing call events.

## Common questions

<AccordionGroup>
  <Accordion title="Why does the example make two API calls?">
    The response engine and voice agent are separate resources. `client.llm.create()` defines what the agent says and which tools it can use. `client.agent.create()` attaches that response engine to voice and call settings.
  </Accordion>

  <Accordion title="What happens if agent creation fails after the Retell LLM is created?">
    The Retell LLM remains available because the two create calls are not atomic. Reuse its `llm_id` on the next attempt, or remove it with the [delete Retell LLM API](/api-references/delete-retell-llm).
  </Accordion>

  <Accordion title="Do I need to publish before testing?">
    No. Test the draft with a browser web call first. Publish it when you are ready to use an immutable version for a phone number or environment tag.
  </Accordion>
</AccordionGroup>
