> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Consume the analysis data

> Consume Retell post-call analysis results through the dashboard, webhooks, or API — extract structured fields and feed them into your CRM and reporting tools.

<Note>We will not populate custom post-call analysis fields for calls that were not connected or where no conversation took place. Please check whether the field exists before using it.</Note>

After a call is analyzed, you can access the analysis results through three different methods:

1. Dashboard - Visual interface for quick access and review
2. Webhook - Real-time notifications with analysis results
3. API - Programmatic access to call analysis data

### Method 1: Dashboard

Access your analysis results directly through the dashboard's history tab. This provides a user-friendly interface to:

* View all analyzed conversations
* Filter and search through analysis results

Your defined analysis categories appear in the "Conversation Analysis" column, allowing for quick insights into each conversation.

<Frame>
  <img height="200" src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/post-call-analysis-history.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=dc7c5451ebd16b48ca8fc2e007888462" alt="Post-call analysis history view" data-path="images/post-call-analysis-history.png" />
</Frame>

### Method 2: Webhook

Receive real-time notifications when call analysis is complete. The webhook payload includes:

```json theme={"dark"}
{
  "event": "call_analyzed",
  "call": {
    // Call object with call_analysis field populated
  }
}
```

To set up webhooks, visit the [Webhook Configuration](/features/webhook-overview) section.

### Method 3: Get Call API

Retrieve analysis results programmatically using the [Get Call API](/api-references/get-call).

**Example Response:**

```json theme={"dark"}
{
  "call_id": "123",
  "call_analysis": {
    // Analysis results object
  }
}
```

For detailed API documentation and response schemas, refer to the [API Reference](/api-references/get-call).

### Video: Add Post call Analysis to Excel Using Make.com

<iframe width="360" height="200" src="https://www.youtube.com/embed/qzBeNczxxFE?si=T49YHHVJ6FHRdoOu" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen />

See community templates in [docs](https://docs.google.com/document/d/1hx6hdTEjAR4y4xXZ7RLMH2byQNVW1ABxC8S4FwvTx_Y/edit?tab=t.0#heading=h.wf5bktkelope).
