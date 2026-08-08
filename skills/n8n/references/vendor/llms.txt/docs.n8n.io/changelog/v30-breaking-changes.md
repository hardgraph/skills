> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/changelog/v30-breaking-changes.md).

# v3.0 Breaking changes

This document highlights breaking changes and actions to prepare for the upcoming transition to n8n v3.0, scheduled for October 2026. These updates improve security, simplify configuration, and remove legacy features.

The release of n8n 3.0 continues n8n's commitment to providing a secure, reliable, and production-ready automation platform. This major version includes important security enhancements and cleanup of deprecated features.

## Deployment <a href="#deployment" id="deployment"></a>

### Docker-based deployment required for self-hosted n8n <a href="#docker-based-deployment-required-for-self-hosted-n8n" id="docker-based-deployment-required-for-self-hosted-n8n"></a>

* Self-hosted n8n will require a Docker-based deployment. Installations run via `npm` / `npx n8n` will no longer be supported.
* **What to do:** If you currently run n8n with `npm` or `npx n8n`, plan a move to a Docker-based deployment before upgrading to v3. For local installations, Docker Compose is expected to be the easiest path.
* *Step-by-step migration guidance will be coming soon*

## Removed nodes and helpers <a href="#removed-nodes-and-helpers" id="removed-nodes-and-helpers"></a>

Older nodes, modes, and helpers that have been replaced by newer patterns are being removed in v3.

### Removed nodes <a href="#removed-nodes" id="removed-nodes"></a>

* **Function** node (legacy)
* **Function Item** node (legacy)
* **Item Lists** node (legacy)
* **What to do:** Migrate affected workflows to the current recommended alternatives before upgrading:
  * Replace **Function** and **Function Item** nodes with the [Code](/integrations/builtin/core-nodes/n8n-nodes-base.code.md) node. Use **Run Once for All Items** mode in place of **Function**, and **Run Once for Each Item** mode in place of **Function Item**.
  * Replace the **Item Lists** node with the node matching the operation you use: [Split Out](/integrations/builtin/core-nodes/n8n-nodes-base.splitout.md), [Aggregate](/integrations/builtin/core-nodes/n8n-nodes-base.aggregate.md), [Sort](/integrations/builtin/core-nodes/n8n-nodes-base.sort.md), [Limit](/integrations/builtin/core-nodes/n8n-nodes-base.limit.md), [Remove Duplicates](/integrations/builtin/core-nodes/n8n-nodes-base.removeduplicates.md), or [Summarize](/integrations/builtin/core-nodes/n8n-nodes-base.summarize.md).

### Changed node behavior <a href="#changed-node-behavior" id="changed-node-behavior"></a>

* **Execute Workflow** node: older behavior is being removed.

### Removed expression helpers <a href="#removed-expression-helpers" id="removed-expression-helpers"></a>

* The deprecated `$getPairedItem` expression helper is being removed.
* **What to do:** Use n8n's standard [item linking](/build/work-with-data/reference-data/link-data-items/how-items-link-through-workflows.md) instead, for example the `pairedItem` property or `$("<node-name>").item`.

## Security <a href="#security" id="security"></a>

Security defaults are getting stronger to make n8n safer by default. These changes may affect existing workflows or credentials.

* **Tighter handling of risky resource names.**
* **More secure credential behavior.**
* **Key rotation enabled by default.**

## Retired capabilities <a href="#retired-capabilities" id="retired-capabilities"></a>

Some legacy or lower-usage product capabilities are being retired in v3. Guidance will be provided where a migration path or alternative exists.

* **Chat hub** — being retired.
* **Workflow import from URL in the editor** — being removed. Other [import methods](/build/manage-workflows/export-and-import.md) remain supported: copy-paste, **Import from File** in the editor UI menu, the CLI, and the Public API.
* **Non-functional nodes** — being removed.

***

*This page will be updated with full details, migration guides, and links as v3 approaches its release.*
