# Transfer Tokens Between Chains from Smart Contracts
Source: https://docs.chain.link/ccip/tutorials/evm/transfer-tokens-from-contract
Last Updated: 2025-05-19

> For the complete documentation index, see [llms.txt](/llms.txt).

In this tutorial, you will use Chainlink CCIP to transfer tokens from a smart contract to an account on a different blockchain. First, you will pay for the CCIP fees on the source blockchain using LINK. Then, you will use the same contract to pay CCIP fees in native gas tokens. For example, you would use ETH on Ethereum or AVAX on Avalanche.

> **NOTE: Node Operator Rewards**
>
> CCIP rewards the oracle node in LINK.

> **CAUTION: Transferring tokens**
>
> This tutorial uses the term "transferring tokens" even though the tokens are not technically transferred. Instead,
> they are locked or burned on the source chain and then unlocked or minted on the destination chain. Read the [Token
> Pools](/ccip/concepts/cross-chain-token/evm/token-pools) section to understand the various mechanisms that are used to
> transfer value across chains.

## Before you begin

1. You should understand how to write, compile, deploy, and fund a smart contract. If you need to brush up on the basics, read this [tutorial](/quickstarts/deploy-your-first-contract), which will guide you through using the [Solidity programming language](https://soliditylang.org/), interacting with the [MetaMask wallet](https://metamask.io) and working within the [Remix Development Environment](https://remix.ethereum.org/).

2. Your account must have some AVAX and LINK tokens on *Avalanche Fuji*. Learn how to [Acquire testnet LINK](/resources/acquire-link).

3. Check the [CCIP Directory](/ccip/directory) to confirm that the tokens you will transfer are supported for your lane. In this example, you will transfer tokens from *Avalanche Fuji* to *Ethereum Sepolia* so check the list of supported tokens [here](/ccip/directory/testnet/chain/avalanche-fuji-testnet).

4. Learn how to [acquire CCIP test tokens](/ccip/test-tokens#evm-chains). Following this guide, you should have CCIP-BnM tokens, and CCIP-BnM should appear in the list of your tokens in MetaMask.

5. Learn how to [fund your contract](/resources/fund-your-contract). This guide shows how to fund your contract in LINK, but you can use the same guide to fund your contract with any ERC20 tokens as long as they appear in the list of tokens in MetaMask.

## Tutorial

> **NOTE: Optimize your development with the CCIP local simulator**
>
> Enhance your development workflow using the [Chainlink CCIP local
> simulator](https://github.com/smartcontractkit/chainlink-local), an installable package designed to simulate Chainlink
> CCIP locally within your Hardhat and Foundry projects. It provides a robust smart contracts and scripts suite,
> enabling you to build, deploy, and execute CCIP token transfers and arbitrary messages on a local Hardhat or Anvil
> development node. With Chainlink Local, you can also work on forked nodes, ensuring a seamless transition of your
> contracts to test networks without modifications. Start integrating Chainlink Local today to streamline your
> development process and validate your CCIP implementations effectively.

In this tutorial, you will transfer [CCIP-BnM](/ccip/test-tokens#about-ccip-test-tokens) tokens from a contract on Avalanche Fuji to an account on Ethereum Sepolia. First, you will pay [CCIP fees in LINK](#transfer-tokens-and-pay-in-link), then you will pay [CCIP fees in native gas](#transfer-tokens-and-pay-in-native). The destination account can be an [EOA (Externally Owned Account)](https://ethereum.org/en/developers/docs/accounts/#types-of-account) or a smart contract. Moreover, the example shows how to transfer CCIP-BnM tokens, but you can re-use the same example to transfer other tokens as long as they are supported for your [lane](/ccip/concepts/architecture/key-concepts#lane).

```sol
// SPDX-License-Identifier: MIT
pragma solidity 0.8.24;

import {IRouterClient} from "@chainlink/contracts-ccip/contracts/interfaces/IRouterClient.sol";

import {Client} from "@chainlink/contracts-ccip/contracts/libraries/Client.sol";
import {OwnerIsCreator} from "@chainlink/contracts@1.4.0/src/v0.8/shared/access/OwnerIsCreator.sol";
import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

/**
 * THIS IS AN EXAMPLE CONTRACT THAT USES HARDCODED VALUES FOR CLARITY.
 * THIS IS AN EXAMPLE CONTRACT THAT USES UN-AUDITED CODE.
 * DO NOT USE THIS CODE IN PRODUCTION.
 */

/// @title - A simple contract for transferring tokens across chains.
contract TokenTransferor is OwnerIsCreator {
  using SafeERC20 for IERC20;

  // Custom errors to provide more descriptive revert messages.
  error NotEnoughBalance(uint256 currentBalance, uint256 requiredBalance); // Used to make sure contract has enough
  // token balance
  error NothingToWithdraw(); // Used when trying to withdraw Ether but there's nothing to withdraw.
  error FailedToWithdrawEth(address owner, address target, uint256 value); // Used when the withdrawal of Ether fails.
  error DestinationChainNotAllowlisted(uint64 destinationChainSelector); // Used when the destination chain has not been
  // allowlisted by the contract owner.
  error InvalidReceiverAddress(); // Used when the receiver address is 0.
  // Event emitted when the tokens are transferred to an account on another chain.

  // The chain selector of the destination chain.
  // The address of the receiver on the destination chain.
  // The token address that was transferred.
  // The token amount that was transferred.
  // the token address used to pay CCIP fees.
  // The fees paid for sending the message.
  event TokensTransferred( // The unique ID of the message.
    bytes32 indexed messageId,
    uint64 indexed destinationChainSelector,
    address receiver,
    address token,
    uint256 tokenAmount,
    address feeToken,
    uint256 fees
  );

  // Mapping to keep track of allowlisted destination chains.
  mapping(uint64 => bool) public allowlistedChains;

  IRouterClient private s_router;

  IERC20 private s_linkToken;

  /// @notice Constructor initializes the contract with the router address.
  /// @param _router The address of the router contract.
  /// @param _link The address of the link contract.
  constructor(
    address _router,
    address _link
  ) {
    s_router = IRouterClient(_router);
    s_linkToken = IERC20(_link);
  }

  /// @dev Modifier that checks if the chain with the given destinationChainSelector is allowlisted.
  /// @param _destinationChainSelector The selector of the destination chain.
  modifier onlyAllowlistedChain(
    uint64 _destinationChainSelector
  ) {
    if (!allowlistedChains[_destinationChainSelector]) {
      revert DestinationChainNotAllowlisted(_destinationChainSelector);
    }
    _;
  }

  /// @dev Modifier that checks the receiver address is not 0.
  /// @param _receiver The receiver address.
  modifier validateReceiver(
    address _receiver
  ) {
    if (_receiver == address(0)) revert InvalidReceiverAddress();
    _;
  }

  /// @dev Updates the allowlist status of a destination chain for transactions.
  /// @notice This function can only be called by the owner.
  /// @param _destinationChainSelector The selector of the destination chain to be updated.
  /// @param allowed The allowlist status to be set for the destination chain.
  function allowlistDestinationChain(
    uint64 _destinationChainSelector,
    bool allowed
  ) external onlyOwner {
    allowlistedChains[_destinationChainSelector] = allowed;
  }

  /// @notice Transfer tokens to receiver on the destination chain.
  /// @notice pay in LINK.
  /// @notice the token must be in the list of supported tokens.
  /// @notice This function can only be called by the owner.
  /// @dev Assumes your contract has sufficient LINK tokens to pay for the fees.
  /// @param _destinationChainSelector The identifier (aka selector) for the destination blockchain.
  /// @param _receiver The address of the recipient on the destination blockchain.
  /// @param _token token address.
  /// @param _amount token amount.
  /// @return messageId The ID of the message that was sent.
  function transferTokensPayLINK(
    uint64 _destinationChainSelector,
    address _receiver,
    address _token,
    uint256 _amount
  )
    external
    onlyOwner
    onlyAllowlistedChain(_destinationChainSelector)
    validateReceiver(_receiver)
    returns (bytes32 messageId)
  {
    // Create an EVM2AnyMessage struct in memory with necessary information for sending a cross-chain message
    //  address(linkToken) means fees are paid in LINK
    Client.EVM2AnyMessage memory evm2AnyMessage = _buildCCIPMessage(_receiver, _token, _amount, address(s_linkToken));

    // Get the fee required to send the message
    uint256 fees = s_router.getFee(_destinationChainSelector, evm2AnyMessage);

    uint256 requiredLinkBalance;
    if (_token == address(s_linkToken)) {
      // Required LINK Balance is the sum of fees and amount to transfer, if the token to transfer is LINK
      requiredLinkBalance = fees + _amount;
    } else {
      requiredLinkBalance = fees;
    }

    uint256 linkBalance = s_linkToken.balanceOf(address(this));

    if (requiredLinkBalance > linkBalance) {
      revert NotEnoughBalance(linkBalance, requiredLinkBalance);
    }

    // approve the Router to transfer LINK tokens on contract's behalf. It will spend the requiredLinkBalance
    s_linkToken.approve(address(s_router), requiredLinkBalance);

    // If sending a token other than LINK, approve it separately
    if (_token != address(s_linkToken)) {
      uint256 tokenBalance = IERC20(_token).balanceOf(address(this));
      if (_amount > tokenBalance) {
        revert NotEnoughBalance(tokenBalance, _amount);
      }
      // approve the Router to spend tokens on contract's behalf. It will spend the amount of the given token
      IERC20(_token).approve(address(s_router), _amount);
    }

    // Send the message through the router and store the returned message ID
    messageId = s_router.ccipSend(_destinationChainSelector, evm2AnyMessage);

    // Emit an event with message details
    emit TokensTransferred(messageId, _destinationChainSelector, _receiver, _token, _amount, address(s_linkToken), fees);

    // Return the message ID
    return messageId;
  }

  /// @notice Transfer tokens to receiver on the destination chain.
  /// @notice Pay in native gas such as ETH on Ethereum or POL on Polygon.
  /// @notice the token must be in the list of supported tokens.
  /// @notice This function can only be called by the owner.
  /// @dev Assumes your contract has sufficient native gas like ETH on Ethereum or POL on Polygon.
  /// @param _destinationChainSelector The identifier (aka selector) for the destination blockchain.
  /// @param _receiver The address of the recipient on the destination blockchain.
  /// @param _token token address.
  /// @param _amount token amount.
  /// @return messageId The ID of the message that was sent.
  function transferTokensPayNative(
    uint64 _destinationChainSelector,
    address _receiver,
    address _token,
    uint256 _amount
  )
    external
    onlyOwner
    onlyAllowlistedChain(_destinationChainSelector)
    validateReceiver(_receiver)
    returns (bytes32 messageId)
  {
    // Create an EVM2AnyMessage struct in memory with necessary information for sending a cross-chain message
    // address(0) means fees are paid in native gas
    Client.EVM2AnyMessage memory evm2AnyMessage = _buildCCIPMessage(_receiver, _token, _amount, address(0));

    // Get the fee required to send the message
    uint256 fees = s_router.getFee(_destinationChainSelector, evm2AnyMessage);

    if (fees > address(this).balance) {
      revert NotEnoughBalance(address(this).balance, fees);
    }

    // approve the Router to spend tokens on contract's behalf. It will spend the amount of the given token
    IERC20(_token).approve(address(s_router), _amount);

    // Send the message through the router and store the returned message ID
    messageId = s_router.ccipSend{value: fees}(_destinationChainSelector, evm2AnyMessage);

    // Emit an event with message details
    emit TokensTransferred(messageId, _destinationChainSelector, _receiver, _token, _amount, address(0), fees);

    // Return the message ID
    return messageId;
  }

  /// @notice Construct a CCIP message.
  /// @dev This function will create an EVM2AnyMessage struct with all the necessary information for tokens transfer.
  /// @param _receiver The address of the receiver.
  /// @param _token The token to be transferred.
  /// @param _amount The amount of the token to be transferred.
  /// @param _feeTokenAddress The address of the token used for fees. Set address(0) for native gas.
  /// @return Client.EVM2AnyMessage Returns an EVM2AnyMessage struct which contains information for sending a CCIP
  /// message.
  function _buildCCIPMessage(
    address _receiver,
    address _token,
    uint256 _amount,
    address _feeTokenAddress
  ) private pure returns (Client.EVM2AnyMessage memory) {
    // Set the token amounts
    Client.EVMTokenAmount[] memory tokenAmounts = new Client.EVMTokenAmount[](1);
    tokenAmounts[0] = Client.EVMTokenAmount({token: _token, amount: _amount});

    // Create an EVM2AnyMessage struct in memory with necessary information for sending a cross-chain message
    return Client.EVM2AnyMessage({
      receiver: abi.encode(_receiver), // ABI-encoded receiver address
      data: "", // No data
      tokenAmounts: tokenAmounts, // The amount and type of token being transferred
      extraArgs: Client._argsToBytes(
        // Additional arguments, setting gas limit and allowing out-of-order execution.
        // Best Practice: For simplicity, the values are hardcoded. It is advisable to use a more dynamic approach
        // where you set the extra arguments off-chain. This allows adaptation depending on the lanes, messages,
        // and ensures compatibility with future CCIP upgrades. Read more about it here:
        // https://docs.chain.link/ccip/concepts/best-practices/evm#using-extraargs
        Client.GenericExtraArgsV2({
          gasLimit: 0, // Gas limit for the callback on the destination chain
          allowOutOfOrderExecution: true // Allows the message to be executed out of order relative to other messages
          // from
          // the same sender
        })
      ),
      // Set the feeToken to a feeTokenAddress, indicating specific asset will be used for fees
      feeToken: _feeTokenAddress
    });
  }

  /// @notice Fallback function to allow the contract to receive Ether.
  /// @dev This function has no function body, making it a default function for receiving Ether.
  /// It is automatically called when Ether is transferred to the contract without any data.
  receive() external payable {}

  /// @notice Allows the contract owner to withdraw the entire balance of Ether from the contract.
  /// @dev This function reverts if there are no funds to withdraw or if the transfer fails.
  /// It should only be callable by the owner of the contract.
  /// @param _beneficiary The address to which the Ether should be transferred.
  function withdraw(
    address _beneficiary
  ) public onlyOwner {
    // Retrieve the balance of this contract
    uint256 amount = address(this).balance;

    // Revert if there is nothing to withdraw
    if (amount == 0) revert NothingToWithdraw();

    // Attempt to send the funds, capturing the success status and discarding any return data
    (bool sent,) = _beneficiary.call{value: amount}("");

    // Revert if the send failed, with information about the attempted transfer
    if (!sent) revert FailedToWithdrawEth(msg.sender, _beneficiary, amount);
  }

  /// @notice Allows the owner of the contract to withdraw all tokens of a specific ERC20 token.
  /// @dev This function reverts with a 'NothingToWithdraw' error if there are no tokens to withdraw.
  /// @param _beneficiary The address to which the tokens will be sent.
  /// @param _token The contract address of the ERC20 token to be withdrawn.
  function withdrawToken(
    address _beneficiary,
    address _token
  ) public onlyOwner {
    // Retrieve the balance of this contract
    uint256 amount = IERC20(_token).balanceOf(address(this));

    // Revert if there is nothing to withdraw
    if (amount == 0) revert NothingToWithdraw();

    IERC20(_token).safeTransfer(_beneficiary, amount);
  }
}
```

### Deploy your contracts

To use this contract:

1. [Open the contract in Remix](https://remix.ethereum.org/#url=https://docs.chain.link/samples/CCIP/TokenTransferor.sol).

2. Compile your contract.

3. Deploy and fund your sender contract on *Avalanche Fuji*:
   1. Open MetaMask and select the *Avalanche Fuji* network.

   2. In Remix IDE, click *Deploy & Run Transactions* and select *Injected Provider - MetaMask* from the environment list. Remix will then interact with your MetaMask wallet to communicate with *Avalanche Fuji*.

   3. Fill in your blockchain's router and LINK contract addresses. The router address can be found on the [CCIP Directory](/ccip/directory) and the LINK contract address on the [LINK token contracts page](/resources/link-token-contracts). For *Avalanche Fuji*:
      - The router address is 0xF694E193200268f9a4868e4Aa017A0118C9a8177,
      - The LINK contract address is 0x0b9d5D9136855f6FEc3c0993feE6E9CE8a297846.

   4. Click the **transact** button. After you confirm the transaction, the contract address appears on the *Deployed Contracts* list.
      Note your contract address.

   5. Open MetaMask and fund your contract with CCIP-BnM tokens. You can transfer 0.002 *CCIP-BnM* to your contract.

4. Enable your contract to transfer tokens to *Ethereum Sepolia*:
   1. In Remix IDE, under *Deploy & Run Transactions*, open the list of functions for your smart contract deployed on *Avalanche Fuji*.
   2. Call the `allowlistDestinationChain` function with 16015286601757825753 as the destination chain selector, and true as allowed. Each chain selector is found on the [CCIP Directory](/ccip/directory).

### Transfer tokens and pay in LINK

You will transfer *0.001 CCIP-BnM*. The CCIP fees for using CCIP will be paid in LINK. Read this [explanation](#transferring-tokens-and-pay-in-link) for a detailed description of the code example.

1. Open MetaMask and connect to *Avalanche Fuji*. Fund your contract with LINK tokens. You can transfer 70 *LINK* to your contract. **Note**: The LINK tokens are used to pay for CCIP fees.

   **Note:** This transaction fee is significantly higher than normal due to gas spikes on Sepolia. To run this example, you can get additional testnet LINK
   from [faucets.chain.link](https://faucets.chain.link) or use a supported testnet other than Sepolia.

2. Transfer CCIP-BnM from *Avalanche Fuji*:
   1. Open MetaMask and select the network *Avalanche Fuji*.

   2. In Remix IDE, under *Deploy & Run Transactions*, open the list of functions for your smart contract deployed on *Avalanche Fuji*.

   3. Fill in the arguments of the ***transferTokensPayLINK*** function:

      | Argument                   | Value and Description                                                                                                                                                                                                                                                    |
      | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
      | \_destinationChainSelector | <CopyText text="16015286601757825753" code /> <br /> CCIP Chain identifier of the destination blockchain (*Ethereum Sepolia* in this example). You can find each chain selector on the [CCIP Directory](/ccip/directory).                                                |
      | \_receiver                 | Your account address on *Ethereum Sepolia*. <br /> The destination account address. It could be a smart contract or an EOA.                                                                                                                                              |
      | \_token                    | <CopyText text="0xD21341536c5cF5EB1bcb58f6723cE26e8D8E90e4" code /><br /> The *CCIP-BnM* contract address at the source chain (*Avalanche Fuji* in this example). You can find all the addresses for each supported blockchain on the [CCIP Directory](/ccip/directory). |
      | \_amount                   | <CopyText text="1000000000000000" code /> <br /> The token amount (*0.001 CCIP-BnM*).                                                                                                                                                                                    |

   4. Click the **transact** button and confirm the transaction on MetaMask.

   5. Once the transaction is successful, note the transaction hash. Here is an [example](https://testnet.snowtrace.io/tx/0x62ca604240fc30133646ff94dcedac5375c5e42b109f3339c85e4fa29541d42b) of a transaction on *Avalanche Fuji*.

> \*\*NOTE: Gas price spikes\*\*
>
>
>
> Under normal circumstances, transactions on the Ethereum Sepolia network require significantly fewer tokens to pay for gas. However, during exceptional periods of high gas price spikes, your transactions may fail if not sufficiently funded. In such cases, you may need to fund your contract with additional tokens. We recommend paying for your CCIP transactions in **LINK** tokens (rather than native tokens) as you can obtain extra LINK testnet tokens from [faucets.chain.link](https://faucets.chain.link/). If you encounter a transaction failure due to these gas price spikes, please add additional LINK tokens to your contract and try again.
> Alternatively, you can use a supported testnet other than Sepolia.

1. Open the [CCIP explorer](https://ccip.chain.link/) and search your cross-chain transaction using the transaction hash.

   ![Image](/images/ccip/tutorials/ccip-explorer-send-tokens-pay-link-tx-details.webp)

2. The CCIP transaction is completed once the status is marked as "Success". The data field is empty because you are only transferring tokens.

   ![Image](/images/ccip/tutorials/ccip-explorer-send-tokens-pay-link-tx-details-success.webp)

3. Check the receiver account on the destination chain:
   1. Note the destination transaction hash from the CCIP explorer. `0x083fc1a79ffcfd617426fd71dff87ca16db2e4333e62a28cdd13d4bec0926bcb` in this example.

   2. Open the block explorer for your destination chain. For *Ethereum Sepolia*, open [etherscan](https://sepolia.etherscan.io).

   3. Search the [transaction hash](https://sepolia.etherscan.io/tx/0x083fc1a79ffcfd617426fd71dff87ca16db2e4333e62a28cdd13d4bec0926bcb).

      ![Image](/images/ccip/tutorials/send-tokens-pay-link-sepolia-tokens-received.webp)

   4. Notice in the *Tokens Transferred* section that CCIP-BnM tokens have been transferred to your account (0.001 CCIP-BnM).

### Transfer tokens and pay in native

You will transfer *0.001 CCIP-BnM*. The CCIP fees for using CCIP will be paid in Avalanche Fuji's native AVAX. Read this [explanation](#transferring-tokens-and-pay-in-native) for a detailed description of the code example.

1. Open MetaMask and connect to *Avalanche Fuji*. Fund your contract with native gas tokens. You can transfer 0.2 *AVAX* to your contract. **Note**: The native gas tokens are used to pay for CCIP fees.

2. Transfer CCIP-BnM from *Avalanche Fuji*:
   1. Open MetaMask and select the network *Avalanche Fuji*.

   2. In Remix IDE, under *Deploy & Run Transactions*, open the list of transactions of your smart contract deployed on *Avalanche Fuji*.

   3. Fill in the arguments of the ***transferTokensPayNative*** function:

      | Argument                   | Value and Description                                                                                                                                                                                                                                                     |
      | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
      | \_destinationChainSelector | <CopyText text="16015286601757825753" code /> <br /> CCIP Chain identifier of the destination blockchain (*Ethereum Sepolia* in this example). You can find each chain selector on the [CCIP Directory](/ccip/directory).                                                 |
      | \_receiver                 | Your account address on *Ethereum Sepolia*. <br /> The destination account address. It could be a smart contract or an EOA.                                                                                                                                               |
      | \_token                    | <CopyText text="0xD21341536c5cF5EB1bcb58f6723cE26e8D8E90e4" code /><br /> The *CCIP-BnM* contract address at the source chain (*Avalanche Fuji* in this example). You can find all the addresses for each supported blockchain on the [CCIP Directory](/ccip/directory).. |
      | \_amount                   | <CopyText text="1000000000000000" code /> <br /> The token amount (*0.001 CCIP-BnM*).                                                                                                                                                                                     |

   4. Click the **transact** button and confirm the transaction on MetaMask.

   5. Once the transaction is successful, note the transaction hash. Here is an [example](https://testnet.snowtrace.io/tx/0x186e5767d65dffe685c24d5ee881201e2b39fd684220a68943b0b861178ddf64) of a transaction on *Avalanche Fuji*.

> \*\*NOTE: Gas price spikes\*\*
>
>
>
> Under normal circumstances, transactions on the Ethereum Sepolia network require significantly fewer tokens to pay for gas. However, during exceptional periods of high gas price spikes, your transactions may fail if not sufficiently funded. In such cases, you may need to fund your contract with additional tokens. We recommend paying for your CCIP transactions in **LINK** tokens (rather than native tokens) as you can obtain extra LINK testnet tokens from [faucets.chain.link](https://faucets.chain.link/). If you encounter a transaction failure due to these gas price spikes, please add additional LINK tokens to your contract and try again.
> Alternatively, you can use a supported testnet other than Sepolia.

1. Open the [CCIP explorer](https://ccip.chain.link/) and search your cross-chain transaction using the transaction hash.

   ![Image](/images/ccip/tutorials/ccip-explorer-send-tokens-tx-details.webp)

2. The CCIP transaction is completed once the status is marked as "Success". The data field is empty because you only transfer tokens. Note that CCIP fees are denominated in LINK. Even if CCIP fees are paid using native gas tokens, node operators will be paid in LINK.

   ![Image](/images/ccip/tutorials/ccip-explorer-send-tokens-tx-details-success.webp)

3. Check the receiver account on the destination chain:
   1. Note the destination transaction hash from the CCIP explorer. `0xf403d828fa377d657af67f12e99ff435974299c27ba2d57c53494d29bbbfc938` in this example.

   2. Open the block explorer for your destination chain. For *Ethereum Sepolia*, open [etherscan](https://sepolia.etherscan.io).

   3. Search the [transaction hash](https://sepolia.etherscan.io/tx/0xf403d828fa377d657af67f12e99ff435974299c27ba2d57c53494d29bbbfc938).

      ![Image](/images/ccip/tutorials/sepolia-tokens-received.webp)

   4. Notice in the *Tokens Transferred* section that CCIP-BnM tokens have been transferred to your account (0.001 CCIP-BnM).

## Explanation

> \*\*NOTE: Integrate Chainlink CCIP v1.6.2 into your project\*\*
>
>
>
> <Tabs sharedStore="ccip-v1-6-2-package" client:visible>
>   <Fragment slot="tab.1">npm</Fragment>
>   <Fragment slot="tab.2">yarn</Fragment>
>   <Fragment slot="tab.3">foundry</Fragment>
>
>   <Fragment slot="panel.2">
>     If you use [Yarn](https://yarnpkg.com/), install the [@chainlink/contracts-ccip NPM package](https://www.npmjs.com/package/@chainlink/contracts-ccip):
>
>     ```shell
>     yarn add @chainlink/contracts-ccip@1.6.2
>     ```
>   </Fragment>
> </Tabs>

The smart contract featured in this tutorial is designed to interact with CCIP to transfer a supported token to an account on a destination chain. The contract code contains supporting comments clarifying the functions, events, and underlying logic. This section further explains initializing the contract and transferring tokens.

### Initializing of the contract

When you deploy the contract, you define the router address and LINK contract address of the blockchain where you deploy the contract. The contract uses the router address to interact with the router to estimate the CCIP fees and the transmission of CCIP messages.

### Transferring tokens and pay in LINK

The `transferTokensPayLINK` function undertakes six primary operations:

1. Call the `_buildCCIPMessage` private function to construct a CCIP-compatible message using the `EVM2AnyMessage` [struct](/ccip/api-reference/evm/v1.6.1/client#any2evmmessage):
   - The `_receiver` address is encoded in bytes to accommodate non-EVM destination blockchains with distinct address formats. The encoding is achieved through [abi.encode](https://docs.soliditylang.org/en/develop/abi-spec.html).
   - The `data` is empty because you only transfer tokens.
   - The `tokenAmounts` is an array, with each element comprising a [`EVMTokenAmount` struct](/ccip/api-reference/evm/v1.6.1/client#evmtokenamount) that contains the token address and amount. The array contains one element where the `_token` (token address) and `_amount` (token amount) are passed by the user when calling the `transferTokensPayLINK` function.
   - The `extraArgs` specifies the `gasLimit` for relaying the message to the recipient contract on the destination blockchain. In this example, the `gasLimit` is set to `0` because the contract only transfers tokens and does not expect function calls on the destination blockchain.
   - The `_feeTokenAddress` designates the token address used for CCIP fees. Here, `address(linkToken)` signifies payment in LINK.

     {" "}

> **CAUTION: Best Practices**
>
> This example is simplified for educational purposes. For production code, please adhere to the following best practices:

- **Do Not Hardcode `extraArgs`**: In this example, `extraArgs` are hardcoded within the contract for simplicity. It is recommended to make `extraArgs` mutable. For instance, you can construct `extraArgs` off-chain and pass them into your function calls, or store them in a storage variable that can be updated as needed. This approach ensures that `extraArgs` remain backward compatible with future CCIP upgrades. Refer to the [Best Practices](/ccip/concepts/best-practices/evm) guide for more information.

- **Validate the Destination Chain**: Always ensure that the destination chain is valid and supported before sending messages.

- **Understand `allowOutOfOrderExecution` Usage**: This example sets `allowOutOfOrderExecution` to `true` (see [GenericExtraArgsV2](/ccip/api-reference/evm/v1.6.1/client#genericextraargsv2)). Read the [Best Practices: Setting `allowOutOfOrderExecution`](/ccip/concepts/best-practices/evm#setting-allowoutoforderexecution) to learn more about this parameter.

- **Understand CCIP Service Limits**: Review the [CCIP Service Limits](/ccip/service-limits) for constraints on message data size, execution gas, and the number of tokens per transaction. If your requirements exceed these limits, you may need to [contact the Chainlink Labs Team](https://chain.link/ccip-contact).

Following these best practices ensures that your contract is robust, future-proof, and compliant with CCIP standards.

1. Computes the fees by invoking the router's `getFee` [function](/ccip/api-reference/evm/v1.6.1/i-router-client#getfee).
2. Ensures your contract balance in LINK is enough to cover the fees.
3. Grants the router contract permission to deduct the fees from the contract's LINK balance.
4. Grants the router contract permission to deduct the amount from the contract's *CCIP-BnM* balance.
5. Dispatches the CCIP message to the destination chain by executing the router's `ccipSend` [function](/ccip/api-reference/evm/v1.6.1/i-router-client#ccipsend).

**Note**: As a security measure, the `transferTokensPayLINK` function is protected by the `onlyAllowlistedChain` to ensure the contract owner has allowlisted a destination chain.

### Transferring tokens and pay in native

The `transferTokensPayNative` function undertakes five primary operations:

1. Call the `_buildCCIPMessage` private function to construct a CCIP-compatible message using the `EVM2AnyMessage` [struct](/ccip/api-reference/evm/v1.6.1/client#any2evmmessage):
   - The `_receiver` address is encoded in bytes to accommodate non-EVM destination blockchains with distinct address formats. The encoding is achieved through [abi.encode](https://docs.soliditylang.org/en/develop/abi-spec.html).
   - The `data` is empty because you only transfer tokens.
   - The `tokenAmounts` is an array, with each element comprising an `EVMTokenAmount` [struct](/ccip/api-reference/evm/v1.6.1/client#evmtokenamount) containing the token address and amount. The array contains one element where the `_token` (token address) and `_amount` (token amount) are passed by the user when calling the `transferTokensPayNative` function.
   - The `extraArgs` specifies the `gasLimit` for relaying the message to the recipient contract on the destination blockchain. In this example, the `gasLimit` is set to `0` because the contract only transfers tokens and does not expect function calls on the destination blockchain.
   - The `_feeTokenAddress` designates the token address used for CCIP fees. Here, `address(0)` signifies payment in native gas tokens (ETH).

     {" "}

> **CAUTION: Best Practices**
>
> This example is simplified for educational purposes. For production code, please adhere to the following best practices:

- **Do Not Hardcode `extraArgs`**: In this example, `extraArgs` are hardcoded within the contract for simplicity. It is recommended to make `extraArgs` mutable. For instance, you can construct `extraArgs` off-chain and pass them into your function calls, or store them in a storage variable that can be updated as needed. This approach ensures that `extraArgs` remain backward compatible with future CCIP upgrades. Refer to the [Best Practices](/ccip/concepts/best-practices/evm) guide for more information.

- **Validate the Destination Chain**: Always ensure that the destination chain is valid and supported before sending messages.

- **Understand `allowOutOfOrderExecution` Usage**: This example sets `allowOutOfOrderExecution` to `true` (see [GenericExtraArgsV2](/ccip/api-reference/evm/v1.6.1/client#genericextraargsv2)). Read the [Best Practices: Setting `allowOutOfOrderExecution`](/ccip/concepts/best-practices/evm#setting-allowoutoforderexecution) to learn more about this parameter.

- **Understand CCIP Service Limits**: Review the [CCIP Service Limits](/ccip/service-limits) for constraints on message data size, execution gas, and the number of tokens per transaction. If your requirements exceed these limits, you may need to [contact the Chainlink Labs Team](https://chain.link/ccip-contact).

Following these best practices ensures that your contract is robust, future-proof, and compliant with CCIP standards.

1. Computes the fees by invoking the router's `getFee` [function](/ccip/api-reference/evm/v1.6.1/i-router-client#getfee).
2. Ensures your contract balance in native gas is enough to cover the fees.
3. Grants the router contract permission to deduct the amount from the contract's *CCIP-BnM* balance.
4. Dispatches the CCIP message to the destination chain by executing the router's `ccipSend` [function](/ccip/api-reference/evm/v1.6.1/i-router-client#ccipsend). **Note**: `msg.value` is set because you pay in native gas.

**Note**: As a security measure, the `transferTokensPayNative` function is protected by the `onlyAllowlistedChain`, ensuring the contract owner has allowlisted a destination chain.

> **CAUTION: Educational Example Disclaimer**
>
> This page includes an educational example to use a Chainlink system, product, or service and is provided to
> demonstrate how to interact with Chainlink's systems, products, and services to integrate them into your own. This
> template is provided "AS IS" and "AS AVAILABLE" without warranties of any kind, it has not been audited, and it may be
> missing key checks or error handling to make the usage of the system, product or service more clear. Do not use the
> code in this example in a production environment without completing your own audits and application of best practices.
> Neither Chainlink Labs, the Chainlink Foundation, nor Chainlink node operators are responsible for unintended outputs
> that are generated due to errors in code.