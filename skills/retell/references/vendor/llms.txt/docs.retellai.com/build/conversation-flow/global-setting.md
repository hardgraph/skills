> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Configure global settings

> Configure global settings for a Retell conversation flow agent — voice, language, LLM, denoising, and other agent-level options on the empty canvas.

## Agent Global Settings

Click on empty canvas and click setting to access global setting. Here's where you set a lot of agent level settings.

<Steps>
  <Step title="Configure Voice Settings">
    1. Open the voice selection dropdown menu:

    <img src="https://mintcdn.com/retellai/AJT6JQMM1II9WOl-/images/cf/voice.png?fit=max&auto=format&n=AJT6JQMM1II9WOl-&q=85&s=84f8dd7cbf310022cf473abb521a05e7" width="768" height="546" data-path="images/cf/voice.png" />

    2. Listen to the available voice samples and select the voice you want to use for the agent:

    <img src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/cf/voices.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=ffe67919364e523495ecdd7242988699" width="2492" height="1452" data-path="images/cf/voices.png" />

    **Custom Voices**: You can also add voices from the ElevenLabs community or clone voices by clicking "Add custom voice". Learn more in our [voice configuration guide](/build/voice).

    3. You can also adjust a couple of voice settings:
       * voice temperature to make the voice more variant or stable.
       * voice speed to make the agent speak faster or slower.
       * voice volume to make the agent speak louder or quieter.
       * voice model (if applicable): when using certain voice providers, you can choose between different models. Check out the dashboard for detailed nuances of each model.
  </Step>

  <Step title="Select Language of Agent">
    Pick the language(s) the agent will understand and speak. This affects speech recognition, voice pronunciation, and the language the agent responds in — you do not need to add a "respond in X" instruction to your prompt.

    To support multiple languages, switch the selector to **Multiselect** and pick the specific languages you want; for best accuracy, prefer a single language when possible. See [Set language for your agent](/agent/language) and [Configure a multilingual agent](/agent/multilingual) for details.
  </Step>

  <Step title="Select a Language Model">
    Select the model you want to use for the agent. Please note that you can override this within individual nodes. Optionally you can tune the LLM temperature to make answers more variant or more stable.

    We recommend starting with GPT-4.1, which offers an optimal balance of:

    * Response quality
    * Latency
    * Cost-effectiveness

    <img src="https://mintcdn.com/retellai/AJT6JQMM1II9WOl-/images/cf/model-selection.png?fit=max&auto=format&n=AJT6JQMM1II9WOl-&q=85&s=b4efb92e9fb30d002fbb6ecdf764d0f4" width="826" height="1000" data-path="images/cf/model-selection.png" />
  </Step>

  <Step title="Write Global Prompt">
    Here's where you specify the agent's persona, identity, guardrails, etc. This set of text will be available in every node, and will influence all response generation.
  </Step>

  <Step title="Configure Knowledge Base">
    Here's where you can supply contexts to agent via documents, URLs, texts. Read more at [Knowledge Base Guide](/build/knowledge-base).
  </Step>

  <Step title="Configure Speech Settings">
    Here are a lot of options that allow you to finetune how your agent interacts with the user.

    * Background sound: select a background sound that plays throughout the whole call to mimic an environment like a call center, making the conversation more humanlike and engaging.
    * Responsiveness: how responsive the agent is. Set it lower if you want the agent to respond slower, which can be useful when talking to folks like the elderly. Reducing responsiveness by 0.1 adds 0.5 seconds of agent wait time.
    * Interruption Sensitivity: how fast the agent gets interrupted by user interruptions. Set it lower if you want the agent to be more resilient to background speech or user interruptions.
    * Backchanneling: Set up how often and what words the agent uses to acknowledge users.
    * Boosted Keywords: Provides some biases towards certain words, making it easier to get recognized. Common ones are brand names, people's names, etc.
    * Speech Normalization: convert entities like date, currency, numbers into plain words, which can help prevent issues where audio generated was not pronouncing those right.
    * Reminder frequency: how often the agent will remind the user when the user is inactive.
    * Pronunciation: [set a pronunciation guide](/build/add-pronunciation) for specific words.
  </Step>

  <Step title="Configure Call Settings">
    Here are a couple of settings that are more call operation related.

    * Voicemail related settings: set up voicemail detection and what to do when voicemail is detected. See more at [Handle Voicemail](/build/handle-voicemail).
    * End call on silence: set up if the user is inactive for a certain amount of time, the call will be ended.
    * Call duration: set up maximum duration of the call.
    * Pause before speaking: For the beginning of the call, if the agent speaks first, it will wait for the configured duration before speaking, useful to handle scenarios when the user is still picking up the phone.
  </Step>

  <Step title="Configure Post Call Analysis">
    Probably set up later, read more at [Post Call Analysis Guide](/features/post-call-analysis-overview).
  </Step>

  <Step title="Configure Privacy & Webhook">
    Here's where you can set up whether to opt out sensitive data storage, and configure webhook settings for receiving call related events.
  </Step>
</Steps>

## Configure Who Speaks First

Click on `begin` icon, and you can select who speaks first in the call.

<img src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/cf/begin-setting.jpeg?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=d8adaf7926da9172e349c0a1d6a9a1bb" width="715" height="708" data-path="images/cf/begin-setting.jpeg" />
