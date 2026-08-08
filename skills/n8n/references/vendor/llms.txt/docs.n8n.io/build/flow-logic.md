> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/flow-logic.md).

# Flow logic

n8n allows you to represent complex logic in your workflows.

## Related sections <a href="#related-sections" id="related-sections"></a>

You need some understanding of [Data](/build/work-with-data/overview.md) in n8n, including [Data structure](/build/work-with-data/understand-n8ns-data-structure.md) and [Data flow within nodes](/build/work-with-data/understand-n8ns-data-structure.md#how-data-flows-within-nodes).

When building your logic, you'll use n8n's [Core nodes](/integrations/builtin/core-nodes.md), including:

* Splitting: [IF](/integrations/builtin/core-nodes/n8n-nodes-base.if.md) and [Switch](/integrations/builtin/core-nodes/n8n-nodes-base.switch.md).
* Merging: [Merge](/integrations/builtin/core-nodes/n8n-nodes-base.merge.md), [Compare Datasets](/integrations/builtin/core-nodes/n8n-nodes-base.comparedatasets.md), and [Code](/integrations/builtin/core-nodes/n8n-nodes-base.code.md).
* Looping: [IF](/integrations/builtin/core-nodes/n8n-nodes-base.if.md) and [Loop Over Items](/integrations/builtin/core-nodes/n8n-nodes-base.splitinbatches.md).
* Waiting: [Wait](/integrations/builtin/core-nodes/n8n-nodes-base.wait.md).
* Creating sub-workflows: [Execute Workflow](/integrations/builtin/core-nodes/n8n-nodes-base.executeworkflow.md) and [Execute Workflow Trigger](/integrations/builtin/core-nodes/n8n-nodes-base.executeworkflowtrigger.md).
* Error handling: [Stop And Error](/integrations/builtin/core-nodes/n8n-nodes-base.stopanderror.md) and [Error Trigger](/integrations/builtin/core-nodes/n8n-nodes-base.errortrigger.md).
