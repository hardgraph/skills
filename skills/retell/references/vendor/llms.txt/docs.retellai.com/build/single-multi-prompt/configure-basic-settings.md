> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Step 2: Configure basic agent settings

> Set up a Retell single or multi-prompt agent: choose the language model, pick a voice and speaking speed, and decide who speaks first on the call.

Three settings decide how your agent sounds and behaves before you write a word of prompt: the language model, the voice, and who speaks first. Set those, and the agent can hold a call.

<Steps>
  <Step title="Select a language model">
    Open the model dropdown in the agent toolbar. Models are grouped into **Versatile and highly intelligent**, **Fast and cost-efficient**, and **Speech to speech**, with the per-minute price next to each one.

    Start with any model marked **Suggested**. Those are the models we currently recommend for a good balance of response quality, latency, and cost, and the list is kept up to date as new models ship. You can switch models later without rewriting your prompt.

    <Frame caption="The model dropdown, with the Suggested pill on the first recommended model highlighted.">
      <img src="https://mintcdn.com/retellai/ccDgYqUTi0dx_57A/images/agent-model-selector.png?fit=max&auto=format&n=ccDgYqUTi0dx_57A&q=85&s=920322128b6797e3d71f764476244f67" alt="The agent's model dropdown open, grouped under a 'Versatile and highly intelligent' heading. Rows list GPT 5.6 Terra, GPT 5.5, GPT 5.4, GPT 4.1, Claude 5 Sonnet, GPT 5.2, GPT 5.1, GPT 5, Claude 4.6 Sonnet, Claude 4.5 Sonnet, Gemini 3.5 Flash, and Gemini 3.0 Flash, each with a per-minute price, followed by a 'Fast and cost-efficient' heading. A blue box highlights the grey 'Suggested' pill next to the first model." style={{ maxHeight: 560 }} width="1742" height="1120" data-path="images/agent-model-selector.png" />
    </Frame>
  </Step>

  <Step title="Choose a voice">
    Open the voice control in the agent toolbar, next to the model dropdown.

    <Frame caption="The voice control in the agent toolbar.">
      <img src="https://mintcdn.com/retellai/ccDgYqUTi0dx_57A/images/agent-voice-control.png?fit=max&auto=format&n=ccDgYqUTi0dx_57A&q=85&s=a75c99b819776298fd7ee5e9845fa100" alt="The agent toolbar showing the model dropdown, a voice control displaying the avatar and name 'Brynne', and a language dropdown set to English (US). A blue box highlights the voice control." style={{ maxHeight: 560 }} width="1800" height="452" data-path="images/agent-voice-control.png" />
    </Frame>

    This opens the **Select Voice** dialog. Each voice card shows its accent, age, and provider, plus the **voice ID** you use when creating agents through the API — note the ID of the voice you pick. Press the play button on a card to hear a sample, and narrow the list with the Gender, Accent, and Search filters.

    <Frame caption="The Select Voice dialog. Each card carries a preview button and the voice ID.">
      <img src="https://mintcdn.com/retellai/ccDgYqUTi0dx_57A/images/agent-voice-library.png?fit=max&auto=format&n=ccDgYqUTi0dx_57A&q=85&s=1bf186d01961c32fc4ed585ca01a9e9c" alt="The Select Voice dialog open over a dimmed agent page, on the 'Platform Voices' tab beside a 'Custom Providers' tab. An 'Add voice clone' button sits next to Gender, Accent, and Search filters. Below, a grid of voice cards for Cimo, Brynne, Kate, Grace, Marissa, and Lily each shows an avatar, name, traits such as 'American · Middle Aged · retell', and a voice ID such as 'ID: retell-Cimo'. Each card has a play button, except the currently selected voice, which shows a checkmark. A blue box highlights the first card." style={{ maxHeight: 560 }} width="1690" height="1024" data-path="images/agent-voice-library.png" />
    </Frame>

    **Custom voices**: to clone a voice, use **Add voice clone** on the Platform Voices tab. To bring a voice from your own provider account, switch to the **Custom Providers** tab and use **Add custom voice**. See the [custom voice guide](/build/voice) for both.

    **Voice speed**: the voice settings popover has a Voice Speed slider from 0.5x to 2.0x, defaulting to 1.0x. Check **Dynamically adjust based on user input** and the agent adapts its speaking speed to the user's pace during the call: it tracks the user's words per minute and gradually shifts its own speed to match. With this on, the user can also ask the agent to speak faster or slower mid-call. The checkbox isn't available on speech-to-speech models, which control their own pacing.
  </Step>

  <Step title="Decide who speaks first">
    The **Welcome Message** setting below the prompt controls how a conversation opens:

    * **User speaks first**: the agent stays silent until the caller says something.
    * **AI speaks first**: the agent opens the conversation. A second dropdown then chooses how:
      * **Dynamic message**: the agent generates its own opener each call, based on your prompt.
      * **Custom message**: the agent reads a fixed message you type in the field below.

    When the agent speaks first, **Pause Before Speaking** appears beside the setting. Raise it if the agent tends to start talking before the caller has finished picking up.

    <Frame caption="The Welcome Message setting with the second dropdown open on Dynamic message.">
      <img src="https://mintcdn.com/retellai/ccDgYqUTi0dx_57A/images/agent-welcome-message.png?fit=max&auto=format&n=ccDgYqUTi0dx_57A&q=85&s=f3af9df2f6336bb63e10d92fb5f13641" alt="The Welcome Message setting below the prompt box. The first dropdown reads 'AI speaks first' and the second is open on 'Dynamic message', with a checkmark beside it and 'Custom message' below. A 'Pause Before Speaking: 0.6s' control sits to the right of the Welcome Message label." style={{ maxHeight: 560 }} width="1800" height="373" data-path="images/agent-welcome-message.png" />
    </Frame>
  </Step>
