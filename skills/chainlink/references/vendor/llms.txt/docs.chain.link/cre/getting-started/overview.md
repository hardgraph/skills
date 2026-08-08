# Getting Started: Overview
Source: https://docs.chain.link/cre/getting-started/overview
Last Updated: 2025-11-04

> For the complete documentation index, see [llms.txt](/llms.txt).

> **NOTE: First time here?**
>
> Before you begin, we strongly recommend reading the [**About CRE**](/cre/) page to understand the fundamental concepts
> of the platform, such as the trigger-and-callback model.

This multi-part tutorial guides you through building a complete workflow from a blank slate.

### What you'll build

You will build a simple but powerful **"Onchain Calculator"** workflow. By the end of this tutorial, your workflow will:

1. Run on a schedule using the **Cron Trigger**.
2. Fetch a random number from a public API using the **HTTP Capability**.
3. Read a value from a smart contract using the **EVM Read Capability**.
4. Combine the two values and write the final result back to the blockchain using the **EVM Write Capability**.

This tutorial is designed to teach you the core features of the CRE SDK in a logical progression. You can use any of the supported languages, Go or TypeScript using the language selector. By the end, you'll have a solid understanding of the end-to-end development process for building and simulating workflows that interact with both offchain and onchain data sources.

> **CAUTION: Educational Example Disclaimer**
>
> This page includes an educational example to use a Chainlink system, product, or service and is provided to
> demonstrate how to interact with Chainlink's systems, products, and services to integrate them into your own. This
> template is provided "AS IS" and "AS AVAILABLE" without warranties of any kind, it has not been audited, and it may be
> missing key checks or error handling to make the usage of the system, product or service more clear. Do not use the
> code in this example in a production environment without completing your own audits and application of best practices.
> Neither Chainlink Labs, the Chainlink Foundation, nor Chainlink node operators are responsible for unintended outputs
> that are generated due to errors in code.

### Where to go next?

- **[Installing the CLI](/cre/getting-started/cli-installation)**: Download and install the `cre` command-line tool.

#### Tutorial structure

- **[Part 1: Project Setup & Simulation](/cre/getting-started/part-1-project-setup)**: Initialize a new, blank CRE project and run your first "Hello World!" simulation.

- **[Part 2: Fetching Offchain Data](/cre/getting-started/part-2-fetching-data)**: Modify your workflow to fetch data from an external API using the `http.Client`.

- **[Part 3: Reading an Onchain Value](/cre/getting-started/part-3-reading-onchain-value)**: Generate contract bindings and use the `evm.Client` to read a value from the blockchain.

- **[Part 4: Writing Onchain](/cre/getting-started/part-4-writing-onchain)**: Complete the calculator by writing your computed result back to a smart contract on Sepolia.

- **[Conclusion](/cre/getting-started/conclusion)**: Review what you've learned and find resources to continue your journey.