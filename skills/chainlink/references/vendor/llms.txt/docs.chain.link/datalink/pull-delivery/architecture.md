# DataLink Architecture (Pull Delivery)
Source: https://docs.chain.link/datalink/pull-delivery/architecture

> For the complete documentation index, see [llms.txt](/llms.txt).

DataLink (Pull Delivery) provides infrastructure for Data Providers to make their specialized data available onchain for consumption by blockchain applications through offchain retrieval with onchain verification capabilities.

The process involves these core steps and components, mirroring the [Data Streams](/data-streams/architecture) architecture but adapted for DataLink feeds:

(Image: Image)

1. **Data Provider Connection:**
   - Chainlink Labs curates and onboards premium Data Providers.
   - The provider makes their proprietary data available via a secure API endpoint.

2. **Data Fetching & Consensus (Data DON):**
   - Multiple nodes within a Chainlink Decentralized Oracle Network (DON) independently fetch data from the provider's API.
   - Nodes reach consensus on the data received from the provider.
   - The DON generates a cryptographically signed oracle report containing the provider's data.

3. **Report Availability (Chainlink Aggregation Layer):**
   - The signed report from the DON is sent to the Chainlink Aggregation Layer.
   - This layer stores the reports and makes them available via low-latency REST and WebSocket APIs, leveraging an [active-active multi-site deployment](/data-streams/architecture#active-active-multi-site-deployment) for high availability.

4. **Application Integration:**
   - **dApp Integration:** Developers fetch the signed reports offchain using the standard Streams Direct [REST API](/data-streams/reference/streams-direct/streams-direct-interface-api), [WebSocket](/data-streams/reference/streams-direct/streams-direct-interface-ws), or [SDKs](/data-streams/reference/streams-direct/streams-direct-go-sdk).
   - **Onchain Verification:** Developers can use the [Chainlink Verifier Contracts](/datalink/pull-delivery/verifier-proxy-addresses) (the same Verifier Contracts used by Data Streams) to [verify the report's integrity onchain](/datalink/pull-delivery/tutorials/onchain-verification-evm), ensuring it hasn't been tampered with since being signed by the DON.

DataLink utilizes the robust transport (Aggregation Layer) and verification (Verifier Contract) components of the Streams Direct infrastructure, but focuses the DON's role on securely fetching and signing data from a *single provider*.