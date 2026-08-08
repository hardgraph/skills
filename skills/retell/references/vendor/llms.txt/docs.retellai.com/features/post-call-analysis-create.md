> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Define the information you want to extract

> Define Retell post-call analysis fields — boolean, selector, text, and number categories — to automatically extract structured information from every call.

<Steps>
  <Step title="Navigate to Post-Call Analysis">
    Go to the agent detail page and click on the "Post-Call Analysis" tab.

    <Frame>
      <img height="200" src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/post-call-analysis-dashboard.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=c5ba16aa27f7b48edff11d06e6928d31" data-path="images/post-call-analysis-dashboard.png" />
    </Frame>
  </Step>

  <Step title="Choose Analysis Category Type and Configure">
    Select the type of analysis that best fits your needs:

    ### Boolean Analysis

    Use for simple yes/no determinations

    Example configuration:

    * **Name**: user\_reached
    * **Description**: Was the user reached or not? Set to false if voicemail is detected, if you are only asked for the reason of the call, or if you are only asked to leave a message. Otherwise, set to true.

    ### Text Analysis

    Use for extracting detailed textual information

    Example configuration:

    * **Name**: detailed\_call\_summary
    * **Description**: Provide a detailed summary of the call so that when the call is transferred, the new human agent has the full context
    * **Format Example**: "Customer called about billing issue. Resolved by explaining the recent price changes. Follow-up needed in 2 weeks."

    ### Number Analysis

    Use for extracting numerical values

    Example configuration:

    * **Name**: purchase\_intent\_amount
    * **Description**: Extract the dollar amount the customer is interested in spending

    ### Selector Analysis

    Use for categorizing from predefined options

    Example configuration:

    * **Name**: issue\_category
    * **Description**: Categorize the main reason for the call.
    * **Choices Example**: \["Technical Support", "Billing Question", "Sales Inquiry", "Product Information"]

    <Note>Please note that you should write the explanation of the choices inside the description field. The choices should only contain the individual choice. </Note>
  </Step>
</Steps>
