> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Press Digit Node

> Press digit nodes let Retell agents navigate IVR menus by inferring and pressing the correct keypad digit silently during an outbound phone call.

The Press Digit Node is used to navigate through IVR (Interactive Voice Response) systems. When in this node, the agent will not speak. Instead, it evaluates whether it should press a digit and determines which specific digit to press.

The node evaluates whether to press a digit each time the user (IVR system) finishes speaking. This timing is also affected by the detection delay setting. If a digit press is needed, the agent will infer the appropriate digit and press it.

<iframe className="w-full aspect-video rounded-xl" src="https://www.youtube.com/embed/agoG3VHbSxA" title="YouTube video player" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowFullScreen />

<img src="https://mintcdn.com/retellai/i0NYIKgRtRqm3xFI/images/cf/press-digit-node.gif?s=fddd18cca34075a2c70ce43598015e93" alt="Press digit node navigating an IVR system" width="800" height="502" data-path="images/cf/press-digit-node.gif" />

## Configure Press Digit Behavior

<Steps>
  <Step title="Setup IVR Navigation Instructions">
    Provide clear instructions so the agent knows whether and what digit to press. Include keywords or phrases to listen for, as well as which ones to avoid.

    **Sample prompt:**

    ```
    Your goal is to reach the scheduling or appointments department.

    Preferred navigation keywords:
    • Scheduling
    • Appointments
    • New patients
    • Front desk

    Avoid:
    • Billing
    • Referrals
    • Medical records
    • Clinical departments

    If you are unsure which IVR option is correct:
    Choose the option most closely related to scheduling or appointments.
    ```
  </Step>

  <Step title="Configure Detection Delay">
    Some IVR systems speak slowly, so to make sure the agent does not make any decision prematurely, you can set a delay on pauses to make sure the whole IVR menu is captured. We recommend setting this to 1 second.
  </Step>

  <Step title="Configure Transitions">
    Transitions occur when the IVR system finishes speaking. When writing your transitions, ensure you cover both successful navigation and potential failure scenarios or edge cases.

    **Success scenario:** Define when the agent has successfully navigated to the target. For example, write conditions like `Reached scheduling department`. If the digit press was correct, the IVR response will confirm this.

    **Edge cases:** Cover scenarios like getting stuck in loops. For example, write conditions like `Menu repeated 3 times` to handle repetitive menus.

    **Example transition conditions:**

    ```
    You've reached the scheduling department.
    ```

    ```
    Menu repeated 3 times.
    ```

    ```
    You've reached the wrong department or company.
    ```

    ```
    You've reached an after-hours or voicemail message.
    ```
  </Step>
</Steps>

## Rest of Node Settings

* **Global Node**: read more at [Global Node](/build/conversation-flow/global-node)
* **LLM**: choose a different model for this particular node. Will be used for determining whether and what digit to press.
* **Fine-tuning examples**: Add example conversations to train your AI agent how to handle specific scenarios. Read more at [finetune examples](/build/conversation-flow/finetune-examples).
