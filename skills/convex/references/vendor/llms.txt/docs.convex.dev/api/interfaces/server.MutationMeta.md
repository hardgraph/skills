# Interface: MutationMeta

> For AI agents: see [llms.txt](/llms.txt) for the complete documentation index. Markdown versions are available by adding .md to a page URL or requesting Accept: text/markdown.

[server](/api/modules/server.md).MutationMeta

Extra context available in Convex mutation functions.

## Hierarchy[​](#hierarchy "Direct link to Hierarchy")

* [`QueryMeta`](/api/interfaces/server.QueryMeta.md)

  ↳ **`MutationMeta`**

## Methods[​](#methods "Direct link to Methods")

### getFunctionMetadata[​](#getfunctionmetadata "Direct link to getFunctionMetadata")

▸ **getFunctionMetadata**(): `Promise`<[`FunctionMetadata`](/api/modules/server.md#functionmetadata)>

#### Returns[​](#returns "Direct link to Returns")

`Promise`<[`FunctionMetadata`](/api/modules/server.md#functionmetadata)>

#### Inherited from[​](#inherited-from "Direct link to Inherited from")

[QueryMeta](/api/interfaces/server.QueryMeta.md).[getFunctionMetadata](/api/interfaces/server.QueryMeta.md#getfunctionmetadata)

#### Defined in[​](#defined-in "Direct link to Defined in")

[server/meta.ts:133](https://github.com/get-convex/convex-js/blob/main/src/server/meta.ts#L133)

***

### getTransactionMetrics[​](#gettransactionmetrics "Direct link to getTransactionMetrics")

▸ **getTransactionMetrics**(): `Promise`<[`TransactionMetrics`](/api/modules/server.md#transactionmetrics)>

#### Returns[​](#returns-1 "Direct link to Returns")

`Promise`<[`TransactionMetrics`](/api/modules/server.md#transactionmetrics)>

#### Inherited from[​](#inherited-from-1 "Direct link to Inherited from")

[QueryMeta](/api/interfaces/server.QueryMeta.md).[getTransactionMetrics](/api/interfaces/server.QueryMeta.md#gettransactionmetrics)

#### Defined in[​](#defined-in-1 "Direct link to Defined in")

[server/meta.ts:134](https://github.com/get-convex/convex-js/blob/main/src/server/meta.ts#L134)

***

### getDeploymentMetadata[​](#getdeploymentmetadata "Direct link to getDeploymentMetadata")

▸ **getDeploymentMetadata**(): `Promise`<[`DeploymentMetadata`](/api/modules/server.md#deploymentmetadata)>

#### Returns[​](#returns-2 "Direct link to Returns")

`Promise`<[`DeploymentMetadata`](/api/modules/server.md#deploymentmetadata)>

#### Inherited from[​](#inherited-from-2 "Direct link to Inherited from")

[QueryMeta](/api/interfaces/server.QueryMeta.md).[getDeploymentMetadata](/api/interfaces/server.QueryMeta.md#getdeploymentmetadata)

#### Defined in[​](#defined-in-2 "Direct link to Defined in")

[server/meta.ts:135](https://github.com/get-convex/convex-js/blob/main/src/server/meta.ts#L135)

***

### getRequestMetadata[​](#getrequestmetadata "Direct link to getRequestMetadata")

▸ **getRequestMetadata**(): `Promise`<[`RequestMetadata`](/api/modules/server.md#requestmetadata)>

#### Returns[​](#returns-3 "Direct link to Returns")

`Promise`<[`RequestMetadata`](/api/modules/server.md#requestmetadata)>

#### Defined in[​](#defined-in-3 "Direct link to Defined in")

[server/meta.ts:144](https://github.com/get-convex/convex-js/blob/main/src/server/meta.ts#L144)
