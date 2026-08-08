> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Handle voicemail and IVR

> Configure Retell agents to detect voicemails and IVR systems on outbound calls and choose what to do next — hang up, leave a message, or navigate the menu.

When making [outbound calls](/deploy/outbound-call), your agent may reach a voicemail or an IVR (Interactive Voice Response) menu instead of a person. You can configure the agent to detect these automatically and choose what happens next.

For example, an outbound reminder campaign can leave a callback message whenever it reaches a voicemail, instead of hanging up silently.

To capture keypad input from the person on the other end, see [capture DTMF input](/build/user-dtmf).

<Frame caption="Voicemail and IVR options in the agent's Call Settings.">
  <img height="300" src="https://mintcdn.com/retellai/_fOA1onl6tLJVhDl/images/voicemail_ivr.png?fit=max&auto=format&n=_fOA1onl6tLJVhDl&q=85&s=00ca575ac97c4bcfa9e14f3ef0d4472c" alt="Call Settings panel with Voicemail Detection toggled on, Voicemail Response set to 'Leave a message' with a static callback sentence, and IVR Hangup toggled on." data-path="images/voicemail_ivr.png" />
</Frame>

## Voicemail detection

The system runs voicemail detection continuously within a timeout window. You can choose what happens when a voicemail is detected:

* **Hang up** — disconnect immediately, even if the voicemail greeting is still playing.
* **Leave a message** — wait for the agent's turn to speak, then deliver a message. Supports [dynamic variables](/build/dynamic-variables).

If the timeout is reached without detecting a voicemail, detection stops and the call continues normally.

<Steps>
  <Step title="Enable voicemail detection">
    Navigate to your agent settings, then open **Call Settings** and enable **Voicemail Detection**.
  </Step>

  <Step title="Choose voicemail behavior">
    Select one of the following options:

    * **Hang up if reaching voicemail**: The agent disconnects when it detects a voicemail.
    * **Leave a message if reaching voicemail**: The agent waits for its turn to speak, then leaves a message. Choose between:
      * **Prompt**: A dynamically generated message based on instructions you provide, such as `Summarize the call and ask the user to call back.`
      * **Static Sentence**: A fixed message, such as `Hey {{user_name}}, sorry we could not reach you directly. Please give us a callback if you can.`
  </Step>
</Steps>

## IVR hangup

When enabled, the system detects if your outbound call reaches an IVR (automated phone menu) system and automatically hangs up the call.

<Note>
  IVR hangup is separate from [IVR navigation](/build/single-multi-prompt/press-digit), which lets your agent interact with and navigate through IVR menus using DTMF tones.
</Note>

To enable, navigate to your agent settings, then open **Call Settings** and toggle on **IVR Hangup**.

## FAQ

<AccordionGroup>
  <Accordion title="The voicemail isn't being hung up even though hang up is enabled. What's wrong?">
    This most likely means the call is being classified as an IVR rather than a voicemail. Because the two detection types are handled separately, voicemail hang up will not trigger in this case. Enable **IVR Hangup** in your agent's **Call Settings** to handle this scenario and automatically disconnect when an IVR system is detected.
  </Accordion>

  <Accordion title="Will voicemail or IVR detection run for the entire call?">
    No, voicemail and IVR detection will only run for the first 3 minutes of the call. Please contact [support@retellai.com](mailto:support@retellai.com) if you need to extend this.
  </Accordion>

  <Accordion title="Is there a latency impact when using voicemail detection?">
    Voicemail detection is optimized for real-time use cases and generally adds under 30ms of latency.
  </Accordion>

  <Accordion title="How can I tell if a call reached a voicemail or IVR?">
    If voicemail is detected, the call will have a [disconnection reason](/reliability/debug-call-disconnect) of `voicemail_reached`. If IVR is detected, the disconnection reason will be `ivr_reached`.
  </Accordion>

  <Accordion title="If the voicemail greeting keeps playing, will the agent hang up?">
    Yes. When set to hang up, the agent disconnects immediately upon detection, regardless of whether the voicemail greeting is still playing.
  </Accordion>

  <Accordion title="If the voicemail greeting keeps playing, will the agent leave a message?">
    No. When set to leave a message, the agent waits for its turn to speak before delivering the message.
  </Accordion>
</AccordionGroup>
