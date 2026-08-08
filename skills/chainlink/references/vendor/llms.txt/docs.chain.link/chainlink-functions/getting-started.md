# Getting Started
Source: https://docs.chain.link/chainlink-functions/getting-started

> For the complete documentation index, see [llms.txt](/llms.txt).

<ChainlinkFunctions callout="shutdown" />

Learn how to make requests to the Chainlink Functions Decentralized Oracle Network (DON) and make any computation or API calls offchain. Chainlink Functions is available on several blockchains (see the [supported networks page](/chainlink-functions/supported-networks)), but this guide uses Sepolia to simplify access to testnet funds. Complete the following tasks to get started with Chainlink Functions:

- Set up your web3 wallet and fund it with testnet tokens.
- Simulate a Chainlink Functions on the [Chainlink Functions
  Playground](https://functions.chain.link/playground).
- Send a Chainlink Functions request to the DON. The JavaScript source code makes an API call to the [Star Wars API](https://swapi.info) and fetches the name of a given character.
- Receive the response from Chainlink Functions and parse the result.

## Simulation

Before making a Chainlink Functions request from your smart contract, it is always a good practice to simulate the source code offchain to make any adjustments or corrections.

1. Open the [Functions playground](https://functions.chain.link/playground).

2. Copy and paste the following source code into the playground's code block.

   ```js
   const characterId = args[0];
   const apiResponse = await Functions.makeHttpRequest({
     url: `https://swapi.info/api/people/${characterId}/`,
   });
   if (apiResponse.error) {
     throw Error("Request failed");
   }
   const { data } = apiResponse;
   return Functions.encodeString(data.name);
   ```

3. Under *Argument*, set the first argument to 1. You are going to fetch the name of the first Star Wars character.

4. Click on *Run code*. Under *Output*, you should see *Luke Skywalker*.

   (Image: Image)

## Configure your resources

### Configure your wallet

You will test on Sepolia, so you must have an Ethereum web3 wallet with enough testnet ETH and LINK tokens. Testnet ETH is the native gas fee token on Sepolia. You will use testnet ETH tokens to pay for gas whenever you make a transaction on Sepolia. On the other hand, you will use LINK tokens to pay the Chainlink Functions Decentralized Oracles Network (DON) for processing your request.

1. [Install the MetaMask wallet](/quickstarts/deploy-your-first-contract#install-and-fund-your-metamask-wallet) or other Ethereum web3 wallet.

2. Set the network for your wallet to the Sepolia testnet. If you need to add Sepolia to your wallet, you can find the chain ID and the LINK token contract address on the [LINK Token Contracts](/resources/link-token-contracts#sepolia-testnet) page.
   -

3. Request testnet LINK and ETH from [faucets.chain.link/sepolia](https://faucets.chain.link/sepolia).

### Deploy a Functions consumer contract on Sepolia

1. Open the [GettingStartedFunctionsConsumer.sol](https://remix.ethereum.org/#url=https://docs.chain.link/samples/ChainlinkFunctions/GettingStartedFunctionsConsumer.sol) contract in Remix.

   [Open GettingStartedFunctionsConsumer.sol in Remix](https://remix.ethereum.org/#url=https://docs.chain.link/samples/ChainlinkFunctions/GettingStartedFunctionsConsumer.sol)

2. Compile the contract.

3. Open MetaMask and select the *Sepolia* network.

4. In Remix under the **Deploy & Run Transactions** tab, select *Injected Provider - MetaMask* in the **Environment** list. Remix will use the MetaMask wallet to communicate with *Sepolia*.

   (Image: Image)

5. Click the **Deploy** button to deploy the contract. MetaMask prompts you to confirm the transaction. Check the transaction details to make sure you are deploying the contract to *Sepolia*.

   (Image: Image)

6. After you confirm the transaction, the contract address appears in the **Deployed Contracts** list. Copy the contract address and save it for later. You will use this address with a Functions Subscription.

   (Image: Image)

### Create a subscription

You use a Chainlink Functions subscription to pay for, manage, and track Functions requests.

1. Go to [functions.chain.link](https://functions.chain.link/).

2. Click **Connect wallet**:

   (Image: Image)

3. Read and accept the Chainlink Foundation Terms of Service. Then click **MetaMask**.

   (Image: Image)

4. Make sure your wallet is connected to the *Sepolia* testnet. If not, click the network name in the top right corner of the page and select *Sepolia*.

   (Image: Image)

5. Click **Create Subscription**:

   (Image: Image)

6. Provide an email address and an optional subscription name:

   (Image: Image)

7. The first time you interact with the Subscription Manager using your EOA, you must accept the Terms of Service (ToS). A MetaMask popup appears and you are asked to accept the ToS:

   (Image: Image)

8. After you approve the ToS, another MetaMask popup appears, and you are asked to approve the subscription creation:

   (Image: Image)

9. After the subscription is created, MetaMask prompts you to sign a message that links the subscription name and email address to your subscription:

   (Image: Image)

   (Image: Image)

### Fund your subscription

1. After the subscription is created, the Functions UI prompts you to fund your subscription. Click **Add funds**:

   (Image: Image)

2. For this example, add 2 LINK and click **Add funds**:

   (Image: Image)

### Add a consumer to your subscription

1. After you fund your subscription, add your consumer to it. Specify the address for the consumer contract that you deployed earlier and click **Add consumer**. MetaMask prompts you to confirm the transaction.

   (Image: Image)

   (Image: Image)

2. Subscription creation and configuration is complete. You can always see the details of your subscription again at [functions.chain.link](https://functions.chain.link):

   (Image: Image)

## Run the example

The example is hardcoded to communicate with Chainlink Functions on Sepolia. After this example is run, you can examine the code and see a detailed description of all components.

1. In Remix under the **Deploy & Run Transactions** tab, expand your contract in the **Deployed Contracts** section.

2. Expand the `sendRequest` function to display its parameters.

3. Fill in the `subscriptionId` with your subscription ID and `args` with `[1]`. You can find your subscription ID on the Chainlink Functions Subscription Manager at [functions.chain.link](https://functions.chain.link/). The `[1]` value for `args` specifies which argument in the response will be retrieved.

4. Click the **transact** button.

   (Image: Image)

5. Wait for the request to be fulfilled. You can monitor the status of your request on the Chainlink Functions Subscription Manager.

   (Image: Image)

6. Refresh the Functions UI to get the latest request status.

7. After the status is *Success*, check the character name. In Remix, under the **Deploy & Run Transactions** tab, click the `character` function. If the transaction and request ran correctly, you will see the name of your character in the response.

   (Image: Image)

Chainlink Functions is capable of much more than just retrieving data. Try one of the [Tutorials](/chainlink-functions/tutorials) to see examples that can GET and POST to public APIs, securely handle API secrets, handle custom responses, and query multiple APIs.

## Examine the code

### Solidity code

```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {FunctionsClient} from "@chainlink/contracts/src/v0.8/functions/v1_0_0/FunctionsClient.sol";

import {FunctionsRequest} from "@chainlink/contracts/src/v0.8/functions/v1_0_0/libraries/FunctionsRequest.sol";
import {ConfirmedOwner} from "@chainlink/contracts/src/v0.8/shared/access/ConfirmedOwner.sol";

/**
 * Request testnet LINK and ETH here: https://faucets.chain.link/
 * Find information on LINK Token Contracts and get the latest ETH and LINK faucets here:
 * https://docs.chain.link/resources/link-token-contracts/
 */

/**
 * @title GettingStartedFunctionsConsumer
 * @notice This is an example contract to show how to make HTTP requests using Chainlink
 * @dev This contract uses hardcoded values and should not be used in production.
 */
contract GettingStartedFunctionsConsumer is FunctionsClient, ConfirmedOwner {
  using FunctionsRequest for FunctionsRequest.Request;

  // State variables to store the last request ID, response, and error
  bytes32 public s_lastRequestId;
  bytes public s_lastResponse;
  bytes public s_lastError;

  // Custom error type
  error UnexpectedRequestID(bytes32 requestId);

  // Event to log responses
  event Response(bytes32 indexed requestId, string character, bytes response, bytes err);

  // Router address - Hardcoded for Sepolia
  // Check to get the router address for your supported network
  // https://docs.chain.link/chainlink-functions/supported-networks
  address router = 0xb83E47C2bC239B3bf370bc41e1459A34b41238D0;

  // JavaScript source code
  // Fetch character name from the Star Wars API.
  // Documentation: https://swapi.info/people
  string source = "const characterId = args[0];" "const apiResponse = await Functions.makeHttpRequest({"
    "url: `https://swapi.info/api/people/${characterId}/`" "});" "if (apiResponse.error) {"
    "throw Error('Request failed');" "}" "const { data } = apiResponse;" "return Functions.encodeString(data.name);";

  //Callback gas limit
  uint32 gasLimit = 300_000;

  // donID - Hardcoded for Sepolia
  // Check to get the donID for your supported network https://docs.chain.link/chainlink-functions/supported-networks
  bytes32 donID = 0x66756e2d657468657265756d2d7365706f6c69612d3100000000000000000000;

  // State variable to store the returned character information
  string public character;

  /**
   * @notice Initializes the contract with the Chainlink router address and sets the contract owner
   */
  constructor() FunctionsClient(router) ConfirmedOwner(msg.sender) {}

  /**
   * @notice Sends an HTTP request for character information
   * @param subscriptionId The ID for the Chainlink subscription
   * @param args The arguments to pass to the HTTP request
   * @return requestId The ID of the request
   */
  function sendRequest(
    uint64 subscriptionId,
    string[] calldata args
  ) external onlyOwner returns (bytes32 requestId) {
    FunctionsRequest.Request memory req;
    req.initializeRequestForInlineJavaScript(source); // Initialize the request with JS code
    if (args.length > 0) req.setArgs(args); // Set the arguments for the request

    // Send the request and store the request ID
    s_lastRequestId = _sendRequest(req.encodeCBOR(), subscriptionId, gasLimit, donID);

    return s_lastRequestId;
  }

  /**
   * @notice Callback function for fulfilling a request
   * @param requestId The ID of the request to fulfill
   * @param response The HTTP response data
   * @param err Any errors from the Functions request
   */
  function fulfillRequest(
    bytes32 requestId,
    bytes memory response,
    bytes memory err
  ) internal override {
    if (s_lastRequestId != requestId) {
      revert UnexpectedRequestID(requestId); // Check if request IDs match
    }
    // Update the contract's state variables with the response and any errors
    s_lastResponse = response;
    character = string(response);
    s_lastError = err;

    // Emit an event to log the response
    emit Response(requestId, character, s_lastResponse, s_lastError);
  }
}
```

- To write a Chainlink Functions consumer contract, your contract must import [FunctionsClient.sol](https://github.com/smartcontractkit/chainlink/blob/contracts-v1.3.0/contracts/src/v0.8/functions/v1_0_0/FunctionsClient.sol) and [FunctionsRequest.sol](https://github.com/smartcontractkit/chainlink/blob/contracts-v1.3.0/contracts/src/v0.8/functions/v1_0_0/libraries/FunctionsRequest.sol). You can read the API references: [FunctionsClient](/chainlink-functions/api-reference/functions-client) and [FunctionsRequest](/chainlink-functions/api-reference/functions-request).

  These contracts are available in an NPM package so that you can import them from within your project.

  ```
  import {FunctionsClient} from "@chainlink/contracts/src/v0.8/functions/v1_0_0/FunctionsClient.sol";
  import {FunctionsRequest} from "@chainlink/contracts/src/v0.8/functions/v1_0_0/libraries/FunctionsRequest.sol";
  ```

- Use the FunctionsRequest.sol library to get all the functions needed for building a Chainlink Functions request.

  ```
  using FunctionsRequest for FunctionsRequest.Request;
  ```

- The latest request ID, latest received response, and latest received error (if any) are defined as state variables:

  ```
  bytes32 public s_lastRequestId;
  bytes public s_lastResponse;
  bytes public s_lastError;
  ```

- We define the `Response` event that your smart contract will emit during the callback

  ```
  event Response(bytes32 indexed requestId, string character, bytes response, bytes err);
  ```

- The Chainlink Functions router address and donID are hardcoded for Sepolia. Check the [supported networks page](/chainlink-functions/supported-networks) to try the code sample on another testnet.

- The `gasLimit` is hardcoded to `300000`, the amount of gas that Chainlink Functions will use to fulfill your request.

- The JavaScript source code is hardcoded in the `source` state variable. For more explanation, read the [JavaScript code section](#javascript-code).

- Pass the router address for your network when you deploy the contract:

  ```
  constructor() FunctionsClient(router)
  ```

- The two remaining functions are:
  - `sendRequest` for sending a request. It receives the subscription ID and list of arguments to pass to the source code. Then:
    - It uses the `FunctionsRequest` library to initialize the request and add the source code and arguments. You can read the API Reference for [Initializing a request](/chainlink-functions/api-reference/functions-request/#initializerequestforinlinejavascript) and [adding arguments](/chainlink-functions/api-reference/functions-request/#setargs).

      ```
      FunctionsRequest.Request memory req;
      req.initializeRequestForInlineJavaScript(source);
      if (args.length > 0) req.setArgs(args);
      ```

    - It sends the request to the router by calling the `FunctionsClient` `sendRequest` function. You can read the API reference for [sending a request](/chainlink-functions/api-reference/functions-client/#_sendrequest). Finally, it stores the request id in `s_lastRequestId` and returns it.

      ```
      s_lastRequestId = _sendRequest(
          req.encodeCBOR(),
          subscriptionId,
          gasLimit,
          jobId
      );
      return s_lastRequestId;
      ```

      **Note**: `_sendRequest` accepts requests encoded in `bytes`. Therefore, you must encode it using [encodeCBOR](/chainlink-functions/api-reference/functions-request/#encodecbor).

- `fulfillRequest` to be invoked during the callback. This function is defined in `FunctionsClient` as `virtual` (read `fulfillRequest` [API reference](/chainlink-functions/api-reference/functions-client/#fulfillrequest)). So, your smart contract must override the function to implement the callback. The implementation of the callback is straightforward: the contract stores the latest response and error in `s_lastResponse` and `s_lastError`, parses the `response` from `bytes` to `string` to fetch the character name before emitting the `Response` event.

  ```
  s_lastResponse = response;
  character = string(response);
  s_lastError = err;
  emit Response(requestId, s_lastResponse, s_lastError);
  ```

### JavaScript code

```js
const characterId = args[0];
const apiResponse = await Functions.makeHttpRequest({
  url: `https://swapi.info/api/people/${characterId}/`,
});
if (apiResponse.error) {
  throw Error("Request failed");
}
const { data } = apiResponse;
return Functions.encodeString(data.name);
```

This JavaScript source code uses [Functions.makeHttpRequest](/chainlink-functions/api-reference/javascript-source#http-requests) to make HTTP requests. The source code calls the `https://swapi.info/` API to request a Star Wars character name. If you read the [Functions.makeHttpRequest](/chainlink-functions/api-reference/javascript-source#http-requests) documentation and the [Star Wars API documentation](https://swapi.info/people), you notice that URL has the following format where `$characterId` is provided as parameter when making the HTTP request:

```
url: `https://swapi.info/api/people/${characterId}/`
```

To check the expected API response for the first character, you can directly paste the following URL in your browser `https://swapi.info/api/people/1/` or run the `curl` command in your terminal:

```bash
curl -X 'GET' \
  'https://swapi.info/api/people/1/' \
  -H 'accept: application/json'
```

The response should be similar to the following example:

```json
{
  "name": "Luke Skywalker",
  "height": "172",
  "mass": "77",
  "hair_color": "blond",
  "skin_color": "fair",
  "eye_color": "blue",
  "birth_year": "19BBY",
  "gender": "male",
  "homeworld": "https://swapi.info/api/planets/1/",
  "films": [
    "https://swapi.info/api/films/1/",
    "https://swapi.info/api/films/2/",
    "https://swapi.info/api/films/3/",
    "https://swapi.info/api/films/6/"
  ],
  "species": [],
  "vehicles": ["https://swapi.info/api/vehicles/14/", "https://swapi.info/api/vehicles/30/"],
  "starships": ["https://swapi.info/api/starships/12/", "https://swapi.info/api/starships/22/"],
  "created": "2014-12-09T13:50:51.644000Z",
  "edited": "2014-12-20T21:17:56.891000Z",
  "url": "https://swapi.info/api/people/1/"
}
```

Now that you understand the structure of the API. Let's delve into the JavaScript code. The main steps are:

- Fetch `characterId` from `args`. Args is an array. The `characterId` is located in the first element.
- Make the HTTP call using `Functions.makeHttpRequest` and store the response in `apiResponse`.
- Throw an error if the call is not successful.
- The API response is located at `data`.
- Read the name from the API response `data.name` and return the result as a [buffer](https://nodejs.org/api/buffer.html#buffer) using the `Functions.encodeString` helper function. Because the `name` is a `string`, we use `encodeString`. For other data types, you can use different [data encoding functions](/chainlink-functions/api-reference/javascript-source#data-encoding-functions).
  **Note**: Read this [article](https://www.freecodecamp.org/news/do-you-want-a-better-understanding-of-buffer-in-node-js-check-this-out-2e29de2968e8/) if you are new to Javascript Buffers and want to understand why they are important.