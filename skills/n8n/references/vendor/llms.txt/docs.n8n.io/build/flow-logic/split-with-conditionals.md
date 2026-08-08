> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/flow-logic/split-with-conditionals.md).

# Split with conditionals

Splitting uses the [IF](/integrations/builtin/core-nodes/n8n-nodes-base.if.md) or [Switch](/integrations/builtin/core-nodes/n8n-nodes-base.switch.md) nodes. It turns a single-branch workflow into a multi-branch workflow. This is a key piece of representing complex logic in n8n.

Compare these workflows:

!["Diagram representing two workflows. One has three steps and follows a linear process, with a user submitting a bug, and the workflow emailing a support team. The second workflow starts the same way, but then splits depending on whether the user marked the issue as urgent. It then splits again depending on the user's support plan"](/files/esOmXpNWeeHGtfP1EoEb)

This is the power of splitting and conditional nodes in n8n.

Refer to the [IF](/integrations/builtin/core-nodes/n8n-nodes-base.if.md) or [Switch](/integrations/builtin/core-nodes/n8n-nodes-base.switch.md) documentation for usage details.
