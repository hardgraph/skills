> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/connect/create-nodes/build-your-node/set-up-your-development-environment.md).

# Set up your development environment

This document lists the essential dependencies for developing a node, as well as guidance on setting up your editor.

## Requirements <a href="#requirements" id="requirements"></a>

To build and test a node, you need:

* Node.js and npm. Minimum version Node 22.22.0. You can find instructions on how to install both using nvm (Node Version Manager) for Linux, Mac, and WSL (Windows Subsystem for Linux) [here](https://github.com/nvm-sh/nvm). For Windows users, refer to Microsoft's guide to [Install NodeJS on Windows](https://docs.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-windows).
* A local instance of n8n. You can install n8n with `npm install n8n -g`, then follow the steps in [Run your node locally](/connect/create-nodes/test-your-node/run-your-node-locally.md) to test your node.
* When [building verified community nodes](/integrations/community-nodes/building-community-nodes.md), you must use the [`n8n-node` tool](/connect/create-nodes/build-your-node/using-the-n8n-node-tool.md) to create and test your node.

You should also have [git](https://git-scm.com/) installed. This allows you to clone and use the [n8n-node-starter](https://github.com/n8n-io/n8n-nodes-starter).

## Editor setup <a href="#editor-setup" id="editor-setup"></a>

n8n recommends using [VS Code](https://code.visualstudio.com/) as your editor.

Install these extensions:

* [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
* [EditorConfig](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)
* [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

By using VS Code and these extensions, you get access to the n8n node linter's warnings as you code.
