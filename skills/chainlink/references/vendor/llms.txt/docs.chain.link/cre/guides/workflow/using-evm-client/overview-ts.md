# EVM Chain Interactions
Source: https://docs.chain.link/cre/guides/workflow/using-evm-client/overview-ts
Last Updated: 2025-11-04

> For the complete documentation index, see [llms.txt](/llms.txt).

The `EVMClient` is the TypeScript SDK's interface for interacting with EVM-compatible blockchains. It provides a simple, powerful, and type-safe way to read data from and write data to your onchain contracts through the underlying **[EVM Read & Write Capabilities](/cre/capabilities/evm-read-write)**.

## How it works

The TypeScript SDK uses **manual ABI definitions** with <a href="https://viem.sh/" target="_blank">viem</a> for contract interactions. This approach provides:

- Type-safe ABI encoding/decoding with viem
- No code generation required—just define ABIs as TypeScript constants
- Full TypeScript type inference for contract calls
- Synchronous-looking async operations with `.result()`
- Built-in helpers like `getNetwork()`, `bytesToHex()`, and chain selectors

This approach gives you the flexibility of direct ABI handling with the type safety of TypeScript and viem's powerful utilities.

## Guides

- **[Onchain Read](/cre/guides/workflow/using-evm-client/onchain-read-ts)**: Learn how to call `view` and `pure` functions on your smart contracts to read onchain state
- **[Onchain Write](/cre/guides/workflow/using-evm-client/onchain-write/overview-ts)**: Learn how to call state-changing functions on your smart contracts to write data to the blockchain
  - **[Building Consumer Contracts](/cre/guides/workflow/using-evm-client/onchain-write/building-consumer-contracts)**: Learn how to write your own consumer contracts that can receive reports from your workflow