</Steps>

### More settings

The panels on the right of the agent editor cover everything else. None are required to make a first call.

<Steps>
  <Step title="Write the prompt">
    The large box in the middle is where you specify the agent's persona, identity, task, and guardrails. On a multi-prompt agent this text is the global prompt: it applies in every state and influences all response generation. See [write a single prompt](/build/single-multi-prompt/write-single-prompt) and [write a multi prompt](/build/single-multi-prompt/write-multi-prompt).
  </Step>

  <Step title="Configure Knowledge Base">
    Supply context to the agent as documents, URLs, or plain text. Read more at the [knowledge base guide](/build/knowledge-base).
  </Step>

  <Step title="Configure Speech Settings">
    Fine-tune how the agent speaks and when it takes its turn.

    * Background sound: select a background sound that plays throughout the whole call to mimic an environment like a call center, making the conversation more humanlike and engaging.
    * Responsiveness: how responsive the agent is. Set it lower if you want the agent to wait longer before responding, which can be useful when talking to folks like the elderly. The lower the value, the more wait time is added before the agent responds. You can also check "Dynamically adjust based on user input" to let the agent automatically tune its response timing during the call. When enabled, the agent observes how quickly the user speaks and adjusts accordingly — slower speakers get more patient response timing, while faster speakers get quicker responses.
    * Interruption Sensitivity: how fast the agent gets interrupted by user interruptions. Set it lower if you want the agent to be more resilient to background speech.
    * Reminder frequency: how often the agent will remind the user when the user is inactive.
    * Pronunciation: [set a pronunciation guide](/build/add-pronunciation) for specific words.
  </Step>

  <Step title="Configure Call Settings">
    Here are a couple of settings that are more call operation related.

    * Voicemail related settings: voicemail detection and what to do when voicemail is detected. See more at [Handle Voicemail](/build/handle-voicemail).
    * End call on silence: how long to wait when the user is silent before the call gets ended automatically.
    * Call duration: maximum duration of the call.
  </Step>

  <Step title="Configure Realtime Transcription Settings">
    Tune how the agent hears the caller.

    * Denoising Mode: how aggressively background noise is filtered out of the caller's audio.
    * Transcription Mode: which speech-to-text model transcribes the call.
    * Vocabulary Specialization: bias transcription toward a domain's terms, so words like drug or product names come through correctly.
  </Step>

  <Step title="Configure Post-Call Data Extraction">
    Pull structured results out of each finished call, such as whether the appointment was booked or what the caller asked for. Usually worth setting up later, once the agent handles calls well. Read more at the [post-call analysis guide](/features/post-call-analysis-overview).
  </Step>

  <Step title="Configure Security & Fallback Settings">
    * Data Storage and PII Settings: opt out of storing recordings, transcripts, or personally identifiable information.
    * Secure URLs: serve recordings and logs from signed links that expire, anywhere from 1 minute to 7 days.
    * Fallback Voice: the voice to switch to if your primary voice provider fails mid-call, so the call continues instead of dropping.
    * Guardrails: limits on what the agent is allowed to say.
    * Default Dynamic Variables: fallback values for prompt variables that aren't supplied when the call starts.
  </Step>

  <Step title="Configure Webhook Settings">
    Point the agent at your endpoint to receive call events. See [register a webhook](/features/register-webhook).
  </Step>
</Steps>

### Video tutorial

<Note>
  This walkthrough predates the current agent editor, so some labels differ from what you'll see. The steps above reflect the shipped UI.
</Note>

<iframe width="560" height="315" src="https://www.youtube.com/embed/um2mU8KhOBI?si=vO3Y86FVFbBh_UCB" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen />

See [community prompt templates](https://docs.google.com/document/d/1hx6hdTEjAR4y4xXZ7RLMH2byQNVW1ABxC8S4FwvTx_Y/edit?tab=t.0#heading=h.wf5bktkelope) for examples to start from.

## Next steps

Once you've configured these basic settings, your agent is ready for basic interactions. To enhance its capabilities, proceed to [adding capabilities by using function calling](/build/single-multi-prompt/function-calling).
