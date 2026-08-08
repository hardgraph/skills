# CCIP Billing
Source: https://docs.chain.link/ccip/billing
Last Updated: 2025-05-19

> For the complete documentation index, see [llms.txt](/llms.txt).

> **NOTE: Prerequisites**
>
> Read the CCIP [Introduction](/ccip) and [Concepts](/ccip/concepts) to understand all the concepts discussed on this
> page.

The CCIP billing model uses the `feeToken` specified in the [message](/ccip/api-reference/evm/v1.6.1/client#evm2anymessage) to pay a single fee on the source blockchain. CCIP uses a gas-locked fee payment mechanism to help ensure the reliable execution of cross-chain transactions regardless of destination blockchain gas spikes. For developers, this means you can simply pay on the source blockchain and CCIP will take care of execution on the destination blockchain.

CCIP supports fee payments in LINK and in alternative assets, including blockchain-native gas tokens and their ERC-20 wrapped versions. The payment model for CCIP is designed to significantly reduce friction for users and quickly scale CCIP to more blockchains by supporting fee payments that originate across a multitude of blockchains over time.

Aside from billing, remember to [carefully estimate the `gasLimit` that you set](/ccip/concepts/best-practices/evm#setting-gaslimit) for your destination contract so CCIP can have enough gas to execute `ccipReceive()`, if applicable. Any unspent gas from this user-set limit is not refunded.

## Billing mechanism

The fee is calculated by the following formula:

```
fee = blockchain fee + network fee
```

Where:

- `fee`: The total fee for processing a [CCIP message](/ccip/api-reference/evm/v1.6.1/client#evm2anymessage). **Note:** Users can call the [getFee](/ccip/api-reference/evm/v1.6.1/i-router-client#getfee) function to estimate the fee.
- `blockchain fee`: This represents an estimation of the gas cost the node operators will pay to deliver the CCIP message to the destination blockchain.
- `network fee`: The fee paid to RMN node operators running the [Decentralized Oracle Network](/ccip/concepts/architecture/key-concepts#decentralized-oracle-network-don).

### Blockchain fee

The blockchain fee is calculated by the following formula:

```
blockchain fee = execution cost + data availability cost
```

#### Execution cost

The execution cost is directly correlated with the estimated gas usage to execute the transaction on the destination blockchain:

```
execution cost = gas price * gas usage * gas multiplier
```

Where:

- `gas price`: The destination gas price. CCIP maintains a cache of destination gas prices on each source blockchain, denominated in each `feeToken`.
- `gas multiplier`: Scaling factor. This multiplier ensures the reliable execution of transactions regardless of destination blockchain gas spikes.
- `gas usage`:

  ```
  gas usage = gas limit + destination gas overhead + destination gas per payload + gas for token transfers`
  ```

  Where:

  - `gas limit`: This specifies the maximum amount of gas CCIP can consume to execute [ccipReceive()](/ccip/api-reference/evm/v1.6.1/ccip-receiver#ccipreceive) on the receiver contract located on the destination blockchain. Users set the gas limit in the [extra argument field](/ccip/api-reference/evm/v1.6.1/client#genericextraargsv2) of the CCIP message. **Note:** Remember to [carefully estimate the `gasLimit` that you set](/ccip/concepts/best-practices/evm#setting-gaslimit) for your destination contract so CCIP can have enough gas to execute `ccipReceive()`. Any unspent gas from this user-set limit is not refunded.
  - `destination gas overhead`: This is the fixed gas cost incurred on the destination blockchain by CCIP (Committing DON + Executing DON) and Risk Management Network.
  - `destination gas per payload`: This variable gas depends on the length of the data field in the [CCIP message](/ccip/api-reference/evm/v1.6.1/client#evm2anymessage). If there is no payload (CCIP only transfers tokens), the value is `0`.
  - `gas for token transfers`: This variable gas cost is for transferring tokens onto the destination blockchain. If there are no token transfers, the value is `0`.

#### Data availability cost

This cost is only relevant if the destination blockchain is a [L2 layer](https://chain.link/education-hub/what-is-layer-2). Some L2s charge fees for [data availability](https://ethereum.org/en/developers/docs/data-availability). For instance, [optimistic rollups](https://ethereum.org/en/developers/docs/scaling/optimistic-rollups/) process the transactions offchain then post the transaction data to Ethereum as calldata, which costs additional gas.

### Network fee

The fee paid to node operators running the [Decentralized Oracle Network](/ccip/concepts/architecture/key-concepts#decentralized-oracle-network-don):

#### Token transfers or programmable token transfers

For token transfers or programmable token transfers (token + data), the network fee varies based on the [token handling mechanism](/ccip/concepts/cross-chain-token/overview#token-handling-mechanisms) and the lanes:

- **Lock and Unlock**: The network fee is percentage-based. For each token, it is calculated using the following expression:

  ```
  tokenAmount * price * percentage
  ```

  Where:

  - `tokenAmount`: The amount of tokens being transferred.
  - `price`: Initially priced in USD and converted into the `feeToken`.
  - `percentage`: The values are provided in the [network fee table](#network-fee-table).

- **Lock and Mint**, **Burn and Mint** and **Burn and Unlock**: The network fee is a static amount. See the [network fee table](#network-fee-table).

> **NOTE: Determine Token Handling Mechanism**
>
> Use the [calculator](#network-token-calculator) below or consult the CCIP Directory on the
> [mainnet](/ccip/directory/mainnet) or (/ccip/directory/testnet) pages to determine a token's handling mechanism on a
> given lane.

#### Messaging (only data)

For messaging (only data): The network fee is a static amount, denominated in USD. See the [network fee table](#network-fee-table).

#### Network fee table

The table below provides an overview of the network fees charged for different use cases on different lanes. Percentage-based fees are calculated on the value transferred in a message. USD-denominated fees are applied per message.

> **Note:** The following applies to **Token Transfers** and **Programmable Token Transfers**, not **Messaging**:
>
> - In the table below, "Not Ethereum" on the **source** side includes Solana; on the **destination** side it excludes Solana.
> - When Solana is the destination, an additional 0.10 USD fee applies for [ATA](https://solana.com/docs/tokens#associated-token-account) generation.

| Use case                                       | Token Pool Mechanism                            | Source Chain | Destination Chain | LINK      | Others   |
| ---------------------------------------------- | ----------------------------------------------- | ------------ | ----------------- | --------- | -------- |
| Token Transfers / Programmable Token Transfers | Lock and Unlock                                 | All Chains   | All Chains        | 0.045 %   | 0.05 %   |
| Token Transfers / Programmable Token Transfers | Lock and Mint / Burn and Mint / Burn and Unlock | Ethereum     | Not Ethereum      | 0.45 USD  | 0.50 USD |
| Token Transfers / Programmable Token Transfers | Lock and Mint / Burn and Mint / Burn and Unlock | Ethereum     | Solana            | 0.54 USD  | 0.60 USD |
| Token Transfers / Programmable Token Transfers | Lock and Mint / Burn and Mint / Burn and Unlock | Not Ethereum | Solana            | 0.315 USD | 0.35 USD |
| Token Transfers / Programmable Token Transfers | Lock and Mint / Burn and Mint / Burn and Unlock | Not Ethereum | Ethereum          | 1.35 USD  | 1.50 USD |
| Token Transfers / Programmable Token Transfers | Lock and Mint / Burn and Mint / Burn and Unlock | Not Ethereum | Not Ethereum      | 0.225 USD | 0.25 USD |
| Messaging                                      | N/A                                             | Ethereum     | Not Ethereum      | 0.45 USD  | 0.50 USD |
| Messaging                                      | N/A                                             | Ethereum     | Solana            | 0.45 USD  | 0.50 USD |
| Messaging                                      | N/A                                             | Not Ethereum | Solana            | 0         | 0        |
| Messaging                                      | N/A                                             | Not Ethereum | Ethereum          | 0.45 USD  | 0.50 USD |
| Messaging                                      | N/A                                             | Not Ethereum | Not Ethereum      | 0.09 USD  | 0.10 USD |

You can use the calculator below to learn the network fees for a specific token. Select the environment (mainnet/testnet), the token, the source blockchain, and the destination blockchain to get the network fee:

<TokenCalculator client:idle />