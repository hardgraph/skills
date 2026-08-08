# Fund Your Contracts
Source: https://docs.chain.link/resources/fund-your-contract

> For the complete documentation index, see [llms.txt](/llms.txt).

Some smart contracts require funding at their addresses so they can operate without you having to call functions manually and pay for the transactions through MetaMask. This guide explains how to fund Solidity contracts with LINK or ETH.

## Retrieve the contract address

1. In Remix, deploy your contract and wait until you see a new contract in the **Deployed Contracts** section.
2. On the left side panel, use the **Copy** button located near the contract title to copy the contract address to your clipboard.

(Image: Image)

## Send funds to your contract

1. Open MetaMask.
2. Select the network that you want to send funds on. For example, select the Sepolia testnet.
3. Click the **Send** button to initiate a transaction.
4. Paste your contract address in the address field.
5. In the **Asset** drop down menu, select the type of asset that you need to send to your contract. For example, you can send LINK. If LINK is not listed, follow the guide to [Acquire testnet LINK](/resources/acquire-link).
6. In the **Amount** field, enter the amount of LINK that you want to send.
7. Click **Next** to review the transaction details and the gas cost.
8. If the transaction details are correct, click **Confirm** and wait for the transaction to process.

(Image: Image)

> **CAUTION: Transaction fee didn't update?**
>
> You may need to click **Fastest**, **Fast**, **Slow**, or **Advanced Options** after entering the **Amount** to update the gas limit for the token transfer to be successful.