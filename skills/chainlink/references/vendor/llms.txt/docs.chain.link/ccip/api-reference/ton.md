# CCIP Contract Interface Reference Documentation (TON)
Source: https://docs.chain.link/ccip/api-reference/ton

> For the complete documentation index, see [llms.txt](/llms.txt).

<Aside>TON currently supports arbitrary message passing only. Token transfers are not yet supported.</Aside>

## Available Versions

### Latest Release

- **[CCIP v1.6.0](/ccip/api-reference/ton/v1.6.0)** (Current Version)
  - Initial release for the TON blockchain
  - Cross-chain arbitrary messaging between TON and EVM-based chains
  - Native GRAM fee payments (nanoGRAM)
  - Configurable gas limits and out-of-order execution

## Documentation Structure

Each version includes detailed documentation for:

- Message Structures and Extra Args
- Router Interface (sending messages and fee estimation)
- Receiver Interface (receiving CCIP messages)
- Events
- Error Codes

## Starter Kit Helpers

The [TON Starter Kit Helper Reference](/ccip/api-reference/ton/starter-kit-helpers) documents the unaudited TypeScript utilities bundled with the TON Starter Kit. These are off-chain helpers for constructing messages and estimating fees — they are not part of the audited protocol contracts.