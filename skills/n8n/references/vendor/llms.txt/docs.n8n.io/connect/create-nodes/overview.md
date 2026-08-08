> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/connect/create-nodes/overview.md).

# Overview

Learn how to build your own custom nodes[^1].

This section includes:

* Guidance on planning your build, including [which style to use](/connect/create-nodes/plan-your-node/choose-a-node-building-style.md).
* [Tutorials](/connect/create-nodes/build-your-node.md) for different node building styles.
* Instructions for [testing your node](/connect/create-nodes/test-your-node.md), including how to use the n8n [node linter](/connect/create-nodes/test-your-node/node-linter.md) and [troubleshooting](/connect/create-nodes/test-your-node/troubleshooting.md) support.
* How to [share your node](/connect/create-nodes/deploy-your-node/submit-community-nodes.md) with the community, submit it for [verification by n8n](/connect/create-nodes/deploy-your-node/submit-community-nodes.md), or use it as a [private node](/connect/create-nodes/deploy-your-node/install-private-nodes.md).
* [Reference material](/connect/create-nodes/build-your-node/reference.md), including UI elements and information on the individual files that make up a node.

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

This section assumes the following:

* Some familiarity with JavaScript and TypeScript.
* Ability to manage your own development environment, including git.
* Knowledge of npm, including creating and submitting packages.
* Familiarity with n8n, including a good understanding of [data structures](/build/work-with-data/understand-n8ns-data-structure.md) and [item linking](/build/work-with-data/reference-data/link-data-items.md).

[^1]: In n8n, nodes are individual components that you compose to create workflows. Nodes define when the workflow should run, allow you to fetch, send, and process data, can define flow control logic, and connect with external services.
