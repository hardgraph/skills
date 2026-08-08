> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/configure-n8n/scaling.md).

# Scaling

When running n8n at scale, with a large number of users, workflows, or executions, you need to change your n8n configuration to ensure good performance.

n8n can run in different [modes](/deploy/host-n8n/configure-n8n/scaling/enable-queue-mode.md) depending on your needs. The `queue` mode provides the best scalability. Refer to [Queue mode](/deploy/host-n8n/configure-n8n/scaling/enable-queue-mode.md) for configuration details.

You can configure data saving and pruning to improve database performance. Refer to [Execution data](/deploy/host-n8n/configure-n8n/scaling/manage-execution-data.md) for details.
