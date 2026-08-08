> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Rerun Post-Call/Chat Analysis

> Edit Retell post-call or post-chat analysis prompts and rerun processing on past sessions to regenerate summaries and success metrics with new criteria.

You can now customize the prompts used to summarize conversations and determine call/chat success. After updating these prompts, you can rerun the analysis to regenerate results that better match your criteria.

<Note>When you rerun the analysis for a call, the system always uses the analysis prompts from the latest draft version of the agent—even if the call was originally handled by an older published version. To update the analysis results, make sure to edit the prompts in the latest draft agent.</Note>

<Note>If the session included an [Agent Transfer](/build/single-multi-prompt/transfer-agent), rerunning the analysis follows the same **Post call analysis setting** chosen for that transfer. If the transfer used **Only transferred agent**, the rerun applies the destination agent's full analysis configuration—its analysis fields, analysis model, and analysis prompts—so the rerun results match what you'd get from the live call.</Note>

## Editing Analysis Prompts

<Frame>
  <img width="400" src="https://mintcdn.com/retellai/unp56-BhY5LaMHMD/images/call_analysis_prompts.png?fit=max&auto=format&n=unp56-BhY5LaMHMD&q=85&s=43abbd7012d9f73d1c44b1f10e11dfcc" alt="Agent" data-path="images/call_analysis_prompts.png" />
</Frame>

In the agent's `Post-Call Data Extraction` section (formerly `Post-Call Analysis`), you can customize prompts for the following built-in analysis categories:

### Call Summary

The `call_summary` field provides a high-level summary of the conversation. Adjust this prompt to extract the type of summary or details you care about.

<Frame>
  <img width="400" src="https://mintcdn.com/retellai/zNT4hiYBT-XWhMeP/images/summary_prompt.png?fit=max&auto=format&n=zNT4hiYBT-XWhMeP&q=85&s=5a8fb4ac6dc44d781aa3b0264fe5fa08" alt="Call summary prompt" data-path="images/summary_prompt.png" />
</Frame>

### Call Successful

You can also customize the prompt that evaluates whether a call or chat was successful, allowing you to define your own success criteria.

<Frame>
  <img width="400" src="https://mintcdn.com/retellai/zNT4hiYBT-XWhMeP/images/successful_prompt.png?fit=max&auto=format&n=zNT4hiYBT-XWhMeP&q=85&s=e35994a963b60fd01691b1f5de2ffcc4" alt="Call successful prompt" data-path="images/successful_prompt.png" />
</Frame>

## Rerunning Post-Call Analysis

<Warning>
  **Important**: Rerunning analysis will incur charges for *all* models, including those that were free during the initial post-call/post-chat run.
</Warning>

After updating your prompts, you can rerun the post-call analysis to generate new outputs based on the revised instructions.

<Frame>
  <img width="500" src="https://mintcdn.com/retellai/Uc_NXIkdu9vDxEeh/images/rerun_analysis.png?fit=max&auto=format&n=Uc_NXIkdu9vDxEeh&q=85&s=3054d00243ab657251d21b56a3124f94" alt="Rerun analysis button" data-path="images/rerun_analysis.png" />
</Frame>

<Frame>
  <img width="500" src="https://mintcdn.com/retellai/Uc_NXIkdu9vDxEeh/images/rerun_analysis_modal.png?fit=max&auto=format&n=Uc_NXIkdu9vDxEeh&q=85&s=d95bc11aca9d3f1be089c754b38720b7" alt="Rerun analysis result" data-path="images/rerun_analysis_modal.png" />
</Frame>

Below is an example of a regenerated summary using the prompt: `Write a 1–5 word summary of the call.`

<Frame>
  <img width="500" src="https://mintcdn.com/retellai/Uc_NXIkdu9vDxEeh/images/rerun_analysis_new_summary.png?fit=max&auto=format&n=Uc_NXIkdu9vDxEeh&q=85&s=c775b9072c4e2025825cd2d36a674fa9" alt="New summary of rerunning prompt" data-path="images/rerun_analysis_new_summary.png" />
</Frame>

## Batch Rerun from Call/Chat History

You can rerun post-call or post-chat analysis in bulk from [Call History or Chat History](/features/session-history). Use this after updating analysis prompts to regenerate results across past sessions.

<Steps>
  <Step title="Open the Actions menu">
    On the **Call History** or **Chat History** page, click the **Actions** button in the top-right corner.
  </Step>

  <Step title="Select Backfill from Post-Call Data">
    Choose **Backfill from Post-Call Data** (or **Backfill from Post-Chat Data** on the Chat History page) from the dropdown.
  </Step>

  <Step title="Configure filters">
    The backfill modal lets you scope which sessions to rerun analysis on. Add one or more filter criteria to narrow down the sessions. You can add multiple filter rows to combine criteria. Click **Add** to add another filter row.
  </Step>

  <Step title="Start the backfill">
    Click **Save** to queue the batch rerun. The system will re-extract post-call/chat analysis data for all matching sessions using the latest draft agent's analysis prompts.
  </Step>
</Steps>

**Important**: Batch rerun will incur charges. The cost scales with the number of matching sessions.

If you have [CRM integration](/integrations/crm-overview) enabled, the re-extracted analysis data will also flow through your configured [analysis data mappings](/integrations/crm-mappings) and update the corresponding contact fields.
