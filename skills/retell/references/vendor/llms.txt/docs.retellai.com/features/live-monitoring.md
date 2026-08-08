> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Monitor live calls

> Watch ongoing Retell calls in real time from the dashboard: follow the live transcript, listen in silently, take over from the agent, or end a call.

Live Monitoring shows the calls happening in your workspace right now. Open any active call to follow its transcript as the conversation unfolds, listen in silently, take over from the agent, or end the call.

[Call & chat history](/features/session-history) and [post-call analysis](/features/post-call-analysis-overview) work after a call ends. Live Monitoring is the view for a call that's still in progress.

<iframe className="w-full aspect-video rounded-xl" src="https://www.youtube.com/embed/cMRIpkP3C3A?si=5BWwzpWR2iKJ0t8K" title="YouTube video player" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowFullScreen />

## When to use it

Reach for Live Monitoring when you need eyes or ears on a call before it ends:

* **Supervise agents and QA in real time.** [Listen silently](#live-listen) to live calls to judge quality, tone, and where the agent needs coaching. The caller and the agent never know you're there.
* **Rescue a call.** When the agent is stuck or the caller asks for a person, [take over](#take-over) and speak with the caller yourself.
* **Spot-check compliance.** Audit sensitive or high-stakes conversations as they happen, instead of catching problems only in review.

For example, a support lead keeps Live Monitoring open during a hiring campaign's busy hours. When a candidate-screening call stalls (the agent keeps re-asking the same question), the lead opens the call, confirms the loop in the live transcript, and takes over to finish the screening in person.

Live Monitoring isn't built for after-the-fact review. To search and filter finished calls, use [call & chat history](/features/session-history); for automatic scoring and structured data on completed calls, use [post-call analysis](/features/post-call-analysis-overview).

## Access Live Monitoring

1. Go to the dashboard
2. Select **Live Monitoring** in the navigation

<Frame caption="Finding Live Monitoring in the dashboard navigation">
  <div style={{ aspectRatio: "16 / 9", width: "100%", display: "flex", alignItems: "center", justifyContent: "center", backgroundColor: "#f5f5f7" }}>
    <img src="https://mintcdn.com/retellai/YBzyAZoiSdrGmSwJ/images/live-monitoring/nav.png?fit=max&auto=format&n=YBzyAZoiSdrGmSwJ&q=85&s=2f4b67b84c3d249c30a0a57d6af34913" alt="Live Monitoring in the dashboard navigation" style={{ maxHeight: "100%", maxWidth: "100%" }} width="540" height="1474" data-path="images/live-monitoring/nav.png" />
  </div>
</Frame>

<Note>
  Live Monitoring is available to the **Admin** and **Developer** roles, and to any custom role granted call management (write) permission. The **Member** role can't access it. If you don't see Live Monitoring in the navigation, ask your workspace admin to update your role — see [Access Control](/accounts/access-control).
</Note>

<Warning>
  If an agent has [PII scrubbing](/accounts/privacy-disable) enabled, monitoring its live calls requires permission to view raw data, which the **Admin** and **Developer** roles have. Scrubbing runs only after a call ends, so no redacted transcript exists mid-call — users without raw-data access can't monitor, listen to, or take over those calls.
</Warning>

## Live call list

The main view lists every call in progress across your workspace, newest first. It covers phone calls (inbound and outbound) and web calls.

<Frame caption="The list of calls currently in progress">
  <div style={{ aspectRatio: "16 / 9", width: "100%", display: "flex", alignItems: "center", justifyContent: "center", backgroundColor: "#f5f5f7" }}>
    <img src="https://mintcdn.com/retellai/YBzyAZoiSdrGmSwJ/images/live-monitoring/call-list.png?fit=max&auto=format&n=YBzyAZoiSdrGmSwJ&q=85&s=1e8300bde27a35cd34d08b94b5d90519" alt="Live Monitoring call list with Time, Call Duration, Type, From, To, and Call ID columns" style={{ maxHeight: "100%", maxWidth: "100%" }} width="2842" height="406" data-path="images/live-monitoring/call-list.png" />
  </div>
</Frame>

Each row shows the start **Time**, a live-ticking **Call Duration**, the **Type** (Phone Inbound, Phone Outbound, or Web), the **From** and **To** numbers (a dash for web calls), and the **Call ID** (with a copy button on hover).

* **Updates on its own.** The list refreshes about every 4 seconds: new calls appear as they start, briefly highlighted, and drop off as they end.
* **Keyboard navigation.** With a call selected, use the up and down arrow keys to move through the list.

When nothing is active, the view shows **No Ongoing Calls**. Select any call to open it.

## Follow a live transcript

Opening a call shows its details and a **live transcript** that streams in over the call as it happens.

<Frame caption="A call's streaming transcript, with the live actions below it">
  <div style={{ aspectRatio: "16 / 9", width: "100%", display: "flex", alignItems: "center", justifyContent: "center", backgroundColor: "#f5f5f7" }}>
    <img src="https://mintcdn.com/retellai/YBzyAZoiSdrGmSwJ/images/live-monitoring/call-details.png?fit=max&auto=format&n=YBzyAZoiSdrGmSwJ&q=85&s=7480a3f3dce6d06a0c62df92d245ef24" alt="Live call view with the header details, streaming transcript, tool-call and node-transition events, and the Live Listen, Take Over, and End Call actions" style={{ maxHeight: "100%", maxWidth: "100%" }} width="1208" height="1980" data-path="images/live-monitoring/call-details.png" />
  </div>
</Frame>

Above the transcript you'll see the start time, call type, the agent handling the call, the Call ID, and the from/to numbers for phone calls. The transcript itself renders:

* **Agent and caller turns** as chat bubbles.
* **Tool calls** — each function the agent invokes, with its arguments and result in an expandable entry marked *Tool call succeeded* or *Tool call failed*.
* **Node transitions** — each step a flow-based agent moves into.
* **Keypad input** — DTMF digits the caller presses.
* **SMS messages** the agent sends.

The view follows the newest message automatically. Scroll up to read earlier turns, and a **Jump to latest** control appears to snap back to the bottom.

## Listen, take over, or end a call

When you have call management permission, three actions sit below the transcript.

<Frame caption="The Live Listen, Take Over, and End Call actions">
  <div style={{ aspectRatio: "16 / 9", width: "100%", display: "flex", alignItems: "center", justifyContent: "center", backgroundColor: "#f5f5f7" }}>
    <img src="https://mintcdn.com/retellai/EZtLsNXn6ZsF7jNb/images/live-monitoring/action-buttons.png?fit=max&auto=format&n=EZtLsNXn6ZsF7jNb&q=85&s=6fa579ca2eec36932a4ad4f650bf978b" alt="Live Listen, Take Over, and End Call actions below the live transcript" style={{ maxHeight: "100%", maxWidth: "100%" }} width="1176" height="222" data-path="images/live-monitoring/action-buttons.png" />
  </div>
</Frame>

### Live Listen

Select **Live Listen** to hear the call audio in real time. Listening is silent and hidden: neither the caller nor the agent is notified, and your microphone is never transmitted. A **You're listening live** indicator shows while you're connected. Select **Exit Live Listen** to stop. Any number of people can listen to the same call at once.

### Take Over

Select **Take Over** to leave the agent behind and speak with the caller yourself. You confirm in a dialog (*"This permanently stops the AI agent and connects you directly to the caller. It can't be undone."*), then your browser prompts for microphone access.

<Warning>
  Taking over **permanently stops the agent** for that call. The agent does not resume, the live transcript pauses (*"Transcription is paused while you're on the call"*), and the call ends when either side hangs up or you end it. This can't be undone.
</Warning>

<Frame caption="After you take over: the transcript pauses and only End Call remains">
  <div style={{ aspectRatio: "16 / 9", width: "100%", display: "flex", alignItems: "center", justifyContent: "center", backgroundColor: "#f5f5f7" }}>
    <img src="https://mintcdn.com/retellai/EZtLsNXn6ZsF7jNb/images/live-monitoring/footer-after-taken-over.png?fit=max&auto=format&n=EZtLsNXn6ZsF7jNb&q=85&s=eb291e1d52d743e26e2570e9577c2ef9" alt="Taken-over state showing the paused-transcript notice, the You've taken over the call banner, and the End Call button" style={{ maxHeight: "100%", maxWidth: "100%" }} width="1182" height="354" data-path="images/live-monitoring/footer-after-taken-over.png" />
  </div>
</Frame>

* Retell asks for your microphone **before** it stops the agent, so if you deny the prompt the take-over is canceled and the agent keeps handling the call.
* Only **one person** can take over a given call. If someone else already has, you'll see *"Call already taken over by another participant."*
* Works for both phone and web calls.
* While you're on the call, leaving the view or closing the tab warns you first, since leaving ends the call for the caller.

### End Call

Select **End Call** to disconnect the caller and end the call for everyone. You confirm first (*"This disconnects the caller and ends the call for everyone."*). It's available whether or not you've taken over.

## FAQ

<AccordionGroup>
  <Accordion title="Why don't I see the Live Monitoring tab?">
    Live Monitoring is available to the **Admin** and **Developer** roles, and to any custom role granted call management (write) permission — the **Member** role can't access it. Contact your workspace admin if you need access. See [Access Control](/accounts/access-control) for details.
  </Accordion>

  <Accordion title="Can the caller or agent tell that I'm listening?">
    No. Live Listen is silent and hidden: neither the caller nor the agent is notified, and your microphone is never transmitted until you explicitly take over.
  </Accordion>

  <Accordion title="Can several people monitor the same call at once?">
    Up to **5 people** can follow the same call's live transcript at the same time; a sixth is turned away with a *"max watchers reached"* message until a slot frees up. Any number of people can **Live Listen** to the audio. Only **one** person can **Take Over** a call.
  </Accordion>

  <Accordion title="Why can't I monitor, listen to, or take over some calls?">
    If the agent has [PII scrubbing](/accounts/privacy-disable) enabled, monitoring, listening, and taking over require permission to view raw data — held by the **Admin** and **Developer** roles. Scrubbing runs only after the call ends, so there's no redacted transcript to show mid-call, and users without raw-data access are blocked from those live calls.
  </Accordion>

  <Accordion title="What happens to the agent when I take over?">
    The agent is permanently stopped for that call. You're connected directly to the caller, and the agent does not resume. The call ends normally when either side hangs up or you end it.
  </Accordion>

  <Accordion title="What if I deny the microphone prompt when taking over?">
    The take-over is canceled and the agent keeps handling the call. Grant microphone access and try again to take over.
  </Accordion>

  <Accordion title="Can I whisper or coach the agent during a live call?">
    No. Whispering or coaching the agent mid-call — sending guidance only the agent can hear — isn't supported. During a live call you can **Live Listen**, **Take Over**, or **End Call**.
  </Accordion>
</AccordionGroup>
