> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.webhook/workflow-development.md).

# Workflow development

The [Webhook node](/integrations/builtin/core-nodes/n8n-nodes-base.webhook.md) works a bit differently from other core nodes. n8n recommends following these processes for building, testing, and using your Webhook node in production.

n8n generates two **Webhook URLs** for each Webhook node: a **Test URL** and a **Production URL**.

## Build and test workflows <a href="#build-and-test-workflows" id="build-and-test-workflows"></a>

While building or testing a workflow, use the **Test** webhook URL.

Using a test webhook ensures that you can view the incoming data in the editor UI, which is useful for debugging. Select **Listen for test event** to register the webhook before sending the data to the test webhook. The test webhook stays active for 120 seconds.

When using the Webhook node on localhost on a [self-hosted](/deploy/host-n8n.md) n8n instance, run n8n in tunnel mode:

* [npm with tunnel](/deploy/host-n8n/install-options/install-with-npm.md#n8n-with-tunnel)
* [Docker with tunnel](/deploy/host-n8n/install-options/install-with-docker.md#n8n-with-tunnel)

## Production workflows <a href="#production-workflows" id="production-workflows"></a>

When your workflow is ready, switch to using the **Production** webhook URL. You can then publish your workflow, and n8n runs it automatically when an external service calls the webhook URL.

When working with a Production webhook, ensure that you have saved and published the workflow. Data flowing through the webhook isn't visible in the editor UI with the production webhook.

Refer to [Create a workflow](/build/understand-workflows/create-and-run-workflows.md) for more information on publishing workflows.
