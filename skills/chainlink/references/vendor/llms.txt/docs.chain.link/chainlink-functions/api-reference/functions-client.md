# FunctionsClient API Reference
Source: https://docs.chain.link/chainlink-functions/api-reference/functions-client

> For the complete documentation index, see [llms.txt](/llms.txt).

<ChainlinkFunctions callout="shutdown" />

> \*\*NOTE: Add Chainlink to your project\*\*
>
>
>
> If you need to integrate Chainlink into your project, install the [@chainlink/contracts NPM package](https://www.npmjs.com/package/@chainlink/contracts).
>
> - If you use [NPM](https://www.npmjs.com/): npm install @chainlink/contracts --save
>
> - If you use [Yarn](https://yarnpkg.com/): yarn add @chainlink/contracts
>
> Functions contracts are available starting from version *0.7.1*.

Consumer contract developers inherit [FunctionsClient](https://github.com/smartcontractkit/chainlink/blob/contracts-v1.3.0/contracts/src/v0.8/functions/v1_0_0/FunctionsClient.sol) to create Chainlink Functions requests.

## Events

### RequestSent

```solidity
event RequestSent(bytes32 id)
```

### RequestFulfilled

```solidity
event RequestFulfilled(bytes32 id)
```

## Errors

### OnlyRouterCanFulfill

```solidity
error OnlyRouterCanFulfill()
```

## Methods

### constructor

```solidity
constructor(address router)
```

### \_sendRequest

```solidity
function _sendRequest(bytes data, uint64 subscriptionId, uint32 callbackGasLimit, bytes32 donId) internal returns (bytes32)
```

Sends a Chainlink Functions request to the stored router address

#### Parameters

| Name             | Type    | Description                                                           |
| ---------------- | ------- | --------------------------------------------------------------------- |
| data             | bytes   | The CBOR encoded bytes data for a Functions request                   |
| subscriptionId   | uint64  | The subscription ID that will be charged to service the request       |
| callbackGasLimit | uint32  | the amount of gas that will be available for the fulfillment callback |
| donId            | bytes32 |                                                                       |

#### Return Values

| Name | Type    | Description                                         |
| ---- | ------- | --------------------------------------------------- |
| \[0] | bytes32 | requestId The generated request ID for this request |

### fulfillRequest

```solidity
function fulfillRequest(bytes32 requestId, bytes response, bytes err) internal virtual
```

User defined function to handle a response from the DON

*Either response or error parameter will be set, but never both*

#### Parameters

| Name      | Type    | Description                                                                         |
| --------- | ------- | ----------------------------------------------------------------------------------- |
| requestId | bytes32 | The request ID, returned by sendRequest()                                           |
| response  | bytes   | Aggregated response from the execution of the user's source code                    |
| err       | bytes   | Aggregated error from the execution of the user code or from the execution pipeline |

### handleOracleFulfillment

```solidity
function handleOracleFulfillment(bytes32 requestId, bytes response, bytes err) external
```

Chainlink Functions response handler called by the Functions Router during fulfillment from the designated transmitter node in an OCR round

*Either response or error parameter will be set, but never both*

#### Parameters

| Name      | Type    | Description                                                                            |
| --------- | ------- | -------------------------------------------------------------------------------------- |
| requestId | bytes32 | The requestId returned by FunctionsClient.sendRequest().                               |
| response  | bytes   | Aggregated response from the request's source code.                                    |
| err       | bytes   | Aggregated error either from the request's source code or from the execution pipeline. |