> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# End call

> Add the End Call tool to a Retell single or multi-prompt agent and define when the agent should automatically hang up based on conversation conditions.

By default, the agent won't end the call automatically. You'll need to configure when and how the call should be terminated using the end call tool.

<Steps>
  <Step title="Add End Call Tool">
    Click "+ Add" in the tools section and select "End Call" from the dropdown menu.

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/end_call.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=53ae902cbe4dbfa4edff788fff3ca94e" alt="Adding end call tool" data-path="images/end_call.png" />
    </Frame>
  </Step>

  <Step title="Configure Termination Conditions">
    Define specific conditions under which the call should be terminated. For example:

    * "If the user says 'thank you', 'goodbye', or 'bye', use the end\_call tool to terminate the conversation."

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/end_call_2.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=6fb7c7e1b1183a8b25eee5cfab6fb885" alt="Configuring end call conditions" data-path="images/end_call_2.png" />
    </Frame>
  </Step>

  <Step title="Update the Prompt">
    Enhance the agent's understanding by incorporating the end call conditions into the prompt. Include specific instructions such as:

    "If the user says 'thank you', 'goodbye', or 'bye', use the end\_call tool to terminate the conversation."

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/end_call_3.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=c9f4683f55f36dae81c83b91bd607ad0" alt="Adding end call instructions to prompt" data-path="images/end_call_3.png" />
    </Frame>
  </Step>
</Steps>
