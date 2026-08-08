> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/workflows.md).

# Workflows

{% hint style="info" %}
**File-based configuration**

You can add `_FILE` to individual variables to provide their configuration in a separate file. Refer to [Keeping sensitive data in separate files](/deploy/host-n8n/configure-n8n/basic-configuration.md#keeping-sensitive-data-in-separate-files) for more details.
{% endhint %}

| Variable                                    | Type    | Default                  | Description                                                                                                                                                                                              |
| ------------------------------------------- | ------- | ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `N8N_ONBOARDING_FLOW_DISABLED`              | Boolean | `false`                  | Whether to disable onboarding tips when creating a new workflow (true) or not (false).                                                                                                                   |
| `N8N_WORKFLOW_ACTIVATION_BATCH_SIZE`        | Number  | `1`                      | How many workflows to publish simultaneously during startup.                                                                                                                                             |
| `N8N_WORKFLOW_CALLER_POLICY_DEFAULT_OPTION` | String  | `workflowsFromSameOwner` | Which workflows can call a workflow. Options are: `any`, `none`, `workflowsFromAList`, `workflowsFromSameOwner`. This feature requires [Workflow sharing](/build/manage-workflows/share-with-others.md). |
| `N8N_WORKFLOW_TAGS_DISABLED`                | Boolean | `false`                  | Whether to disable workflow tags (true) or enable tags (false).                                                                                                                                          |
| `WORKFLOWS_DEFAULT_NAME`                    | String  | `My workflow`            | The default name used for new workflows.                                                                                                                                                                 |
