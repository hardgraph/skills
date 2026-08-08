# SmartData
Source: https://docs.chain.link/data-feeds/smartdata

> For the complete documentation index, see [llms.txt](/llms.txt).

Chainlink SmartData is a suite of onchain data offerings designed to unlock the utility, accessibility, and reliability of tokenized real-world assets (RWAs). By providing secure minting assurances alongside essential real-world data such as reserves, Net Asset Value (NAV), and Assets Under Management (AUM) data, the SmartData suite embeds security and enriches data into tokenized RWA offerings.

## SmartData Feed Types

SmartData offers two distinct types of data feeds:

1. **Single-value SmartData Feeds**: Similar to traditional price feeds, these provide a single numeric value per feed (like total reserves or NAV). These use the [`AggregatorV3Interface`](/data-feeds/api-reference#aggregatorv3interface) and are read the same way as other Data Feeds.

2. **[Multiple-Variable Response (MVR) Feeds](/data-feeds/mvr-feeds)**: These bundle multiple data points of various types (both numeric and non-numeric) into a single onchain update. MVR feeds use the [`BundleAggregatorProxy` interface](/data-feeds/mvr-feeds/api-reference#ibundleaggregatorproxy) and require a different approach to read and decode the data.

## SmartData Product Categories

- [Proof of Reserve Feeds](#proof-of-reserve-feeds)
- [NAVLink Feeds](#navlink-feeds)
- [SmartAUM Feeds](#smartaum-feeds)

### Proof of Reserve Feeds

Proof of Reserves feeds provide the status of reserves for stablecoins, wrapped assets, and real world assets. Proof of Reserve Feeds operate similarly to Price Feeds, but provide answers in units of measurement such as ounces (oz) or number of tokens.

To find a list of available Proof of Reserve Feeds, see the [SmartData Feed Addresses](/data-feeds/smartdata/addresses) page.

#### Types of Proof of Reserve Feeds

Reserves are available for both offchain assets and cross-chain assets. This categorization describes the data reporting variations of Proof of Reserve feeds and helps highlight some of the inherent market risks surrounding the data quality of these feeds.

##### Offchain reserves

Offchain reserves are sourced from APIs through an [external adapter](/chainlink-nodes/external-adapters/external-adapters).

(Image: Image)

Offchain reserves provide their data using the following methods:

- Third-party: An auditor, accounting firm, or other third party audits and verifies reserves. This is done by combining both fiat and investment assets into a numeric value that is reported against the token.
- Custodian: Reserves data are pulled directly from the bank or custodian. The custodian has direct access to the bank or vault holding the assets. Generally, this works when the underlying asset pulled requires no additional valuation and is simply reported onchain.
- ⚠️ Self-reported: Reserve data is read from an API that the token issuer hosts. Reserve data reported by an asset issuer's self-hosted API carries additional risks. Chainlink Labs is not responsible for the accuracy of self-reported reserves data. Users must do their own risk assessment for asset issuer risk.

##### Cross-chain reserves

Cross-chain reserves are sourced from the network where the reserves are held. Chainlink node operators can report cross-chain reserves by running an [external adapter](/chainlink-nodes/external-adapters/external-adapters) and querying the source-chain client directly. In some instances, the reserves are composed of a dynamic list of IDs or addresses using a composite adapter.

(Image: Image)

Cross-chain reserves provide their data using the following methods:

- Wallet address manager: The project uses the [IPoRAddressList](https://github.com/smartcontractkit/chainlink/blob/contracts-v1.3.0/contracts/src/v0.8/interfaces/PoRAddressList.sol) wallet address manager contract and self-reports which addresses they own. Reserve data reported by an asset issuer's self-reported addresses carries additional risks. Chainlink Labs is not responsible for the accuracy of self-reported reserves data. Users must do their own risk assessment for asset issuer risk.
- Wallet address: The project reports which addresses they own through a self-hosted API. Reserve data reported by an asset issuer's self-reported addresses carries additional risks. Chainlink Labs is not responsible for the accuracy of self-reported reserves data. Users must do their own risk assessment for asset issuer risk.

> \*\*CAUTION: Disclaimer\*\*
>
>
>
> <p>
>   Proof of Reserve feeds can vary in their configurations. Please be careful with the configuration of the feeds used
>   by your smart contracts. You are solely responsible for reviewing the quality of the data (e.g., a Proof of Reserve
>   feed) that you integrate into your smart contracts and assume full responsibility for any damage, injury, or any
>   other loss caused by your use of the feeds used by your smart contracts.
> </p>

### NAVLink Feeds

Chainlink NAVLink Feeds provide real-time, tamper-proof data on the Net Asset Value (NAV) of tokenized assets, funds, or portfolios. NAV is an essential metric in the financial industry for assessing the value of mutual funds, ETFs, and other investment vehicles. It is calculated by subtracting total liabilities from the total assets held within the vehicle.

By making NAV data available onchain, developers can build decentralized applications that require accurate and up-to-date valuation metrics. These applications include asset management platforms, DeFi protocols, and investment strategies that rely on NAV for operations such as rebalancing, minting, or redemption.

To find a list of available SmartNav Feeds, see the [SmartData Feed Addresses](/data-feeds/smartdata/addresses) page.

### SmartAUM Feeds

Chainlink SmartAUM Feeds provide current data on the total market value of assets managed by an entity on behalf of clients. Assets Under Management (AUM) is a crucial indicator used in financial analyses and decision-making processes.

By bringing AUM data onchain, decentralized applications can access information for activities such as risk assessment, performance benchmarking, and investment strategy development.

To find a list of available Assets Under Management Feeds, see the [SmartData Feed Addresses](/data-feeds/smartdata/addresses) page.

## Using SmartData Feeds

### Using Single-value SmartData Feeds

Read answers from single-value SmartData feeds the same way that you read other Data Feeds. Specify the [SmartData feed address](/data-feeds/smartdata/addresses) that you want to read instead of specifying a Price feed address. See the [Using Data Feeds](/data-feeds/using-data-feeds) page to learn more.

Using Solidity, your smart contract should reference [`AggregatorV3Interface`](https://github.com/smartcontractkit/chainlink/blob/contracts-v1.3.0/contracts/src/v0.8/shared/interfaces/AggregatorV3Interface.sol), which defines the external functions implemented by Data Feeds.

Example for reading a Proof of Reserve feed:

```sol
// SPDX-License-Identifier: MIT
pragma solidity 0.8.24;

import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/shared/interfaces/AggregatorV3Interface.sol";

contract ReserveConsumerV3 {
  AggregatorV3Interface internal reserveFeed;

  /**
   * Network: Ethereum Mainnet
   * Aggregator: WBTC PoR
   * Address: 0xa81FE04086865e63E12dD3776978E49DEEa2ea4e
   */
  constructor() {
    reserveFeed = AggregatorV3Interface(0xa81FE04086865e63E12dD3776978E49DEEa2ea4e);
  }

  /**
   * Returns the latest price
   */
  function getLatestReserve() public view returns (int256) {
    // prettier-ignore
    (
      /*uint80 roundID*/
      ,
      int256 reserve,
      /*uint startedAt*/
      ,
      /*uint timeStamp*/
      ,
      /*uint80 answeredInRound*/
    ) = reserveFeed.latestRoundData();

    return reserve;
  }
}
```

### Using MVR Feeds

[MVR feeds](/data-feeds/mvr-feeds) require a different approach compared to single-value SmartData feeds. Instead of returning a single numeric value, they return a bytes array that must be decoded into a specific data structure.

Your code needs to:

1. Call the `latestBundle()` function to get the raw data
2. Decode the bytes array into a struct that matches the feed's structure
3. Apply the appropriate decimal scaling to numeric fields

For detailed implementation guides, see:

- [Using MVR Feeds on EVM Chains (Solidity)](/data-feeds/mvr-feeds/guides/evm-solidity)
- [Using MVR Feeds with ethers.js (JS)](/data-feeds/mvr-feeds/guides/ethersjs)
- [Using MVR Feeds with Viem (TS)](/data-feeds/mvr-feeds/guides/viem)