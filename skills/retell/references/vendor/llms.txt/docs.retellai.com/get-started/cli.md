> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Retell CLI

> Install the Retell CLI to manage agents, phone numbers, knowledge bases, and other Retell resources from your terminal with simple commands.

## Overview

The Retell CLI lets you manage Retell resources from your terminal. See the [`@retell-ai/retell-cli` npm package](https://www.npmjs.com/package/@retell-ai/retell-cli) for the full CLI reference. For application code, use a [Retell SDK](/get-started/sdk).

## Get started

<Steps>
  <Step title="Install the CLI">
    Requires [Node.js](https://nodejs.org) 22 or later.

    ```bash theme={"dark"}
    npm install --global @retell-ai/retell-cli
    ```
  </Step>

  <Step title="Authenticate">
    With a Retell [API key](/accounts/manage-api-keys), run:

    ```bash theme={"dark"}
    retell auth login
    ```

    Enter your API key when prompted.
  </Step>

  <Step title="Run a command">
    For example, list your agents:

    ```bash theme={"dark"}
    retell list-agents
    ```

    Run `retell --help` to see all available commands.
  </Step>
</Steps>
