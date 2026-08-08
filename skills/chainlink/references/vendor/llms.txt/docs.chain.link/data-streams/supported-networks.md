# Supported Networks
Source: https://docs.chain.link/data-streams/supported-networks

> For the complete documentation index, see [llms.txt](/llms.txt).

<DataStreams section="dsNotes" />

Chainlink Data Streams provides data access directly via API or WebSocket for offchain use cases. It involves [verifying report integrity](/data-streams/tutorials/evm-onchain-report-verification) against onchain verifier proxy contracts.

The table below lists all networks supported by Data Streams, each with verifier proxy contracts deployed for onchain report verification. All [Data Streams report types](/data-streams/reference/report-schema-overview) share these verifier addresses across supported networks. Click any verifier proxy address to view the contract in the block explorer.

## Streams Trade implementation (Onchain Lookup)

[Streams Trade](/data-streams/streams-trade), the alternative implementation, allows smart contracts to access Data Streams onchain using the [`StreamsLookup`](/data-streams/getting-started) capability integrated with [Chainlink Automation](/chainlink-automation).

Streams Trade is currently available on the following networks:

- Arbitrum
- Avalanche
- Base
- BNB Chain
- Ethereum
- Optimism
- Polygon