# Transfer Tokens with Data
Source: https://docs.chain.link/ccip/tutorials/evm/programmable-token-transfers
Last Updated: 2025-05-19

> For the complete documentation index, see [llms.txt](/llms.txt).

In this tutorial, you will use Chainlink CCIP to transfer tokens and arbitrary data between smart contracts on different blockchains. First, you will pay for the CCIP fees on the source blockchain using LINK. Then, you will use the same contract to pay CCIP fees in native gas tokens. For example, you would use ETH on Ethereum or AVAX on Avalanche.

> **NOTE: Node Operator Rewards**
>
> CCIP rewards the oracle node operators in LINK.

> **CAUTION: Transferring tokens**
>
> This tutorial uses the term "transferring tokens" even though the tokens are not technically transferred. Instead,
> they are locked or burned on the source chain and then unlocked or minted on the destination chain. Read the [Token
> Pools](/ccip/concepts/cross-chain-token/evm/token-pools) section to understand the various mechanisms that are used to
> transfer value across chains.

## Before you begin

1. You should understand how to write, compile, deploy, and fund a smart contract. If you need to brush up on the basics, read this [tutorial](/quickstarts/deploy-your-first-contract), which will guide you through using the [Solidity programming language](https://soliditylang.org/), interacting with the [MetaMask wallet](https://metamask.io) and working within the [Remix Development Environment](https://remix.ethereum.org/).
2. Your account must have some AVAX and LINK tokens on *Avalanche Fuji* and ETH tokens on *Ethereum Sepolia*. Learn how to [Acquire testnet LINK](/resources/acquire-link).
3. Check the [CCIP Directory](/ccip/directory) to confirm that the tokens you will transfer are supported for your lane. In this example, you will transfer tokens from *Avalanche Fuji* to *Ethereum Sepolia* so check the list of supported tokens [here](/ccip/directory/testnet/chain/avalanche-fuji-testnet).
4. Learn how to [acquire CCIP test tokens](/ccip/test-tokens#evm-chains). Following this guide, you should have CCIP-BnM tokens, and CCIP-BnM should appear in the list of your tokens in MetaMask.
5. Learn how to [fund your contract](/resources/fund-your-contract). This guide shows how to fund your contract in LINK, but you can use the same guide for funding your contract with any ERC20 tokens as long as they appear in the list of tokens in MetaMask.
6. Follow the previous tutorial: [*Transfer tokens*](/ccip/tutorials/evm/transfer-tokens-from-contract).

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

In this tutorial, you will send a *string* text and CCIP-BnM tokens between smart contracts on *Avalanche Fuji* and *Ethereum Sepolia* using CCIP. First, you will pay [CCIP fees in LINK](#transfer-and-receive-tokens-and-data-and-pay-in-link), then you will pay [CCIP fees in native gas](#transfer-and-receive-tokens-and-data-and-pay-in-native).

```sol
// SPDX-License-Identifier: MIT
pragma solidity 0.8.24;

import {CCIPReceiver} from "@chainlink/contracts-ccip/contracts/applications/CCIPReceiver.sol";
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

/// @title - A simple messenger contract for transferring/receiving tokens and data across chains.
contract ProgrammableTokenTransfers is CCIPReceiver, OwnerIsCreator {
  using SafeERC20 for IERC20;

  // Custom errors to provide more descriptive revert messages.
  error NotEnoughBalance(uint256 currentBalance, uint256 requiredBalance); // Used to make sure contract has enough
  // token balance
  error NothingToWithdraw(); // Used when trying to withdraw Ether but there's nothing to withdraw.
  error FailedToWithdrawEth(address owner, address target, uint256 value); // Used when the withdrawal of Ether fails.
  error DestinationChainNotAllowed(uint64 destinationChainSelector); // Used when the destination chain has not been
  // allowlisted by the contract owner.
  error SourceChainNotAllowed(uint64 sourceChainSelector); // Used when the source chain has not been allowlisted by the
  // contract owner.
  error SenderNotAllowed(address sender); // Used when the sender has not been allowlisted by the contract owner.
  error InvalidReceiverAddress(); // Used when the receiver address is 0.

  // Event emitted when a message is sent to another chain.
  // The chain selector of the destination chain.
  // The address of the receiver on the destination chain.
  // The text being sent.
  // The token address that was transferred.
  // The token amount that was transferred.
  // the token address used to pay CCIP fees.
  // The fees paid for sending the message.
  event MessageSent( // The unique ID of the CCIP message.
    bytes32 indexed messageId,
    uint64 indexed destinationChainSelector,
    address receiver,
    string text,
    address token,
    uint256 tokenAmount,
    address feeToken,
    uint256 fees
  );

  // Event emitted when a message is received from another chain.
  // The chain selector of the source chain.
  // The address of the sender from the source chain.
  // The text that was received.
  // The token address that was transferred.
  // The token amount that was transferred.
  event MessageReceived( // The unique ID of the CCIP message.
    bytes32 indexed messageId,
    uint64 indexed sourceChainSelector,
    address sender,
    string text,
    address token,
    uint256 tokenAmount
  );

  bytes32 private s_lastReceivedMessageId; // Store the last received messageId.
  address private s_lastReceivedTokenAddress; // Store the last received token address.
  uint256 private s_lastReceivedTokenAmount; // Store the last received amount.
  string private s_lastReceivedText; // Store the last received text.

  // Mapping to keep track of allowlisted destination chains.
  mapping(uint64 => bool) public allowlistedDestinationChains;

  // Mapping to keep track of allowlisted source chains.
  mapping(uint64 => bool) public allowlistedSourceChains;

  // Mapping to keep track of allowlisted senders.
  mapping(address => bool) public allowlistedSenders;

  IERC20 private s_linkToken;

  /// @notice Constructor initializes the contract with the router address.
  /// @param _router The address of the router contract.
  /// @param _link The address of the link contract.
  constructor(
    address _router,
    address _link
  ) CCIPReceiver(_router) {
    s_linkToken = IERC20(_link);
  }

  /// @dev Modifier that checks if the chain with the given destinationChainSelector is allowlisted.
  /// @param _destinationChainSelector The selector of the destination chain.
  modifier onlyAllowlistedDestinationChain(
    uint64 _destinationChainSelector
  ) {
    if (!allowlistedDestinationChains[_destinationChainSelector]) {
      revert DestinationChainNotAllowed(_destinationChainSelector);
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

  /// @dev Modifier that checks if the chain with the given sourceChainSelector is allowlisted and if the sender is
  /// allowlisted.
  /// @param _sourceChainSelector The selector of the destination chain.
  /// @param _sender The address of the sender.
  modifier onlyAllowlisted(
    uint64 _sourceChainSelector,
    address _sender
  ) {
    if (!allowlistedSourceChains[_sourceChainSelector]) {
      revert SourceChainNotAllowed(_sourceChainSelector);
    }
    if (!allowlistedSenders[_sender]) revert SenderNotAllowed(_sender);
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
    allowlistedDestinationChains[_destinationChainSelector] = allowed;
  }

  /// @dev Updates the allowlist status of a source chain
  /// @notice This function can only be called by the owner.
  /// @param _sourceChainSelector The selector of the source chain to be updated.
  /// @param allowed The allowlist status to be set for the source chain.
  function allowlistSourceChain(
    uint64 _sourceChainSelector,
    bool allowed
  ) external onlyOwner {
    allowlistedSourceChains[_sourceChainSelector] = allowed;
  }

  /// @dev Updates the allowlist status of a sender for transactions.
  /// @notice This function can only be called by the owner.
  /// @param _sender The address of the sender to be updated.
  /// @param allowed The allowlist status to be set for the sender.
  function allowlistSender(
    address _sender,
    bool allowed
  ) external onlyOwner {
    allowlistedSenders[_sender] = allowed;
  }

  /// @notice Sends data and transfer tokens to receiver on the destination chain.
  /// @notice Pay for fees in LINK.
  /// @dev Assumes your contract has sufficient LINK to pay for CCIP fees.
  /// @param _destinationChainSelector The identifier (aka selector) for the destination blockchain.
  /// @param _receiver The address of the recipient on the destination blockchain.
  /// @param _text The string data to be sent.
  /// @param _token token address.
  /// @param _amount token amount.
  /// @return messageId The ID of the CCIP message that was sent.
  function sendMessagePayLINK(
    uint64 _destinationChainSelector,
    address _receiver,
    string calldata _text,
    address _token,
    uint256 _amount
  )
    external
    onlyOwner
    onlyAllowlistedDestinationChain(_destinationChainSelector)
    validateReceiver(_receiver)
    returns (bytes32 messageId)
  {
    // Create an EVM2AnyMessage struct in memory with necessary information for sending a cross-chain message
    // address(linkToken) means fees are paid in LINK
    Client.EVM2AnyMessage memory evm2AnyMessage =
      _buildCCIPMessage(_receiver, _text, _token, _amount, address(s_linkToken));

    // Initialize a router client instance to interact with cross-chain router
    IRouterClient router = IRouterClient(this.getRouter());

    // Get the fee required to send the CCIP message
    uint256 fees = router.getFee(_destinationChainSelector, evm2AnyMessage);

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
    s_linkToken.approve(address(router), requiredLinkBalance);

    // If sending a token other than LINK, approve it separately
    if (_token != address(s_linkToken)) {
      uint256 tokenBalance = IERC20(_token).balanceOf(address(this));
      if (_amount > tokenBalance) {
        revert NotEnoughBalance(tokenBalance, _amount);
      }
      // approve the Router to spend tokens on contract's behalf. It will spend the amount of the given token
      IERC20(_token).approve(address(router), _amount);
    }

    // Send the message through the router and store the returned message ID
    messageId = router.ccipSend(_destinationChainSelector, evm2AnyMessage);

    // Emit an event with message details
    emit MessageSent(
      messageId, _destinationChainSelector, _receiver, _text, _token, _amount, address(s_linkToken), fees
    );

    // Return the message ID
    return messageId;
  }

  /// @notice Sends data and transfer tokens to receiver on the destination chain.
  /// @notice Pay for fees in native gas.
  /// @dev Assumes your contract has sufficient native gas like ETH on Ethereum or POL on Polygon.
  /// @param _destinationChainSelector The identifier (aka selector) for the destination blockchain.
  /// @param _receiver The address of the recipient on the destination blockchain.
  /// @param _text The string data to be sent.
  /// @param _token token address.
  /// @param _amount token amount.
  /// @return messageId The ID of the CCIP message that was sent.
  function sendMessagePayNative(
    uint64 _destinationChainSelector,
    address _receiver,
    string calldata _text,
    address _token,
    uint256 _amount
  )
    external
    onlyOwner
    onlyAllowlistedDestinationChain(_destinationChainSelector)
    validateReceiver(_receiver)
    returns (bytes32 messageId)
  {
    // Create an EVM2AnyMessage struct in memory with necessary information for sending a cross-chain message
    // address(0) means fees are paid in native gas
    Client.EVM2AnyMessage memory evm2AnyMessage = _buildCCIPMessage(_receiver, _text, _token, _amount, address(0));

    // Initialize a router client instance to interact with cross-chain router
    IRouterClient router = IRouterClient(this.getRouter());

    // Get the fee required to send the CCIP message
    uint256 fees = router.getFee(_destinationChainSelector, evm2AnyMessage);

    if (fees > address(this).balance) {
      revert NotEnoughBalance(address(this).balance, fees);
    }

    // approve the Router to spend tokens on contract's behalf. It will spend the amount of the given token
    IERC20(_token).approve(address(router), _amount);

    // Send the message through the router and store the returned message ID
    messageId = router.ccipSend{value: fees}(_destinationChainSelector, evm2AnyMessage);

    // Emit an event with message details
    emit MessageSent(messageId, _destinationChainSelector, _receiver, _text, _token, _amount, address(0), fees);

    // Return the message ID
    return messageId;
  }

  /**
   * @notice Returns the details of the last CCIP received message.
   * @dev This function retrieves the ID, text, token address, and token amount of the last received CCIP message.
   * @return messageId The ID of the last received CCIP message.
   * @return text The text of the last received CCIP message.
   * @return tokenAddress The address of the token in the last CCIP received message.
   * @return tokenAmount The amount of the token in the last CCIP received message.
   */
  function getLastReceivedMessageDetails()
    public
    view
    returns (bytes32 messageId, string memory text, address tokenAddress, uint256 tokenAmount)
  {
    return (s_lastReceivedMessageId, s_lastReceivedText, s_lastReceivedTokenAddress, s_lastReceivedTokenAmount);
  }

  /// handle a received message
  function _ccipReceive(
    Client.Any2EVMMessage memory any2EvmMessage
  )
    internal
    override
    onlyAllowlisted(any2EvmMessage.sourceChainSelector, abi.decode(any2EvmMessage.sender, (address))) // Make sure
    // source chain and sender are allowlisted

  {
    s_lastReceivedMessageId = any2EvmMessage.messageId; // fetch the messageId
    s_lastReceivedText = abi.decode(any2EvmMessage.data, (string)); // abi-decoding of the sent text
    // Expect one token to be transferred at once, but you can transfer several tokens.
    s_lastReceivedTokenAddress = any2EvmMessage.destTokenAmounts[0].token;
    s_lastReceivedTokenAmount = any2EvmMessage.destTokenAmounts[0].amount;

    emit MessageReceived(
      any2EvmMessage.messageId,
      any2EvmMessage.sourceChainSelector, // fetch the source chain identifier (aka selector)
      abi.decode(any2EvmMessage.sender, (address)), // abi-decoding of the sender address,
      abi.decode(any2EvmMessage.data, (string)),
      any2EvmMessage.destTokenAmounts[0].token,
      any2EvmMessage.destTokenAmounts[0].amount
    );
  }

  /// @notice Construct a CCIP message.
  /// @dev This function will create an EVM2AnyMessage struct with all the necessary information for programmable tokens
  /// transfer.
  /// @param _receiver The address of the receiver.
  /// @param _text The string data to be sent.
  /// @param _token The token to be transferred.
  /// @param _amount The amount of the token to be transferred.
  /// @param _feeTokenAddress The address of the token used for fees. Set address(0) for native gas.
  /// @return Client.EVM2AnyMessage Returns an EVM2AnyMessage struct which contains information for sending a CCIP
  /// message.
  function _buildCCIPMessage(
    address _receiver,
    string calldata _text,
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
      data: abi.encode(_text), // ABI-encoded string
      tokenAmounts: tokenAmounts, // The amount and type of token being transferred
      extraArgs: Client._argsToBytes(
        // Additional arguments, setting gas limit and allowing out-of-order execution.
        // Best Practice: For simplicity, the values are hardcoded. It is advisable to use a more dynamic approach
        // where you set the extra arguments off-chain. This allows adaptation depending on the lanes, messages,
        // and ensures compatibility with future CCIP upgrades. Read more about it here:
        // https://docs.chain.link/ccip/concepts/best-practices/evm#using-extraargs
        Client.GenericExtraArgsV2({
          gasLimit: 200_000, // Gas limit for the callback on the destination chain
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
  /// It is automatically called when Ether is sent to the contract without any data.
  receive() external payable {}

  /// @notice Allows the contract owner to withdraw the entire balance of Ether from the contract.
  /// @dev This function reverts if there are no funds to withdraw or if the transfer fails.
  /// It should only be callable by the owner of the contract.
  /// @param _beneficiary The address to which the Ether should be sent.
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

1. [Open the contract in Remix](https://remix.ethereum.org/#url=https://docs.chain.link/samples/CCIP/ProgrammableTokenTransfers.sol).

2. Compile your contract.

3. Deploy, fund your sender contract on *Avalanche Fuji* and enable sending messages to *Ethereum Sepolia*:
   1. Open MetaMask and select the network *Avalanche Fuji*.
   2. In Remix IDE, click on *Deploy & Run Transactions* and select *Injected Provider - MetaMask* from the environment list. Remix will then interact with your MetaMask wallet to communicate with *Avalanche Fuji*.
   3. Fill in your blockchain's router and LINK contract addresses. The router address can be found on the [CCIP Directory](/ccip/directory) and the LINK contract address on the [LINK token contracts page](/resources/link-token-contracts). For *Avalanche Fuji*:
      - The router address is 0xF694E193200268f9a4868e4Aa017A0118C9a8177,
      - The LINK contract address is 0x0b9d5D9136855f6FEc3c0993feE6E9CE8a297846.
   4. Click the **transact** button. After you confirm the transaction, the contract address appears on the *Deployed Contracts* list.
      Note your contract address.
   5. Open MetaMask and fund your contract with CCIP-BnM tokens. You can transfer 0.002 *CCIP-BnM* to your contract.
   6. Enable your contract to send CCIP messages to *Ethereum Sepolia*:
      1. In Remix IDE, under *Deploy & Run Transactions*, open the list of functions of your smart contract deployed on *Avalanche Fuji*.
      2. Call the `allowlistDestinationChain`, setting the destination chain selector to 16015286601757825753 and setting `allowed` to true. Each chain selector is found on the [CCIP Directory](/ccip/directory).

4. Deploy your receiver contract on *Ethereum Sepolia* and enable receiving messages from your sender contract:
   1. Open MetaMask and select the network *Ethereum Sepolia*.
   2. In Remix IDE, under *Deploy & Run Transactions*, make sure the environment is still *Injected Provider - MetaMask*.
   3. Fill in your blockchain's router and LINK contract addresses. The router address can be found on the [CCIP Directory](/ccip/directory) and the LINK contract address on the [LINK token contracts page](/resources/link-token-contracts). For *Ethereum Sepolia*, the router address is 0x0BF3dE8c5D3e8A2B34D2BEeB17ABfCeBaf363A59 and the LINK contract address is 0x779877A7B0D9E8603169DdbD7836e478b4624789.
   4. Click the **transact** button. After you confirm the transaction, the contract address appears on the *Deployed Contracts* list.
      Note your contract address.
   5. Enable your contract to receive CCIP messages from *Avalanche Fuji*:
      1. In Remix IDE, under *Deploy & Run Transactions*, open the list of functions of your smart contract deployed on *Ethereum Sepolia*.
      2. Call the `allowlistSourceChain` with 14767482510784806043 as the source chain selector, and true as allowed. Each chain selector is found on the [CCIP Directory](/ccip/directory).
   6. Enable your contract to receive CCIP messages from the contract that you deployed on *Avalanche Fuji*:
      1. In Remix IDE, under *Deploy & Run Transactions*, open the list of functions of your smart contract deployed on *Ethereum Sepolia*.
      2. Call the `allowlistSender` with the contract address of the contract that you deployed on *Avalanche Fuji*, and true as allowed.

At this point, you have one *sender* contract on *Avalanche Fuji* and one *receiver* contract on *Ethereum Sepolia*. As security measures, you enabled the sender contract to send CCIP messages to *Ethereum Sepolia* and the receiver contract to receive CCIP messages from the sender on *Avalanche Fuji*.

**Note**: Another security measure enforces that only the router can call the `_ccipReceive` function. Read the [explanation](#explanation) section for more details.

### Transfer and Receive tokens and data and pay in LINK

You will transfer *0.001 CCIP-BnM* and a text. The CCIP fees for using CCIP will be paid in LINK. Read this [explanation](#transferring-tokens-and-data-and-pay-in-link) for a detailed description of the code example.

1. Open MetaMask and connect to *Avalanche Fuji*. Fund your contract with LINK tokens. You can transfer 70 *LINK* to your contract. In this example, LINK is used to pay the CCIP fees.

   **Note:** This transaction fee is significantly higher than normal due to gas spikes on Sepolia. To run this example, you can get additional testnet LINK
   from [faucets.chain.link](https://faucets.chain.link) or use a supported testnet other than Sepolia.

2. Send a string data with tokens from *Avalanche Fuji*:
   1. Open MetaMask and select the network *Avalanche Fuji*.

   2. In Remix IDE, under *Deploy & Run Transactions*, open the list of functions of your smart contract deployed on *Avalanche Fuji*.

   3. Fill in the arguments of the ***sendMessagePayLINK*** function:

      | Argument                   | Value and Description                                                                                                                                                                                                                                                    |
      | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
      | \_destinationChainSelector | <CopyText text="16015286601757825753" code /> <br /> CCIP Chain identifier of the destination blockchain (*Ethereum Sepolia* in this example). You can find each chain selector on the [CCIP Directory](/ccip/directory).                                                |
      | \_receiver                 | Your receiver contract address on *Ethereum Sepolia*. <br /> The destination contract address.                                                                                                                                                                           |
      | \_text                     | <CopyText text="Hello World!" code /><br />Any `string`                                                                                                                                                                                                                  |
      | \_token                    | <CopyText text="0xD21341536c5cF5EB1bcb58f6723cE26e8D8E90e4" code /><br /> The *CCIP-BnM* contract address at the source chain (*Avalanche Fuji* in this example). You can find all the addresses for each supported blockchain on the [CCIP Directory](/ccip/directory). |
      | \_amount                   | <CopyText text="1000000000000000" code /> <br /> The token amount (*0.001 CCIP-BnM*).                                                                                                                                                                                    |

   4. Click on `transact` and confirm the transaction on MetaMask.

   5. After the transaction is successful, record the transaction hash. Here is an [example](https://testnet.snowtrace.io/tx/0xd3a0fade0e143fb39964c764bd4803e40062ba8c88e129f44ee795e33ade464b) of a transaction on *Avalanche Fuji*.

> \*\*NOTE: Gas price spikes\*\*
>
>
>
> Under normal circumstances, transactions on the Ethereum Sepolia network require significantly fewer tokens to pay for gas. However, during exceptional periods of high gas price spikes, your transactions may fail if not sufficiently funded. In such cases, you may need to fund your contract with additional tokens. We recommend paying for your CCIP transactions in **LINK** tokens (rather than native tokens) as you can obtain extra LINK testnet tokens from [faucets.chain.link](https://faucets.chain.link/). If you encounter a transaction failure due to these gas price spikes, please add additional LINK tokens to your contract and try again.
> Alternatively, you can use a supported testnet other than Sepolia.

1. Open the [CCIP explorer](https://ccip.chain.link/) and search your cross-chain transaction using the transaction hash.

   ![Image](/images/ccip/tutorials/ccip-explorer-send-tokens-message-pay-link-tx-details.webp)

2. The CCIP transaction is completed once the status is marked as "Success". In this example, the CCIP message ID is *0x99a15381125e740c43a60f03c6b011ae05a3541998ca482fb5a4814417627df8*.

   ![Image](/images/ccip/tutorials/ccip-explorer-send-tokens-message-pay-link-tx-details-success.webp)

3. Check the receiver contract on the destination chain:
   1. Open MetaMask and select the network *Ethereum Sepolia*.

   2. In Remix IDE, under *Deploy & Run Transactions*, open the list of functions of your smart contract deployed on *Ethereum Sepolia*.

   3. Call the `getLastReceivedMessageDetails` function.

      ![Image](/images/ccip/tutorials/sepolia-token-messagedetails-pay-link.webp)

   4. Notice the received messageId is *0x99a15381125e740c43a60f03c6b011ae05a3541998ca482fb5a4814417627df8*, the received text is *Hello World!*, the token address is *0xFd57b4ddBf88a4e07fF4e34C487b99af2Fe82a05* (CCIP-BnM token address on *Ethereum Sepolia*) and the token amount is 1000000000000000 (0.001 CCIP-BnM).

**Note**: These example contracts are designed to work bi-directionally. As an exercise, you can use them to transfer tokens with data from *Avalanche Fuji* to *Ethereum Sepolia* and from *Ethereum Sepolia* back to *Avalanche Fuji*.

### Transfer and Receive tokens and data and pay in native

You will transfer *0.001 CCIP-BnM* and a text. The CCIP fees for using CCIP will be paid in Avalanche's native AVAX. Read this [explanation](#transferring-tokens-and-data-and-pay-in-native) for a detailed description of the code example.

1. Open MetaMask and connect to *Avalanche Fuji*. Fund your contract with AVAX tokens. You can transfer 0.2 *AVAX* to your contract. The native gas tokens are used to pay the CCIP fees.

2. Send a string data with tokens from *Avalanche Fuji*:
   1. Open MetaMask and select the network *Avalanche Fuji*.

   2. In Remix IDE, under *Deploy & Run Transactions*, open the list of functions of your smart contract deployed on *Avalanche Fuji*.

   3. Fill in the arguments of the ***sendMessagePayNative*** function:

      | Argument                   | Value and Description                                                                                                                                                                                                                                                    |
      | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
      | \_destinationChainSelector | <CopyText text="16015286601757825753" code /> <br /> CCIP Chain identifier of the destination blockchain (*Ethereum Sepolia* in this example). You can find each chain selector on the [CCIP Directory](/ccip/directory).                                                |
      | \_receiver                 | Your receiver contract address at *Ethereum Sepolia*. <br /> The destination contract address.                                                                                                                                                                           |
      | \_text                     | <CopyText text="Hello World!" code /><br />Any `string`                                                                                                                                                                                                                  |
      | \_token                    | <CopyText text="0xD21341536c5cF5EB1bcb58f6723cE26e8D8E90e4" code /><br /> The *CCIP-BnM* contract address at the source chain (*Avalanche Fuji* in this example). You can find all the addresses for each supported blockchain on the [CCIP Directory](/ccip/directory). |
      | \_amount                   | <CopyText text="1000000000000000" code /> <br /> The token amount (*0.001 CCIP-BnM*).                                                                                                                                                                                    |

   4. Click on `transact` and confirm the transaction on MetaMask.

   5. Once the transaction is successful, note the transaction hash. Here is an [example](https://testnet.snowtrace.io/tx/0x8101fef78288981813915e77f8e5746bdba69711bdb7bc1706944a67ac70854b) of a transaction on *Avalanche Fuji*.

> \*\*NOTE: Gas price spikes\*\*
>
>
>
> Under normal circumstances, transactions on the Ethereum Sepolia network require significantly fewer tokens to pay for gas. However, during exceptional periods of high gas price spikes, your transactions may fail if not sufficiently funded. In such cases, you may need to fund your contract with additional tokens. We recommend paying for your CCIP transactions in **LINK** tokens (rather than native tokens) as you can obtain extra LINK testnet tokens from [faucets.chain.link](https://faucets.chain.link/). If you encounter a transaction failure due to these gas price spikes, please add additional LINK tokens to your contract and try again.
> Alternatively, you can use a supported testnet other than Sepolia.

1. Open the [CCIP explorer](https://ccip.chain.link/) and search your cross-chain transaction using the transaction hash.

   ![Image](/images/ccip/tutorials/ccip-explorer-send-tokens-message-tx-details.webp)

2. The CCIP transaction is completed once the status is marked as "Success". In this example, the CCIP message ID is *0x32bf96ac8b01fe3f04ffa548a3403b3105b4ed479eff407ff763b7539a1d43bd*. Note that CCIP fees are denominated in LINK. Even if CCIP fees are paid using native gas tokens, node operators will be paid in LINK.

   ![Image](/images/ccip/tutorials/ccip-explorer-send-tokens-message-tx-details-success.webp)

3. Check the receiver contract on the destination chain:
   1. Open MetaMask and select the network *Ethereum Sepolia*.

   2. In Remix IDE, under *Deploy & Run Transactions*, open the list of functions of your smart contract deployed on *Ethereum Sepolia*.

   3. Call the `getLastReceivedMessageDetails` function.

      ![Image](/images/ccip/tutorials/sepolia-token-messagedetails.webp)

   4. Notice the received messageId is *0x32bf96ac8b01fe3f04ffa548a3403b3105b4ed479eff407ff763b7539a1d43bd*, the received text is *Hello World!*, the token address is *0xFd57b4ddBf88a4e07fF4e34C487b99af2Fe82a05* (CCIP-BnM token address on *Ethereum Sepolia*) and the token amount is 1000000000000000 (0.001 CCIP-BnM).

**Note**: These example contracts are designed to work bi-directionally. As an exercise, you can use them to transfer tokens with data from *Avalanche Fuji* to *Ethereum Sepolia* and from *Ethereum Sepolia* back to *Avalanche Fuji*.

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

The smart contract featured in this tutorial is designed to interact with CCIP to transfer and receive tokens and data. The contract code contains supporting comments clarifying the functions, events, and underlying logic. Here we will further explain initializing the contract and sending data with tokens.

### Initializing the contract

When deploying the contract, we define the router address and LINK contract address of the blockchain we deploy the contract on.
Defining the router address is useful for the following:

- Sender part:
  - Calls the router's `getFee` [function](/ccip/api-reference/evm/v1.6.1/i-router-client#getfee) to estimate the CCIP fees.
  - Calls the router's `ccipSend` [function](/ccip/api-reference/evm/v1.6.1/i-router-client#ccipsend) to send CCIP messages.

- Receiver part:
  - The contract inherits from [CCIPReceiver](/ccip/api-reference/evm/v1.6.1/ccip-receiver), which serves as a base contract for receiver contracts. This contract requires that child contracts implement the `_ccipReceive` [function](/ccip/api-reference/evm/v1.6.1/ccip-receiver#_ccipreceive). `_ccipReceive` is called by the `ccipReceive` [function](/ccip/api-reference/evm/v1.6.1/ccip-receiver#ccipreceive), which ensures that only the router can deliver CCIP messages to the receiver contract.

### Transferring tokens and data and pay in LINK

The `sendMessagePayLINK` function undertakes six primary operations:

1. Call the `_buildCCIPMessage` private function to construct a CCIP-compatible message using the `EVM2AnyMessage` [struct](/ccip/api-reference/evm/v1.6.1/client#any2evmmessage):
   - The `_receiver` address is encoded in bytes to accommodate non-EVM destination blockchains with distinct address formats. The encoding is achieved through [abi.encode](https://docs.soliditylang.org/en/develop/abi-spec.html).
   - The `data` is encoded from a `string` to `bytes` using [abi.encode](https://docs.soliditylang.org/en/develop/abi-spec.html).
   - The `tokenAmounts` is an array, with each element comprising an `EVMTokenAmount` [struct](/ccip/api-reference/evm/v1.6.1/client#evmtokenamount) containing the token address and amount. The array contains one element where the `_token` (token address) and `_amount` (token amount) are passed by the user when calling the `sendMessagePayLINK` function.
   - The `extraArgs` specifies the `gasLimit` for relaying the message to the recipient contract on the destination blockchain. In this example, the `gasLimit` is set to \`200000.
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

**Note**: As a security measure, the `sendMessagePayLINK` function is protected by the `onlyAllowlistedDestinationChain`, ensuring the contract owner has allowlisted a destination chain.

### Transferring tokens and data and pay in native

The `sendMessagePayNative` function undertakes five primary operations:

1. Call the `_buildCCIPMessage` private function to construct a CCIP-compatible message using the `EVM2AnyMessage` [struct](/ccip/api-reference/evm/v1.6.1/client#any2evmmessage):
   - The `_receiver` address is encoded in bytes to accommodate non-EVM destination blockchains with distinct address formats. The encoding is achieved through [abi.encode](https://docs.soliditylang.org/en/develop/abi-spec.html).
   - The `data` is encoded from a `string` to `bytes` using [abi.encode](https://docs.soliditylang.org/en/develop/abi-spec.html).
   - The `tokenAmounts` is an array, with each element comprising an `EVMTokenAmount` [struct](/ccip/api-reference/evm/v1.6.1/client#evmtokenamount) containing the token address and amount. The array contains one element where the `_token` (token address) and `_amount` (token amount) are passed by the user when calling the `sendMessagePayNative` function.
   - The `extraArgs` specifies the `gasLimit` for relaying the message to the recipient contract on the destination blockchain. In this example, the `gasLimit` is set to \`200000.
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

**Note**: As a security measure, the `sendMessagePayNative` function is protected by the `onlyAllowlistedDestinationChain`, ensuring the contract owner has allowlisted a destination chain.

### Receiving messages

On the destination blockchain, the router invokes the `_ccipReceive` [function](/ccip/api-reference/evm/v1.6.1/ccip-receiver#_ccipreceive) which expects a `Any2EVMMessage` [struct](/ccip/api-reference/evm/v1.6.1/client#any2evmmessage) that contains:

- The CCIP `messageId`.
- The `sourceChainSelector`.
- The `sender` address in bytes format. Given that the sender is known to be a contract deployed on an EVM-compatible blockchain, the address is decoded from bytes to an Ethereum address using the [ABI specifications](https://docs.soliditylang.org/en/v0.8.20/abi-spec.html).
- The `tokenAmounts` is an array containing received tokens and their respective amounts. Given that only one token transfer is expected, the first element of the array is extracted.
- The `data`, which is also in bytes format. Given a `string` is expected, the data is decoded from bytes to a string using the [ABI specifications](https://docs.soliditylang.org/en/v0.8.20/abi-spec.html).

**Note**: Three important security measures are applied:

- `_ccipReceive` is called by the `ccipReceive` [function](/ccip/api-reference/evm/v1.6.1/ccip-receiver#ccipreceive), which ensures that only the router can deliver CCIP messages to the receiver contract. See the `onlyRouter` [modifier](/ccip/api-reference/evm/v1.6.1/ccip-receiver#onlyrouter) for more information.
- The modifier `onlyAllowlisted` ensures that only a call from an allowlisted source chain and sender is accepted.

> **CAUTION: Educational Example Disclaimer**
>
> This page includes an educational example to use a Chainlink system, product, or service and is provided to
> demonstrate how to interact with Chainlink's systems, products, and services to integrate them into your own. This
> template is provided "AS IS" and "AS AVAILABLE" without warranties of any kind, it has not been audited, and it may be
> missing key checks or error handling to make the usage of the system, product or service more clear. Do not use the
> code in this example in a production environment without completing your own audits and application of best practices.
> Neither Chainlink Labs, the Chainlink Foundation, nor Chainlink node operators are responsible for unintended outputs
> that are generated due to errors in code.