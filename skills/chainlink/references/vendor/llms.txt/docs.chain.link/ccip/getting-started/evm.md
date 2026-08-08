# Get Started with CCIP (EVM)
Source: https://docs.chain.link/ccip/getting-started/evm
Last Updated: 2025-12-25

> For the complete documentation index, see [llms.txt](/llms.txt).

**Build and run a secure cross-chain messaging workflow between two EVM chains using Chainlink CCIP.**

In this guide, you will:

1. Deploy a sender on a source chain
2. Deploy a receiver on a destination chain
3. Send and verify a cross-chain message

## Before you begin

> **NOTE: Please note**
>
> We use the same two smart contracts throughout the tutorial. You can check them out in the [Examine the example
> code](#examine-the-example-code) section.

You will need:

- Basic [Solidity](https://soliditylang.org/) and [smart contract deployment](/quickstarts/deploy-your-first-contract) experience

- One [wallet](https://metamask.io/) funded on two CCIP-supported EVM testnets: [Avalanche Fuji and Ethereum Sepolia](/ccip/directory/testnet). You will need some native tokens and `LINK` on both networks.

- Choose one of the following development environments:
  - **[Hardhat 3](https://hardhat.org/docs/getting-started)**
  - **[Foundry](https://book.getfoundry.sh/)**
  - **[Remix](https://remix-ide.readthedocs.io/en/latest/)**

## Examine the example code

This section goes through the code for the `sender` and `receiver` contracts
needed to complete the tutorial.
We will use the same contracts for all three development environments.

<Accordion title="Sender code" number={1}>
  The smart contract in this tutorial is designed to interact with CCIP to send data. The contract code includes comments to clarify the various functions, events, and underlying logic. However, this section explains the key elements. You can see the full contract code below.

  <CodeSample src="samples/CCIP/Sender.sol" filename="Sender.sol" />

  #### Initializing the contract

  When deploying the contract, you define the router address and the LINK contract address of the blockchain where you choose to deploy the contract.

  The router address provides functions that are required for this example:

  - The `getFee` [function](/ccip/api-reference/evm/v1.6.1/i-router-client#getfee) to estimate the CCIP fees.
  - The `ccipSend` [function](/ccip/api-reference/evm/v1.6.1/i-router-client#ccipsend) to send CCIP messages.

  #### Sending data

  The `sendMessage` function completes several operations:

  1. Construct a CCIP-compatible message using the `EVM2AnyMessage` [struct](/ccip/api-reference/evm/v1.6.1/client#evm2anymessage):
     - The `receiver` address is encoded in bytes format to accommodate non-EVM destination blockchains with distinct address formats. The encoding is achieved through [abi.encode](https://docs.soliditylang.org/en/develop/abi-spec.html).
     - The `data` is encoded from a string text to bytes using [abi.encode](https://docs.soliditylang.org/en/develop/abi-spec.html).
     - The `tokenAmounts` is an array. Each element comprises a [struct](/ccip/api-reference/evm/v1.6.1/client#evmtokenamount) that contains the token address and amount. In this example, the array is empty because no tokens are sent.
     - The `extraArgs` specify the `gasLimit` for relaying the CCIP message to the recipient contract on the destination blockchain. In this example, the `gasLimit` is set to `200000`.
     - The `feeToken` designates the token address used for CCIP fees. Here, `address(linkToken)` signifies payment in LINK.

  2. Compute the fees by invoking the router's `getFee` [function](/ccip/api-reference/evm/v1.6.1/i-router-client#getfee).

  3. Ensure that your contract balance in LINK is enough to cover the fees.

  4. Grant the router contract permission to deduct the fees from the contract's LINK balance.

  5. Dispatch the CCIP message to the destination chain by executing the router's `ccipSend` [function](/ccip/api-reference/evm/v1.6.1/i-router-client#ccipsend).

  > **CAUTION: Best Practices**
  >
  > This example is simplified for educational purposes. For production code, please adhere to the following best practices:

  - **Do Not Hardcode `extraArgs`**: In this example, `extraArgs` are hardcoded within the contract for simplicity. It is recommended to make `extraArgs` mutable. For instance, you can construct `extraArgs` off-chain and pass them into your function calls, or store them in a storage variable that can be updated as needed. This approach ensures that `extraArgs` remain backward compatible with future CCIP upgrades. Refer to the [Best Practices](/ccip/concepts/best-practices/evm) guide for more information.

  - **Validate the Destination Chain**: Always ensure that the destination chain is valid and supported before sending messages.

  - **Understand `allowOutOfOrderExecution` Usage**: This example sets `allowOutOfOrderExecution` to `true` (see [GenericExtraArgsV2](/ccip/api-reference/evm/v1.6.1/client#genericextraargsv2)). Read the [Best Practices: Setting `allowOutOfOrderExecution`](/ccip/concepts/best-practices/evm#setting-allowoutoforderexecution) to learn more about this parameter.

  - **Understand CCIP Service Limits**: Review the [CCIP Service Limits](/ccip/service-limits) for constraints on message data size, execution gas, and the number of tokens per transaction. If your requirements exceed these limits, you may need to [contact the Chainlink Labs Team](https://chain.link/ccip-contact).

  Following these best practices ensures that your contract is robust, future-proof, and compliant with CCIP standards.
</Accordion>

## Send a cross-chain message using CCIP

Send and verify a cross-chain message using CCIP in under 10 minutes, with your favorite development framework.

### Hardhat 3

Best for a `Typescript` based scripting workflow where you deploy contracts, send a CCIP message, and verify delivery from the command line.

<br />

<Accordion title="Set up the contracts" number={2}>
  1. Create a new directory named `contracts` for your smart contracts if it doesn't already exist.
  2. Create a new file named `Sender.sol` in this directory and paste the [sender contract code](#examine-the-example-code) inside it.
  3. Create a new file named `Receiver.sol` in the same directory and paste the [receiver contract code](#examine-the-example-code) inside it.
  4. Run the following command to compile the contracts:

  ```bash filename="Terminal"
  npx hardhat build
  ```
</Accordion>

<Accordion title="Verify message delivery" number={4}>
  1. Wait for a few minutes for the message to be delivered to the receiver contract.

  2. Create a new file named `verify-cross-chain-message.ts` in the `scripts` directory and paste the following code inside it:

  <Callout type="note" title="Paste the Receiver contract address here">
    The second script will call the `getLastReceivedMessageDetails` function on the receiver contract to verify if the
    message has been received. It will need the correct address to call the function.
  </Callout>

  ```typescript filename="scripts/verify-cross-chain-message.ts"
  import { network } from "hardhat"

  // Paste the Receiver contract address
  const RECEIVER_ADDRESS = ""

  console.log("Connecting to Ethereum Sepolia...")
  const sepoliaNetwork = await network.connect("sepolia")

  console.log("Checking for received message...\n")
  const receiver = await sepoliaNetwork.viem.getContractAt("Receiver", RECEIVER_ADDRESS)

  const [messageId, text] = await receiver.read.getLastReceivedMessageDetails()

  // A null hexadecimal value means no message has been received yet
  const ZERO_BYTES32 = "0x0000000000000000000000000000000000000000000000000000000000000000"

  if (messageId === ZERO_BYTES32) {
    console.log("No message received yet.")
    console.log("Please wait a bit longer and try again.")
    process.exit(1)
  } else {
    console.log(`✅ Message ID: ${messageId}`)
    console.log(`Text: "${text}"`)
  }
  ```

  This script does the following:

  - Connects to the Ethereum Sepolia network.
  - Reads the last received message details from the receiver contract.
  - Checks if any message has been received.
  - Prints the message ID and text of the last received message.

  3. Run the following command to verify the cross-chain message:

  ```bash filename="Terminal"
  npx hardhat run scripts/verify-cross-chain-message.ts
  ```

  4. You should see the message ID and text of the last received message printed in the terminal.
</Accordion>

### Foundry

Best for **Solidity-native** workflows that prefer a modular, powerful scripting framework.

<Accordion title="Bootstrap a new Foundry project" number={1}>
  1. Open a new terminal in a directory of your choice and run this command to initialize a new Foundry project at the root:

  ```bash filename="Terminal"
  forge init
  ```

  2. Install the required dependencies:

  ```bash filename="Terminal"
  forge install smartcontractkit/chainlink-ccip smartcontractkit/chainlink-evm
  ```

  > **NOTE: Note**
  >
  > `cast wallet import` will throw an error if an `env` variable with the same name already exists. Use `cast wallet
  >   list` to check previously set variables.

  3. Use Foundry's `cast` command to create a new keystore for your `PRIVATE_KEY`:

  ```bash filename="Terminal"
  cast wallet import --interactive PRIVATE_KEY
  ```

  And use the `cast wallet list` command to verify:

  ![Image](/images/ccip/tutorials/ccip-getting-started-evm-2.png)

  > **CAUTION: Note**
  >
  > You will notice that while we pulled in our RPC URLs via encrypted keystore variables in the Hardhat project, we only
  > encrypt the `PRIVATE_KEY` in Foundry. This is because as of of writing, Foundry supports keystore encryption only for
  > Private keys, and not generic strings.

  4. Configure the remappings so that your `foundry.toml` file looks like this:

  ```toml filename="foundry.toml"
  [profile.default]
  solc = "0.8.24"
  src = "src"
  out = "out"
  libs = ["lib"]

  remappings = [
    "forge-std/=lib/forge-std/src/",
    "@chainlink/contracts-ccip/contracts/=lib/chainlink-ccip/chains/evm/contracts/",
    "@chainlink/contracts/=lib/chainlink-evm/contracts/",
    "@openzeppelin/contracts@5.0.2/utils/introspection/=lib/forge-std/src/interfaces/"
  ]

  # RPC URLs will be fed to our script via Foundry's config file
  [rpc_endpoints]
  sepolia = "ENTER_YOUR_SEPOLIA_RPC_URL_HERE"
  fuji = "ENTER_YOUR_FUJI_RPC_URL_HERE"
  ```
</Accordion>

<Accordion title="Send a cross-chain message using CCIP" number={3}>
  1. Create a new directory named `script` at the root of the project if it doesn't already exist.
  2. Create a new file named `SendCrossChainMessage.s.sol` in this directory and paste the following code inside it:

  ```solidity filename="script/SendCrossChainMessage.s.sol"
  // SPDX-License-Identifier: UNLICENSED
  pragma solidity 0.8.24;

  import {Script, console} from "forge-std/Script.sol";

  import {Sender} from "../src/Sender.sol";
  import {Receiver} from "../src/Receiver.sol";
  import {LinkTokenInterface} from "@chainlink/contracts/src/v0.8/shared/interfaces/LinkTokenInterface.sol";

  contract SendCrossChainMessage is Script {
      // Avalanche Fuji configuration
      address constant FUJI_ROUTER = 0xF694E193200268f9a4868e4Aa017A0118C9a8177;
      address constant FUJI_LINK = 0x0b9d5D9136855f6FEc3c0993feE6E9CE8a297846;

      // Ethereum Sepolia configuration
      address constant SEPOLIA_ROUTER =0x0BF3dE8c5D3e8A2B34D2BEeB17ABfCeBaf363A59;
      uint64 constant SEPOLIA_CHAIN_SELECTOR = 16015286601757825753;

      // Configuring decimal value for LINK token
      uint256 ONE_LINK = 1e18;

      function run() public {

          // Load form configs from foundry.toml
          uint256 fujiFork = vm.createFork(vm.rpcUrl("fuji"));
          uint256 sepoliaFork = vm.createFork(vm.rpcUrl("sepolia"));

          // Step 1: Deploy Sender on Fuji

          // Connect to Fuji Network
          console.log("Connecting to Avalanche Fuji...");
          vm.selectFork(fujiFork);
          vm.startBroadcast();

          // Deploy Sender contract
          console.log("\n[Step 1] Deploying Sender contract on Avalanche Fuji...");
          Sender sender = new Sender(FUJI_ROUTER, FUJI_LINK);
          console.log("Sender contract has been deployed to this address on the Fuji testnet:", address(sender));
          console.log(
              string.concat(
                  "View on Avascan: https://testnet.avascan.info/blockchain/all/address/",
                  vm.toString(address(sender))
              )
          );

          // Step 2: Fund Sender with 1 LINK
          console.log("\n[Step 2] Funding Sender with 1 LINK on Avalanche Fuji...");
          LinkTokenInterface(FUJI_LINK).transfer(address(sender), ONE_LINK);
          vm.stopBroadcast();
          console.log("Funded Sender with 1 LINK on Fuji");

          // Step 3: Deploy Receiver on Sepolia

          // Connect to Sepolia Network
          console.log("Connecting to Ethereum Sepolia...");
          vm.selectFork(sepoliaFork);
          vm.startBroadcast();

          // Deploy Receiver contract

          console.log("\n[Step 3] Deploying Receiver contract on Ethereum Sepolia...");
          Receiver receiver = new Receiver(SEPOLIA_ROUTER);
          vm.stopBroadcast();
          console.log("Receiver deployed on Sepolia at this address:", address(receiver));
          console.log(
              string.concat(
                  "View on Etherscan: https://sepolia.etherscan.io/address/",
                  vm.toString(address(receiver))
              )
          );
          console.log("\n .....Copy the receiver address since it will be needed to run the verification script.....\n");
          console.log(address(receiver));

          // Step 4: Send cross-chain message (Fuji -> Sepolia)
          vm.selectFork(fujiFork);
          vm.startBroadcast();

          // Send cross-chain message
          console.log("Sending cross-chain message from Fuji to Sepolia...");
          bytes32 messageId = sender.sendMessage(
              SEPOLIA_CHAIN_SELECTOR,
              address(receiver),
              "Hello World from Foundry script!"
          );
          vm.stopBroadcast();

          console.log("The message has been sent to the CCIP router on Fuji, check for successful delivery after 5 minutes...");
          console.log("CCIP messageId:");
          console.logBytes32(messageId);
          console.log("View transaction status on CCIP Explorer: https://ccip.chain.link");
      }
  }
  ```

  3. Run the following command to send the cross-chain message:

  ```bash filename="Terminal"
  forge script script/SendCrossChainMessage.s.sol:SendCrossChainMessage --broadcast --multi --account PRIVATE_KEY
  ```

  > **NOTE**
  >
  > - The `--broadcast` flag is used to broadcast transactions to an actual network.

  - You can get detailed runtime logs by using the `-vvvv` verbosity flag. - The `--multi` flag is used to send transactions to multiple chains.
</Accordion>

### Remix

Best for **Web3-native** workflows that prefer a browser-based IDE.

<Accordion title="Deploy the sender contract" number={1}>
  Deploy the `Sender.sol` contract on *Avalanche Fuji*. To see a detailed explanation of this contract, read the [Code Explanation](#sender-code) section.

  1. Open the Sender.sol contract in Remix.

  2. Compile the contract.

  3. Deploy the sender contract on *Avalanche Fuji*:
     1. Open MetaMask and select the *Avalanche Fuji* network.

     2. In Remix under the **Deploy & Run Transactions** tab, select *Injected Provider - MetaMask* in the **Environment** list. Remix will use the MetaMask wallet to communicate with *Avalanche Fuji*.

     3. Under the **Deploy** section, fill in the router address and the LINK token contract addresses for your specific blockchain. You can find both of these addresses on the [CCIP Directory](/ccip/directory). The LINK token contract address is also listed on the [LINK Token Contracts](/resources/link-token-contracts) page. For *Avalanche Fuji*, the router address is <CopyText text="0xF694E193200268f9a4868e4Aa017A0118C9a8177" code /> and the LINK address is <CopyText text="0x0b9d5D9136855f6FEc3c0993feE6E9CE8a297846" code />.

        ![Image](/images/ccip/tutorials/deploy-sender-avalanche-fuji.webp)

     4. Click the **transact** button to deploy the contract. MetaMask prompts you to confirm the transaction. Check the transaction details to make sure you are deploying the contract to *Avalanche Fuji*.

     5. After you confirm the transaction, the contract address appears in the **Deployed Contracts** list. Copy your contract address.

        ![Image](/images/ccip/tutorials/deployed-sender-avalanche-fuji.webp)

     6. Open MetaMask and send <CopyText text="70" code /> LINK to the contract address that you copied. Your contract will pay CCIP fees in LINK.

        **Note:** This transaction fee is significantly higher than normal due to gas spikes on Sepolia. To run this example, you can get additional testnet LINK
        from [faucets.chain.link](https://faucets.chain.link) or use a supported testnet other than Sepolia.
</Accordion>

<Accordion title="Send data" number={3}>
  Send a `Hello World!` string from your contract on *Avalanche Fuji* to the contract you deployed on *Ethereum Sepolia*:

  1. Open MetaMask and select the *Avalanche Fuji* network.

  2. In Remix under the **Deploy & Run Transactions** tab, expand the first contract in the **Deployed Contracts** section.

  3. Expand the **sendMessage** function and fill in the following arguments:

     | Argument                 | Description                                                                                                                         | Value (*Ethereum Sepolia*)                    |
     | ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
     | destinationChainSelector | CCIP Chain identifier of the target blockchain. You can find each network's chain selector on the [CCIP Directory](/ccip/directory) | <CopyText text="16015286601757825753" code /> |
     | receiver                 | The destination smart contract address                                                                                              | Your deployed contract address                |
     | text                     | Any `string`                                                                                                                        | <CopyText text="Hello World!" code />         |

     ![Image](/images/ccip/tutorials/fuji-sendmessage.webp)

  4. Click the **transact** button to run the function. MetaMask prompts you to confirm the transaction.

  <Aside type="note" title="Gas price spikes">
    Under normal circumstances, transactions on the Ethereum Sepolia network require significantly fewer tokens to pay for gas. However, during exceptional periods of high gas price spikes, your transactions may fail if not sufficiently funded. In such cases, you may need to fund your contract with additional tokens. We recommend paying for your CCIP transactions in **LINK** tokens (rather than native tokens) as you can obtain extra LINK testnet tokens from [faucets.chain.link](https://faucets.chain.link/). If you encounter a transaction failure due to these gas price spikes, please add additional LINK tokens to your contract and try again.
    Alternatively, you can use a supported testnet other than Sepolia.
  </Aside>

  5. After the transaction is successful, note the transaction hash. Here is an [example](https://testnet.snowtrace.io/tx/0x113933ec9f1b2e795a1e2f564c9d452db92d3e9a150545712687eb546916e633) of a successful transaction on *Avalanche Fuji*.

  After the transaction is finalized on the source chain, it will take a few minutes for CCIP to deliver the data to *Ethereum Sepolia* and call the `ccipReceive` function on your receiver contract. You can use the [CCIP explorer](https://ccip.chain.link/) to see the status of your CCIP transaction and then read data stored by your receiver contract.

  6. Open the [CCIP explorer](https://ccip.chain.link/) and use the transaction hash that you copied to search for your cross-chain transaction. The explorer provides several details about your request.

     ![Image](/images/ccip/tutorials/ccip-explorer-tx-details.webp)

  7. When the status of the transaction is marked with a "Success" status, the CCIP transaction and the destination transaction are complete.

     ![Image](/images/ccip/tutorials/ccip-explorer-tx-details-success.webp)
</Accordion>